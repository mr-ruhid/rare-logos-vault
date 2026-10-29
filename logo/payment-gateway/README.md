# rare-logos-vault

**Correct, high-quality payment logos in one place.** No more spending hours searching the internet: find a logo by name, grab its link and use it.

The vault currently contains **22 logos**: card networks, digital wallets, banks, payment gateways and security badges.

---

## Table of contents

- [Quick start](#quick-start)
- [Usage examples](#usage-examples)
- [index.json structure](#indexjson-structure)
- [Repository structure](#repository-structure)
- [Logo list](#logo-list)
- [Contributing](#contributing)

---

## Quick start

All logos are listed in a single file: **`index.json`**. For every logo it contains the name, category, country, format and a direct link.

```
https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json
```

File names are not uniform across the repo (`visa_inc.webp`, `Google_Pay_Logo.webp`, etc.), so instead of building links by hand, look logos up by name in `index.json`.

---

## Usage examples

### HTML

Use the link directly inside an `<img>` tag:

```html
<img
  src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/amex/amex.svg"
  alt="American Express"
  height="32"
/>
```

### JavaScript: find a logo by name

```js
const INDEX_URL =
  "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json";

const db = await (await fetch(INDEX_URL)).json();

// 1) By key (slug)
const visa = db.logos["visa"];
console.log(visa.url);

// 2) By name or alias (e.g. "gpay", "3ds")
function findLogo(query) {
  const q = query.toLowerCase().trim();
  return Object.entries(db.logos).find(([slug, logo]) =>
    slug === q ||
    logo.name.toLowerCase() === q ||
    (logo.aliases || []).includes(q)
  )?.[1];
}

console.log(findLogo("gpay").url);
```

### JavaScript: filter by category or country

```js
// All digital wallets
const wallets = Object.values(db.logos).filter(l => l.category === "wallet");

// Brands from Azerbaijan only
const az = Object.values(db.logos).filter(l => l.country === "AZ");
```

### Python

```python
import requests

INDEX_URL = "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json"
db = requests.get(INDEX_URL, timeout=10).json()

logo = db["logos"]["paypal"]
print(logo["name"], logo["url"])

# Download the file
img = requests.get(logo["url"], timeout=10).content
with open(f"paypal.{logo['format']}", "wb") as f:
    f.write(img)
```

### Showing accepted payment methods on a checkout page

```html
<div id="payment-methods"></div>

<script>
  const slugs = ["visa", "mastercard", "apple-pay", "google-pay"];

  fetch("https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json")
    .then(r => r.json())
    .then(db => {
      document.getElementById("payment-methods").innerHTML = slugs
        .map(s => `<img src="${db.logos[s].url}" alt="${db.logos[s].name}" height="28">`)
        .join("");
    });
</script>
```

### About caching

Links are served through the [jsDelivr](https://www.jsdelivr.com/) CDN and are cached. After a new commit, changes may not appear immediately. In that case:

- use a commit hash or tag instead of `@main` in the link (`@a1b2c3d`, `@v1.0.0`). This also keeps the link stable over time.
- or purge the cache: `https://purge.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json`

---

## index.json structure

```json
{
  "meta": {
    "name": "rare-logos-vault / payment-gateway",
    "version": "1.0.0",
    "cdn_base": "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/",
    "count": 22
  },
  "logos": {
    "visa": {
      "name": "Visa",
      "category": "card-network",
      "country": "US",
      "format": "webp",
      "url": "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/visa/visa_inc.webp",
      "path": "logo/payment-gateway/visa/visa_inc.webp",
      "aliases": ["visa-inc"]
    }
  }
}
```

| Field | Description |
| --- | --- |
| key (`visa`) | Unique slug: lowercase, no spaces |
| `name` | Official brand name |
| `category` | `card-network`, `wallet`, `bank`, `gateway`, `instant-payment`, `terminal-network`, `security-badge` |
| `country` | ISO country code (`AZ`, `US`); `null` for global badges |
| `format` | `svg` or `webp` |
| `url` | Direct CDN link |
| `path` | Path inside the repository |
| `aliases` | Alternative names (for lookup) |

---

## Repository structure

```
logo/
└── payment-gateway/
    ├── index.json
    ├── visa/
    ├── amex/
    ├── birbank_azerbaijan/
    └── ...
```

---

## Logo list

| Preview | Name | Key | Category | Country | Format |
| --- | --- | --- | --- | --- | --- |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/m10_azerbaijan/m10.svg" height="32" alt="M10"> | M10 | `m10` | wallet | AZ | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/alipay/alipay.webp" height="32" alt="Alipay"> | Alipay | `alipay` | wallet | CN | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/amex/amex.svg" height="32" alt="American Express"> | American Express | `amex` | card-network | US | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/apple-pay/apple_pay_transparent.svg" height="32" alt="Apple Pay"> | Apple Pay | `apple-pay` | wallet | US | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/birbank_azerbaijan/birbank.svg" height="32" alt="Birbank"> | Birbank | `birbank` | bank | AZ | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/google-pay/Google_Pay_Logo.webp" height="32" alt="Google Pay"> | Google Pay | `google-pay` | wallet | US | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/jcb/jcb.webp" height="32" alt="JCB"> | JCB | `jcb` | card-network | JP | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/leobank_azerbaijan/Logo_of_Leobank.webp" height="32" alt="Leobank"> | Leobank | `leobank` | bank | AZ | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/m-pesa_africa/m-pesa.webp" height="32" alt="M-Pesa"> | M-Pesa | `m-pesa` | wallet | KE | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/maestro/maestro_logo_variant2.webp" height="32" alt="Maestro"> | Maestro | `maestro` | card-network | US | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/million_azerbaijan/million.svg" height="32" alt="Million"> | Million | `million` | terminal-network | AZ | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/mastercard/mastercard_footer_logo.webp" height="32" alt="Mastercard"> | Mastercard | `mastercard` | card-network | US | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/pay-pal/paypal.webp" height="32" alt="PayPal"> | PayPal | `paypal` | wallet | US | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/payment-component/3d-secure.svg" height="32" alt="3-D Secure"> | 3-D Secure | `3d-secure` | security-badge | — | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/payment-component/pci-dss.svg" height="32" alt="PCI DSS"> | PCI DSS | `pci-dss` | security-badge | — | svg |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/phonepe/PhonePe_Logo.webp" height="32" alt="PhonePe"> | PhonePe | `phonepe` | wallet | IN | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/pix_brasil/pix.webp" height="32" alt="Pix"> | Pix | `pix` | instant-payment | BR | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/stc_pay/stc-pay.webp" height="32" alt="stc pay"> | stc pay | `stc-pay` | wallet | SA | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/stripe/stripe.webp" height="32" alt="Stripe"> | Stripe | `stripe` | gateway | US | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/unionpay/unionpay.webp" height="32" alt="UnionPay"> | UnionPay | `unionpay` | card-network | CN | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/visa/visa_inc.webp" height="32" alt="Visa"> | Visa | `visa` | card-network | US | webp |
| <img src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/wechat-pay/wechat-pay.svg" height="32" alt="WeChat Pay"> | WeChat Pay | `wechat-pay` | wallet | CN | svg |

---

## Contributing

Want to add a new logo or fix an existing one? Read [CONTRIBUTING.md](CONTRIBUTING.md). It explains file requirements, naming rules and the Pull Request steps.

If a logo is missing or wrong, please open an **Issue**.

---

> All logos are trademarks of their respective owners. More details: [CONTRIBUTING.md, legal notes](CONTRIBUTING.md#9-legal-notes-and-takedown-requests).
