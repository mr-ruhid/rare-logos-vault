# rare-logos-vault

Ödəniş sistemlərinin **düzgün və keyfiyyətli loqoları** bir yerdə. Artıq internetdə saatlarla axtarmağa ehtiyac yoxdur: loqonu adı ilə tapın, linkini götürün və istifadə edin.

Hazırda **22 loqo** var: kart şəbəkələri, elektron cüzdanlar, banklar, ödəniş gateway-ləri və təhlükəsizlik nişanları.

---

## Mündəricat

- [Sürətli başlanğıc](#sürətli-başlanğıc)
- [İstifadə nümunələri](#istifadə-nümunələri)
- [index.json strukturu](#indexjson-strukturu)
- [Repo quruluşu](#repo-quruluşu)
- [Loqoların siyahısı](#loqoların-siyahısı)
- [Töhfə vermək](#töhfə-vermək)

---

## Sürətli başlanğıc

Bütün loqoların siyahısı bir faylda saxlanılır: **`index.json`**. Hər loqonun adı, kateqoriyası, ölkəsi, formatı və birbaşa linki orada var.

```
https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json
```

Fayl adları bir-birindən fərqli ola bilər (`visa_inc.webp`, `Google_Pay_Logo.webp` və s.). Ona görə linki əl ilə yazmaq əvəzinə, `index.json`-dan adla götürün.

---

## İstifadə nümunələri

### HTML

Linki birbaşa `<img>` içində istifadə edə bilərsiniz:

```html
<img
  src="https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/amex/amex.svg"
  alt="American Express"
  height="32"
/>
```

### JavaScript: adla loqo tapmaq

```js
const INDEX_URL =
  "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json";

const db = await (await fetch(INDEX_URL)).json();

// 1) Açar (slug) ilə
const visa = db.logos["visa"];
console.log(visa.url);

// 2) Ad və ya alias ilə (məsələn "gpay", "3ds")
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

### JavaScript: kateqoriyaya görə siyahı

```js
// Bütün elektron cüzdanlar
const wallets = Object.values(db.logos).filter(l => l.category === "wallet");

// Yalnız Azərbaycan brendləri
const az = Object.values(db.logos).filter(l => l.country === "AZ");
```

### Python

```python
import requests

INDEX_URL = "https://cdn.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json"
db = requests.get(INDEX_URL, timeout=10).json()

logo = db["logos"]["paypal"]
print(logo["name"], logo["url"])

# Faylı yükləmək
img = requests.get(logo["url"], timeout=10).content
with open(f"paypal.{logo['format']}", "wb") as f:
    f.write(img)
```

### Ödəniş səhifəsində göstərmək (nümunə)

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

### Keş haqqında

Linklər [jsDelivr](https://www.jsdelivr.com/) CDN üzərindən verilir və keşlənir. Yeni commit-dən sonra dəyişiklik dərhal görünməyə bilər. Bu halda:

- linkdə `@main` əvəzinə commit hash və ya tag yazın (`@a1b2c3d`, `@v1.0.0`). Bu həm də linkin dəyişməməsini təmin edir.
- və ya keşi təmizləyin: `https://purge.jsdelivr.net/gh/mr-ruhid/rare-logos-vault@main/logo/payment-gateway/index.json`

---

## index.json strukturu

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

| Sahə | İzah |
| --- | --- |
| açar (`visa`) | Unikal ad (slug): kiçik hərf, boşluqsuz |
| `name` | Brendin rəsmi adı |
| `category` | `card-network`, `wallet`, `bank`, `gateway`, `instant-payment`, `terminal-network`, `security-badge` |
| `country` | ISO ölkə kodu (`AZ`, `US`). Qlobal nişanlar üçün `null` |
| `format` | `svg` və ya `webp` |
| `url` | Birbaşa CDN linki |
| `path` | Repo daxilindəki yol |
| `aliases` | Alternativ adlar (axtarış üçün) |

---

## Repo quruluşu

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

## Loqoların siyahısı

| Önizləmə | Ad | Açar | Kateqoriya | Ölkə | Format |
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

## Töhfə vermək

Yeni loqo əlavə etmək və ya mövcud loqonu düzəltmək istəyirsinizsə, [CONTRIBUTING.md](CONTRIBUTING.md) sənədini oxuyun. Orada fayl tələbləri, adlandırma qaydası və Pull Request addımları izah olunub.

Loqo çatışmırsa və ya səhv görsəniz, **Issue** açın.

---

> Bütün loqolar öz sahiblərinin əmtəə nişanıdır. Ətraflı: [CONTRIBUTING.md, hüquqi qeydlər](CONTRIBUTING.md#9-hüquqi-qeydlər-və-silinmə-sorğusu).
