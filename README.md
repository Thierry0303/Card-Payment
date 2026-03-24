# PayDev Docs

**Card payment infrastructure explained for developers.**

A single-file, open-source reference covering everything a developer needs to know about how card payments actually work — from the physical tap to settlement, authentication, tokenisation, fraud, chargebacks, and compliance.

🔗 **Live site:** [thierry0303.github.io/paydev-docs](https://thierry0303.github.io/paydev-docs)

---

## What's inside

| Tab | Sections | What it covers |
|---|---|---|
| **Overview** | — | 4-party model diagram, core concepts, scenario guide ("I'm building a...") |
| **Tx Flow** | 8 | Auth path SVG, ISO 8583 key fields, settlement T+0/T+1/T+2, SMS vs DMS architecture, Visa V.I.P., MC Banknet, gateway routing |
| **Card Present** | 8 | CP vs CNP, DE 22 entry modes, contactless tap sequence (RF → EMV → ISO 8583), CVM limits, fallback, acquirer certification |
| **API** | — | Idempotency, webhook handling, network tokens, error handling patterns |
| **Tokenisation** | 8 | 6-party provisioning, TRID reference (Apple/Google/Samsung), token states, SE vs HCE, Account Updater |
| **Authentication** | 10 | 3DS 2.1/2.2/2.3, frictionless/challenge/decoupled/3RI flows, ECI values, CAVV, SCA exemptions |
| **Fraud & Risk** | 5 | Live RBA simulator, velocity rules, device signals, ML model overview, prevention guide with liability shift table |
| **Disputes** | 8 | Dispute lifecycle timeline, Visa allocation vs collaboration, MC reason codes, CE 3.0, monitoring programmes, prevention checklist |
| **PCI & Compliance** | 7 | PCI SSC standards map, PCI DSS v4.0.1 (12 reqs + v4 changes), SSF/S3 (14 objectives), SLC (6 objectives), SAD prohibition, EMV L1/L2/L3 |
| **Standards** | 5 | ISO 7812, 7813, 7816, 14443, standards stack |
| **Glossary** | — | 30+ payment terms defined |
| **History** | — | Timeline 1950–2024 |
| **Tools** | — | Luhn checker, payment flow simulator (8 DE39 scenarios), BIN prefix reference |
| **Appendix** | 4 | ISO 8583 Field 39 response codes, MCC codes (200+), country codes (ISO 3166), currency codes (ISO 4217) |

---

## Features

- **Dark / light mode** — auto-detects `prefers-color-scheme`, toggle in nav, persists to `localStorage`
- **Full-text search** — `⌘K` / `Ctrl+K` searches all 61 sections with highlighted snippets and direct navigation
- **15 SVG diagrams** — 4-party model, auth path, settlement timeline, SMS vs DMS, contactless flow, 3DS flows, token provisioning sequence, dispute lifecycle, EMV stack, and more — all CSS-variable-aware for both themes
- **Interactive tools** — Luhn/Mod10 checker, payment flow simulator with live card visual and transaction log
- **Mobile** — hamburger drawer, sub-nav wraps, responsive grids
- **No dependencies** — single HTML file, no npm, no build step, no external JS

---

## Architecture

```
index.html          ← entire site (single file, ~530KB, ~5900 lines)
README.md
CONTRIBUTING.md
LICENSE
```

The site is intentionally a single `.html` file with no build pipeline. This makes it trivially deployable to any static host and easy to fork/modify without tooling setup.

**Tech:**
- Vanilla JS (ES5-compatible, no frameworks)
- IBM Plex Mono + DM Sans via Google Fonts
- CSS custom properties for full theming
- SVG diagrams inline (no external image dependencies)

---

## Running locally

No build step needed.

```bash
git clone https://github.com/thierry0303/paydev-docs.git
cd paydev-docs
open index.html          # macOS
# or
start index.html         # Windows
# or serve with any static server:
npx serve .
python3 -m http.server 8080
```

---

## Deploying to GitHub Pages

The site is already configured for GitHub Pages. After cloning and making changes:

```bash
git add index.html
git commit -m "your change description"
git push origin main
```

GitHub Pages will deploy automatically from `main`. The live URL is:
`https://thierry0303.github.io/paydev-docs`

To enable GitHub Pages on a fresh fork:
1. Go to **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Save — live in ~60 seconds

---

## Content sources

Content is written from first principles and cross-referenced against publicly available scheme documentation and standards:

| Standard / Source | Used for |
|---|---|
| ISO 8583:1987/2003 | Field definitions, MTI structure, response codes |
| ISO/IEC 7812 | PAN/BIN structure, IIN migration |
| ISO/IEC 7813 | Track 1/2 magnetic stripe data |
| ISO/IEC 7816 | APDU structure, EMV chip interface |
| ISO/IEC 14443 | Contactless RF / NFC protocol |
| ISO 4217 | Currency codes and minor units |
| ISO 3166-1 | Country codes |
| ISO 18245 | Merchant Category Codes |
| EMVCo Book 1–4 | EMV kernel, cryptography, terminal behaviour |
| EMVCo 3DS 2.x spec | Authentication flows, AReq/ARes, ECI values |
| PCI DSS v4.0.1 (June 2024) | 12 requirements, SAQ levels, v4 changes |
| PCI SSF v1.2 (2023) | S3 objectives, SLC objectives |
| Visa Public Developer Docs | V.I.P., BASE II, VisaNet concepts |
| Mastercard Public Developer Docs | Banknet, MDS, DE 3 processing codes |

> **Proprietary documents** referenced during development (Mastercard SMS Specifications, Visa Gateway Cross-Reference, DMSC Technical Specifications, VTS Issuer Web Service Guide) were used only to understand concepts — no proprietary text is reproduced. The site content is original.

---

## Scope and limitations

This site covers **card network payments** (Visa, Mastercard, primarily). It does not cover:

- Open banking / account-to-account payments (SEPA, Faster Payments, ACH)
- Crypto payments
- BNPL / instalment products
- Specific PSP or gateway APIs (Stripe, Adyen, etc.) beyond generic concepts
- Real-time gross settlement or central bank systems

---

## Contributing

Corrections, additions, and improvements are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Common contribution types:
- **Corrections** — scheme rules change, response code descriptions need updating
- **Coverage gaps** — missing scenarios, edge cases not covered
- **New content** — sections that would benefit developers (e.g. open loop transit, SoftPOS, MoneySend)
- **Diagram improvements** — better SVG diagrams for existing content

---

## Versioning

The site uses a simple `vX.Y` badge visible in the nav bar. There is no semantic versioning — the badge increments with each meaningful content or feature update.

Current version: **v6.4**

---

## License

MIT License — see [LICENSE](LICENSE).

Content is provided for educational and reference purposes. It does not constitute legal, compliance, or financial advice. Always verify requirements against the applicable scheme rules, standards, and your compliance team.

---

## Acknowledgements

Built to fill a gap: card payment infrastructure is well-documented in scheme rulebooks and standards, but those documents are long, siloed, and not written for developers. This site tries to be what a senior payments engineer would explain to a new team member on their first week.
