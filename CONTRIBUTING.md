# Contributing to PayDev Docs

Thanks for taking the time to contribute. This is a single-file reference site — contributions are straightforward but there are a few things worth knowing before you start.

---

## What to contribute

### High-value contributions
- **Factual corrections** — scheme rules, response code descriptions, standard references that are wrong or outdated
- **Content gaps** — scenarios or edge cases a developer would realistically encounter that aren't covered
- **Diagram improvements** — clearer or more accurate SVG diagrams for existing content
- **New sections** — well-scoped additions (e.g. SoftPOS/CPOC flow, MoneySend/disbursements, open loop transit)

### Lower priority
- Typo fixes (fine to submit, but don't open an issue just for a typo — just send the PR)
- Visual polish without content improvement
- Adding external links (the site is intentionally self-contained)

### Out of scope
- PSP-specific content (Stripe, Adyen, Braintree APIs)
- Open banking / A2A / SEPA / Faster Payments
- Crypto payments
- Content requiring access to non-public scheme documentation

---

## How to contribute

### For small corrections (text, data, response codes)

1. Fork the repo
2. Edit `index.html` directly — find the relevant section by searching for the section title or section ID (e.g. `id="flow-sec-auth"`)
3. Open a pull request with a clear description of what you changed and why

### For new sections or significant additions

1. Open an issue first describing what you want to add
2. Wait for a brief discussion — avoids duplicate work and scope creep
3. Fork, implement, and open a PR referencing the issue

---

## Technical notes

### Single-file architecture

The entire site is `index.html`. There is no build step, no npm, no bundler. Everything — HTML, CSS, JS, SVG diagrams — lives in this one file.

### Section structure

Each tab with sub-navigation uses this pattern:

```html
<!-- Section div -->
<div class="flow-section" id="{prefix}-sec-{name}" style="display:none">
  <span class="sec-anchor" id="{prefix}-{name}"></span>
  <div class="section-title">Section heading</div>
  <!-- content -->
</div><!-- end {prefix}-sec-{name} -->
```

The `prefix` maps to the page via `PAGE_ID_MAP` in the JS:

```js
const PAGE_ID_MAP = {
  'flow': 'flow', 'tok': 'tokens', 'pci': 'pci',
  'fraud': 'fraud', 'tds': '3ds', 'cp': 'cardpresent',
  'std': 'standards', 'disp': 'disputes', 'app': 'appendix',
};
```

**Critical rule:** every `flow-section` div must be self-contained — the number of `<div` opens must equal the number of `</div>` closes within the section boundary (i.e. between its opening tag and the next section's opening tag). An imbalanced section will break the `showSection()` toggle.

You can verify balance with:

```python
import re

with open('index.html') as f:
    html = f.read()

# Check a specific section
sec_id = 'flow-sec-auth'
next_id = 'flow-sec-iso'
pa = html.find(f'id="{sec_id}"')
pb = html.find(f'id="{next_id}"')
chunk = html[pa:pb]
opens  = len(re.findall(r'<div\b', chunk))
closes = len(re.findall(r'</div>', chunk))
print(f'{sec_id}: +{opens} -{closes} diff={opens - closes}')  # should be 0
```

### Adding a new sub-nav section to an existing tab

1. Add a button to the tab's `<div class="sub-nav">`:
   ```html
   <button class="sub-link" onclick="showSection('flow','newname',this)">Button label</button>
   ```

2. Add the section div **before** the `</div><!-- end {prefix}-sec-{last} -->` close of the last existing section, or after it if you want it at the end:
   ```html
   <div class="flow-section" id="flow-sec-newname" style="display:none">
     <span class="sec-anchor" id="flow-newname"></span>
     <div class="section-title">Your section heading</div>
     <!-- content -->
   </div><!-- end flow-sec-newname -->
   ```

3. Verify div balance (see above)

### SVG diagrams

SVG diagrams in this site:
- Use `var(--accent)`, `var(--muted)`, `var(--text)`, `var(--bg2)`, etc. — never hardcoded hex colours — so they work in both dark and light mode
- Are `viewBox`-based with `style="width:100%"` for responsiveness
- Use `font-family:var(--mono)` for all text
- Include `<defs>` arrow markers when needed

Wrap SVGs in:
```html
<div style="background:var(--bg2);border:1px solid var(--border);border-radius:10px;padding:20px 24px;margin-bottom:24px;overflow-x:auto">
  <svg viewBox="0 0 860 200" xmlns="http://www.w3.org/2000/svg" style="width:100%;min-width:560px;font-family:var(--mono)">
    <!-- ... -->
  </svg>
</div>
```

### CSS variables reference

```
--bg, --bg2, --bg3          Background layers (dark to slightly lighter)
--border, --border2          Border colours (subtle to slightly stronger)
--text, --muted              Primary and secondary text
--accent                     Teal (#00d4aa dark / #00a882 light)
--accent2                    Blue (#3b82f6)
--accent3                    Amber (#f59e0b)
--red, --green, --orange, --purple    Status/scheme colours
--mono, --sans               Font families
```

---

## Content standards

### Accuracy
- Cite the relevant standard or source in a comment if adding factual content
- If you're not certain something is correct, flag it in the PR description
- Scheme rules change — date-sensitive content should note the version (e.g. "as of PCI DSS v4.0.1, June 2024")

### Tone
- Written for developers, not compliance officers
- Direct and specific — "Field 39 value 51 means the issuer declined due to insufficient funds" not "a decline may occur"
- No marketing language
- No hedging that adds no information

### What not to reproduce
- Do not reproduce text from proprietary scheme rulebooks (Visa Core Rules, Mastercard Rules, etc.)
- Do not reproduce text from PCI SSC standards documents verbatim
- Explain concepts in original language, cite the source

---

## Pull request checklist

Before submitting:

- [ ] `index.html` opens correctly in a browser with no console errors
- [ ] Dark mode and light mode both look correct for your changes
- [ ] Sub-nav toggles work correctly for any modified tabs (click each button, verify content changes)
- [ ] No hardcoded hex colours in new SVG diagrams (use CSS variables)
- [ ] Div balance verified for any new or modified sections
- [ ] PR description explains what changed and links to the source/standard if applicable

---

## Reporting issues

Use GitHub Issues for:
- Factual errors (with a source reference if possible)
- Broken functionality (sub-nav not toggling, search not working, etc.)
- Missing content suggestions

Label guide:
- `bug` — something is broken
- `content` — factual error or missing content
- `enhancement` — new feature or section
- `good first issue` — straightforward fix, well-scoped
