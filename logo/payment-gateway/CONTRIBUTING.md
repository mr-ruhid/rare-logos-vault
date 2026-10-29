# Contributing

Thank you for contributing to **rare-logos-vault**! Our goal is simple: people should not have to search the internet for hours to find the **correct, high-quality logos** of payment systems. Everything should be in one place.

This guide explains how to add a new logo, fix an existing one and report problems.

---

## Table of contents

1. [Ground rules](#1-ground-rules)
2. [Folder and file naming](#2-folder-and-file-naming)
3. [File requirements](#3-file-requirements)
4. [Adding to index.json](#4-adding-to-indexjson)
5. [Step by step: adding a new logo](#5-step-by-step-adding-a-new-logo)
6. [Commit message convention](#6-commit-message-convention)
7. [Pull Request checklist](#7-pull-request-checklist)
8. [Fixing an existing logo](#8-fixing-an-existing-logo)
9. [Legal notes and takedown requests](#9-legal-notes-and-takedown-requests)
10. [Code of conduct](#10-code-of-conduct)

---

## 1. Ground rules

- Only add **official** logos. Fan-made, redrawn or recolored versions are not accepted.
- Get the logo from the brand's **official website** whenever possible (brand / press kit / media resources page).
- Do not add duplicates. Search `index.json` first.
- Keep **one brand per Pull Request**. It makes review easier.
- Do not alter the logo's shape, proportions or colors (no stretching, squeezing, shadows or effects).

## 2. Folder and file naming

Logos are stored in this structure:

```
logo/
└── payment-gateway/
    ├── index.json
    ├── visa/
    │   └── visa_inc.webp
    ├── amex/
    │   └── amex.svg
    └── birbank_azerbaijan/
        └── birbank.svg
```

Rules:

- Folder name: **lowercase**, no spaces, English letters only (`stripe`, `wechat-pay`).
- For local brands that operate in a single country, add the country to the end of the folder name: `birbank_azerbaijan`, `pix_brasil`.
- File name: **lowercase**, words separated by `-` or `_`, and it should contain the brand name (`stripe.webp`, `apple_pay_transparent.svg`).
- If a brand has several variants (light/dark background, etc.), add the variant at the end: `visa_dark.svg`, `visa_light.svg`.
- Do not use non-English letters, uppercase letters or spaces in names.

## 3. File requirements

### SVG (preferred format)

- The background must be **transparent**.
- It must **not contain embedded raster images** (base64 PNG/JPG). Only vector paths are allowed.
- No `<script>`, external links or font dependencies. Text must be converted to outlines.
- A `viewBox` attribute must be present.
- Optimize with [SVGO](https://github.com/svg/svgo) if possible. File size must be **under 200 KB**.

### WebP / PNG (when no SVG is available)

- The background must be **transparent**.
- Width must be at least **512 px**, and the image must not be blurry or pixelated.
- No extra padding around the logo and no watermarks.
- If an SVG version is found later, it can be added alongside the WebP file.

### Colors

- Keep the official colors. You may add single-color (white/black) variants only if the brand itself provides them.

## 4. Adding to index.json

Every new logo must be added to the `logos` object in `logo/payment-gateway/index.json`. Don't forget to update `meta.count` too.

Example:

```json
"visa": {
  "name": "Visa",
  "category": "card-network",
  "country": "US",
  "format": "webp",
  "url": "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/visa/visa_inc.webp",
  "path": "logo/payment-gateway/visa/visa_inc.webp",
  "aliases": ["visa-inc"],
  "source": "https://brand.visa.com/"
}
```

Field reference:

| Field | Requirement | Description |
| --- | --- | --- |
| key (`visa`) | required | Unique slug: lowercase, no spaces, words separated by `-`. |
| `name` | required | Official spelling of the brand. |
| `category` | required | One of the categories listed below. |
| `country` | required | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code (`AZ`, `US`). Use `null` for global badges. |
| `format` | required | `svg`, `webp` (or `png`). |
| `url` | required | jsDelivr CDN link. Do **not** use `github.com/.../blob/...` links. |
| `path` | required | File path inside the repository. |
| `aliases` | optional | Alternative names that can be used for lookup (`gpay`, `3ds`). |
| `source` | **required for new logos** | Link to the official page the logo was taken from. |

Allowed categories:

`card-network`, `wallet`, `bank`, `gateway`, `instant-payment`, `terminal-network`, `security-badge`

If you need a new category, please open an Issue and propose it first.

After editing, check that the JSON is valid:

```bash
python3 -m json.tool logo/payment-gateway/index.json > /dev/null && echo "JSON is valid"
```

## 5. Step by step: adding a new logo

1. **Fork** the repository.
2. Clone your fork and create a new branch:
   ```bash
   git checkout -b add-<brand-name>
   ```
3. Add the logo to the correct folder with the correct name (section 2).
4. Check the file requirements (section 3).
5. Add a new entry to `index.json` and increase `meta.count`.
6. Validate the JSON (command at the end of section 4).
7. Commit following section 6:
   ```bash
   git add .
   git commit -m "feat(payment-gateway): add <Brand> logo"
   ```
8. Push your branch and open a **Pull Request**. Mention the logo's source in the PR description.

If you are not familiar with Git, you can also upload files and open a PR through the GitHub web interface.

## 6. Commit message convention

Format:

```
<type>(payment-gateway): <short description>
```

Types:

| Type | When to use |
| --- | --- |
| `feat` | A new logo or a new variant is added |
| `fix` | A wrong logo, a wrong link or an `index.json` error is fixed |
| `chore` | File renames, optimization, cleanup |
| `docs` | Changes to README, CONTRIBUTING, etc. |

Examples:

```
feat(payment-gateway): add Kapital Bank logo
fix(payment-gateway): replace blurry paypal.webp with svg
chore(payment-gateway): optimize amex.svg with svgo
docs: update contributing guide
```

## 7. Pull Request checklist

Before opening a PR, check the following:

- [ ] The logo comes from an official source and the `source` field is filled in
- [ ] The same logo does not already exist in the repo
- [ ] The background is transparent
- [ ] If SVG: no embedded raster images and no scripts
- [ ] If raster: at least 512 px wide and not blurry
- [ ] Folder and file names follow the rules (lowercase, no spaces)
- [ ] An entry was added to `index.json` and `meta.count` was updated
- [ ] The JSON is valid (`json.tool` shows no error)
- [ ] The `url` field is a jsDelivr link, not `github.com/blob`
- [ ] The PR covers only one brand

## 8. Fixing an existing logo

- **Outdated or wrong logo:** Add the new file and explain in the PR what changed and why. Keep the old file name where possible so existing links do not break.
- **Better version found (e.g. SVG instead of WebP):** Add the new file next to the old one and update the `format` and `url` fields in `index.json`.
- **Renaming a file:** This can break projects that use the links. Please open an Issue and discuss it first.

## 9. Legal notes and takedown requests

- All logos are **trademarks** of their respective owners. This repository collects them only to indicate supported payment methods and does not claim ownership.
- Some brands (Visa, Mastercard, Apple Pay, Google Pay, etc.) set **strict usage rules** for their logos. Anyone using a logo is responsible for following those rules.
- If you are the owner of a logo, or an authorized representative, and want it removed, please open an **Issue** or contact the repository owner. After verification, the logo will be removed promptly.
- Do not add unofficial designs or redrawn logos that are protected by someone else's copyright.

## 10. Code of conduct

- Be respectful and constructive. Focus on the work, not the person.
- Review comments are there to protect the quality of the project. Please do not take them personally.
- Insults, harassment and spam are not tolerated.

---

If you have any questions, open an **Issue**. Thank you in advance for your contributions!
