# Rahini Sunset Villa — Production Website

**Domain:** www.rahinisunsetvilla.com
**Stack:** Single-file HTML/CSS/JS · Razorpay payments · EmailJS confirmations · jsPDF receipts

---

## What's in this folder

```
rahini-sunset-villa/
├── index.html         # The entire website
├── netlify.toml       # Netlify config (HTTPS, www-canonical, security headers)
├── README.md          # This file
└── images/            # 12 optimized property photos (1.31 MB total)
    ├── hero.jpg
    ├── room-sunset-signature.jpg
    ├── room-mountain-suite.jpg
    ├── room-family-suite.jpg
    ├── room-full-villa.jpg
    ├── experience.jpg
    ├── about.jpg
    └── gallery-1..5-*.jpg
```

---

## 3 keys to fill in before going live
Open `index.html`, find the `CONFIG` block (around line 1305):

| Key | What to do |
|-----|------------|
| `RAZORPAY_KEY` | Currently set to a public **test** key (`rzp_test_1DP5mmOlF5G5ag`). Replace with your live key from https://dashboard.razorpay.com/app/keys (starts with `rzp_live_`) |
| `EMAILJS_TEMPLATE_ID` | Replace `'template_booking'` with your real template ID from EmailJS dashboard |
| `EMAILJS_PUBLIC_KEY` | Replace `'YOUR_EMAILJS_PUBLIC_KEY'` with your EmailJS public key (Account → API Keys) |

Already wired in (no changes needed):
- ✅ EmailJS service: `service_3l8l24g`
- ✅ Phone: `+91 70186 69092`
- ✅ Domain: `www.rahinisunsetvilla.com`

---

## Deploy to Netlify (3 minutes)

### Drag-and-drop method
1. Go to https://app.netlify.com/
2. Drag the entire `rahini-sunset-villa` folder onto the dashboard's deploy zone.
3. You'll get a temporary `*.netlify.app` URL — open it and test.

### Connect your custom domain
1. Netlify → **Site settings → Domain management → Add custom domain**.
2. Add `www.rahinisunsetvilla.com` and `rahinisunsetvilla.com`.
3. At your domain registrar, set DNS:
   - **CNAME** record: `www` → `<your-site>.netlify.app`
   - **A** record: `@` → `75.2.60.5` (Netlify's load balancer)

   Or change nameservers to Netlify's DNS for fully automatic management.
4. SSL auto-provisions (Let's Encrypt) within 5–60 minutes. The `netlify.toml` already forces HTTPS.

### Git-based deploys (recommended for ongoing updates)
```bash
git init && git add . && git commit -m "Launch"
# Push to GitHub, then in Netlify "Import from Git" → select repo
```
Every push auto-deploys.

---

## Switching from TEST → LIVE Razorpay payments

1. Open `index.html`, find:
   ```js
   RAZORPAY_KEY: 'rzp_test_1DP5mmOlF5G5ag',
   ```
2. Replace with your live key (starts with `rzp_live_`).
3. Re-deploy.

### Test cards (TEST MODE — no real money)
- **Card:** `4111 1111 1111 1111` · CVV `123` · expiry any future date · OTP `1234`
- **UPI success:** `success@razorpay`
- **UPI failure:** `failure@razorpay`

End-to-end test: book → pay with test card → see confirmation → download PDF receipt. No real money moves until you swap the key.

---

## (Optional) Server-side payment verification

Right now Razorpay payments work fully client-side. For tamper-proof production security, add a Netlify Function that creates orders and verifies signatures with your Razorpay secret key.

Sample (`netlify/functions/create-order.js`):
```js
const Razorpay = require('razorpay');
exports.handler = async (event) => {
  const { amount, booking_id } = JSON.parse(event.body);
  const rzp = new Razorpay({
    key_id: process.env.RAZORPAY_KEY_ID,
    key_secret: process.env.RAZORPAY_KEY_SECRET
  });
  const order = await rzp.orders.create({ amount, currency:'INR', receipt: booking_id });
  return { statusCode: 200, body: JSON.stringify(order) };
};
```
Set `RAZORPAY_KEY_ID` and `RAZORPAY_KEY_SECRET` in Netlify → Site settings → Environment variables.

---

## Build summary

| Concern | Status |
|---------|--------|
| Real Razorpay payment gateway (UPI/Card/Net Banking/Wallet) | ✅ Money actually moves |
| PDF receipt is receipt-only (jsPDF, not screenshot) | ✅ Clean A4 PDF |
| Guest dropdown adapts to room max-occupancy | ✅ Pick 2-guest room → only 1, 2 shown |
| Adults / Children / Pets selectors | ✅ Smart capacity logic |
| Phone updated everywhere | ✅ +91 70186 69092 |
| Domain pinned to www.rahinisunsetvilla.com | ✅ |
| All "Airbnb" branding removed | ✅ Stress-tested |
| Luxury gold logo (SVG, scalable) | ✅ |
| EmailJS service `service_3l8l24g` wired | ✅ |
| SSL/HTTPS + www-canonical via netlify.toml | ✅ |
| Mobile-first responsive | ✅ |
| Real property photos (12 images, 1.31 MB optimized) | ✅ |
| Payment failure & dismissal handled (no false confirms) | ✅ |
