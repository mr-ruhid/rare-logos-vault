# Töhfə vermə qaydası (Contributing)

**rare-logos-vault** layihəsinə töhfə verdiyiniz üçün təşəkkür edirik! Məqsədimiz sadədir: insanlar ödəniş sistemlərinin **düzgün və keyfiyyətli loqolarını** internetdə saatlarla axtarmasın, hamısı bir yerdə olsun.

Bu sənəd yeni loqo əlavə etməyin, mövcud loqonu düzəltməyin və problem bildirməyin qaydalarını izah edir.

---

## Mündəricat

1. [Əsas qaydalar](#1-əsas-qaydalar)
2. [Qovluq və fayl adlandırma](#2-qovluq-və-fayl-adlandırma)
3. [Fayl tələbləri](#3-fayl-tələbləri)
4. [index.json-a əlavə etmə](#4-indexjson-a-əlavə-etmə)
5. [Addım-addım: yeni loqo əlavə etmək](#5-addım-addım-yeni-loqo-əlavə-etmək)
6. [Commit mesajı qaydası](#6-commit-mesajı-qaydası)
7. [Pull Request yoxlama siyahısı](#7-pull-request-yoxlama-siyahısı)
8. [Mövcud loqonu düzəltmək](#8-mövcud-loqonu-düzəltmək)
9. [Hüquqi qeydlər və silinmə sorğusu](#9-hüquqi-qeydlər-və-silinmə-sorğusu)
10. [Davranış qaydaları](#10-davranış-qaydaları)

---

## 1. Əsas qaydalar

- Yalnız **rəsmi** loqoları əlavə edin. Fanat tərəfindən yenidən çəkilmiş, dəyişdirilmiş və ya rəngi dəyişdirilmiş versiyalar qəbul edilmir.
- Loqonu mümkün qədər **rəsmi brend səhifəsindən** (brand / press kit / media resources) götürün.
- Eyni loqo artıq varsa, təkrar əlavə etməyin. Əvvəlcə `index.json`-da axtarın.
- Bir Pull Request-də **bir brend** olsun. Bu, yoxlamanı asanlaşdırır.
- Loqonun forması, nisbəti və rəngləri **dəyişdirilməməlidir** (uzatmaq, sıxmaq, kölgə və effekt əlavə etmək olmaz).

## 2. Qovluq və fayl adlandırma

Loqolar bu quruluşda saxlanılır:

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

Qaydalar:

- Qovluq adı: **kiçik hərflərlə**, boşluqsuz, ingilis əlifbası ilə (`stripe`, `wechat-pay`).
- Yalnız bir ölkədə fəaliyyət göstərən yerli brendlər üçün qovluğun sonuna ölkə əlavə edin: `birbank_azerbaijan`, `pix_brasil`.
- Fayl adı: **kiçik hərflərlə**, sözləri `-` və ya `_` ilə ayırın. Adda brendin adı olsun (`stripe.webp`, `apple_pay_transparent.svg`).
- Eyni brendin bir neçə variantı varsa (açıq/tünd fon və s.), sonuna variantı yazın: `visa_dark.svg`, `visa_light.svg`.
- Adlarda Azərbaycan hərfləri (ə, ö, ü, ı, ş, ç, ğ), böyük hərf və boşluq istifadə etməyin.

## 3. Fayl tələbləri

### SVG (üstünlük verilən format)

- Fon **şəffaf** olmalıdır.
- Daxilində **rastr şəkil (base64 PNG/JPG) olmamalıdır**. Yalnız vektor yollar (path) olmalıdır.
- `<script>`, xarici linklər və şrift asılılığı olmamalıdır. Yazılar yola (outline) çevrilməlidir.
- `viewBox` atributu olmalıdır.
- Mümkünsə [SVGO](https://github.com/svg/svgo) ilə optimallaşdırın. Ölçü **200 KB-dan** az olmalıdır.

### WebP / PNG (SVG tapılmadıqda)

- Fon **şəffaf** olmalıdır.
- Ən azı **512 px** eni olmalıdır, bulanıq və ya pikselləşmiş olmamalıdır.
- Ətrafında artıq boşluq (padding) və ya su nişanı (watermark) olmamalıdır.
- SVG variantı sonradan tapılsa, WebP faylı ilə yanaşı əlavə edilə bilər.

### Rənglər

- Rəsmi rəngləri saxlayın. Yalnız brendin özünün təqdim etdiyi tək rəngli (ağ/qara) variantları əlavə edə bilərsiniz.

## 4. index.json-a əlavə etmə

Hər yeni loqo `logo/payment-gateway/index.json` faylındakı `logos` obyektinə əlavə olunmalıdır. `meta.count` dəyərini də yeniləməyi unutmayın.

Nümunə:

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

Sahələrin izahı:

| Sahə | Tələb | İzah |
| --- | --- | --- |
| açar (`visa`) | məcburi | Kiçik hərflə, boşluqsuz, `-` ilə ayrılmış unikal ad (slug). |
| `name` | məcburi | Brendin rəsmi yazılışı. |
| `category` | məcburi | Aşağıdakı siyahıdan biri. |
| `country` | məcburi | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) kodu (`AZ`, `US`). Qlobal nişanlar üçün `null`. |
| `format` | məcburi | `svg` və ya `webp` (və ya `png`). |
| `url` | məcburi | jsDelivr CDN linki. `github.com/.../blob/...` linki **yazmayın**. |
| `path` | məcburi | Repo daxilindəki fayl yolu. |
| `aliases` | ixtiyari | Axtarışda istifadə oluna biləcək alternativ adlar (`gpay`, `3ds`). |
| `source` | **yeni loqolar üçün məcburi** | Loqonun götürüldüyü rəsmi səhifənin linki. |

İcazə verilən kateqoriyalar:

`card-network`, `wallet`, `bank`, `gateway`, `instant-payment`, `terminal-network`, `security-badge`

Yeni kateqoriya lazımdırsa, əvvəlcə Issue açıb təklif edin.

Əlavə etdikdən sonra JSON-un düzgün olduğunu yoxlayın:

```bash
python3 -m json.tool logo/payment-gateway/index.json > /dev/null && echo "JSON düzgündür"
```

## 5. Addım-addım: yeni loqo əlavə etmək

1. Repo-nu **Fork** edin.
2. Fork-u kompüterinizə klonlayın və yeni branch yaradın:
   ```bash
   git checkout -b add-<brend-adi>
   ```
3. Loqonu bölmə 2-yə uyğun qovluğa və adla əlavə edin.
4. Fayl tələblərini (bölmə 3) yoxlayın.
5. `index.json`-a yeni qeyd əlavə edin və `meta.count` dəyərini artırın.
6. JSON-u yoxlayın (bölmə 4-ün sonundakı komanda).
7. Commit edin (bölmə 6-ya uyğun):
   ```bash
   git add .
   git commit -m "feat(payment-gateway): add <Brend> logo"
   ```
8. Branch-i push edin və **Pull Request** açın. PR təsvirində loqonun mənbəyini göstərin.

Git ilə tanış deyilsinizsə, GitHub-ın veb interfeysindən də fayl yükləyib PR aça bilərsiniz.

## 6. Commit mesajı qaydası

Format:

```
<tip>(payment-gateway): <qısa izah>
```

Tiplər:

| Tip | Nə vaxt |
| --- | --- |
| `feat` | Yeni loqo və ya yeni variant əlavə edildikdə |
| `fix` | Səhv loqo, səhv link və ya `index.json` xətası düzəldildikdə |
| `chore` | Fayl adı dəyişikliyi, optimallaşdırma, təmizlik |
| `docs` | README, CONTRIBUTING və s. dəyişiklikləri |

Nümunələr:

```
feat(payment-gateway): add Kapital Bank logo
fix(payment-gateway): replace blurry paypal.webp with svg
chore(payment-gateway): optimize amex.svg with svgo
docs: update contributing guide
```

## 7. Pull Request yoxlama siyahısı

PR açmazdan əvvəl bunları yoxlayın:

- [ ] Loqo rəsmi mənbədən götürülüb və `source` sahəsi doldurulub
- [ ] Eyni loqo repoda əvvəlcədən yoxdur
- [ ] Fon şəffafdır
- [ ] SVG-dirsə, içində rastr şəkil və skript yoxdur
- [ ] Rastr formatdırsa, eni ən azı 512 px-dir və bulanıq deyil
- [ ] Qovluq və fayl adı qaydalara uyğundur (kiçik hərf, boşluqsuz)
- [ ] `index.json`-a qeyd əlavə olunub, `meta.count` yenilənib
- [ ] JSON düzgündür (`json.tool` xəta vermir)
- [ ] `url` sahəsi jsDelivr linkidir, `github.com/blob` deyil
- [ ] PR yalnız bir brendi əhatə edir

## 8. Mövcud loqonu düzəltmək

- **Köhnə və ya səhv loqo:** Yeni faylı əlavə edin və PR-da nəyin dəyişdiyini və səbəbini yazın. Köhnə fayl adını mümkün qədər saxlayın ki, mövcud linklər pozulmasın.
- **Keyfiyyətli versiya tapılıbsa (məsələn WebP əvəzinə SVG):** Yeni faylı yanına əlavə edin və `index.json`-da `format` və `url` sahələrini yeniləyin.
- **Fayl adının dəyişdirilməsi:** Linkləri istifadə edən layihələri sındıra bilər. Əvvəlcə Issue açıb müzakirə edin.

## 9. Hüquqi qeydlər və silinmə sorğusu

- Bütün loqolar öz sahiblərinin **əmtəə nişanıdır**. Bu repo yalnız ödəniş üsullarını göstərmək məqsədilə onları toplayır və sahiblik iddia etmir.
- Bəzi brendlər (Visa, Mastercard, Apple Pay, Google Pay və s.) loqolarından istifadə üçün **ciddi qaydalar** təyin edir. Loqonu istifadə edən şəxs həmin qaydalara əməl etməlidir.
- Loqonun sahibi və ya səlahiyyətli nümayəndəsi loqonun silinməsini istəyirsə, **Issue** açın və ya repo sahibi ilə əlaqə saxlayın. Sorğu yoxlanıldıqdan sonra loqo tez bir zamanda silinəcək.
- Başqasının müəllif hüququ ilə qorunan, rəsmi olmayan dizayn və ya yenidən çəkilmiş loqoları əlavə etməyin.

## 10. Davranış qaydaları

- Hörmətli və konstruktiv olun. Rəy bildirərkən şəxsə yox, işə fikir verin.
- Yoxlama zamanı verilən qeydlər layihənin keyfiyyətini qorumaq üçündür, bunu şəxsi qəbul etməyin.
- Təhqir, təzyiq və spam qəbul edilmir.

---

Sualınız varsa, **Issue** açın. Töhfələriniz üçün əvvəlcədən təşəkkür edirik!
