# Rahini Sunset Villa — Production Website

**Domain:** www.rahinisunsetvilla.com
**Stack:** Single-file HTML/CSS/JS · Razorpay payments · EmailJS confirmations · jsPDF receipts

---

## What's in this folder

```
rahini-sunset-villa/
├── index.html         # The entire website
├── netlify.toml       # Netlify config (headers + caching, NO manual redirects)
├── .gitignore         # Safe git ignore (does NOT exclude images/)
├── README.md          # This file
└── images/            # 12 optimized property photos (1.31 MB)
    ├── hero.jpg
    ├── room-sunset-signature.jpg
    ├── room-mountain-suite.jpg
    ├── room-family-suite.jpg
    ├── room-full-villa.jpg
    ├── experience.jpg
    ├── about.jpg
    └── gallery-1..5-*.jpg
```

**CRITICAL:** The `images/` folder MUST be deployed with `index.html`. Without it, the site loads but photos are broken.

---

## Deploying via GitHub — avoiding the 2 common pitfalls

### 1. Push the entire folder, not just index.html
```bash
cd rahini-sunset-villa
git init
git add .                          # the dot is critical — grabs everything
git status                         # VERIFY you see index.html, netlify.toml, AND all 12 images
git commit -m "Launch Rahini Sunset Villa"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

**Before pushing, `git status` MUST show all 12 image files.** If missing, check that `.gitignore` isn't excluding them.

### 2. Connect Netlify to GitHub
- Netlify dashboard → "Add new site" → "Import from Git" → authorize GitHub → select repo.
- Build settings: leave "Build command" empty. "Publish directory" = `.` (a single dot).
- Deploy. You get a `*.netlify.app` URL. Open it — verify images display BEFORE touching the domain.

### 3. Attach your custom domain (set Primary correctly)
- Netlify → Site settings → Domain management.
- Add both `rahinisunsetvilla.com` and `www.rahinisunsetvilla.com`.
- Pick one (recommend `www.rahinisunsetvilla.com`) → click "Options" → "Set as primary domain".
  - Netlify automatically redirects the non-primary to the primary.
  - DO NOT add manual redirect rules — that causes ERR_TOO_MANY_REDIRECTS.
- DNS at your registrar:
  - `A` record: `@` → `75.2.60.5`
  - `CNAME` record: `www` → `<your-site-name>.netlify.app`
- SSL auto-provisions within 5–60 min. Under HTTPS, click "Verify DNS configuration" then "Provision certificate".

---

## If you drag-and-drop instead of using Git

1. https://app.netlify.com/ → drag the entire `rahini-sunset-villa` FOLDER (not just the HTML file) onto the deploy zone.
2. In the deploy details, scroll to "Deploy file browser" — confirm `images/` folder is there with 12 files.

---

## Fixing "images not showing"

If the site loads but room cards / gallery show broken images:

1. Right-click a broken image → "Open image in new tab" → note the URL.
2. It should be `https://www.rahinisunsetvilla.com/images/room-sunset-signature.jpg` etc.
3. Paste that URL directly in the browser:
   - **404 Not Found** → the file wasn't deployed. Check your GitHub repo — does it contain the `images/` folder with all 12 files? If not, push it.
   - **Loads fine** → hard-refresh the main site (Ctrl+Shift+R / Cmd+Shift+R).
4. In Netlify: Deploys → latest deploy → "Browse deploy files" — confirm `images/` folder is there.

---

## Fixing ERR_TOO_MANY_REDIRECTS

**Cause:** Manual redirects in `netlify.toml` fighting with Netlify's built-in primary-domain redirect.

**Fix (already applied in this `netlify.toml`):**
1. No `[[redirects]]` sections exist in this file. Good.
2. In Netlify → Site settings → Domain management → set ONE domain as Primary. Netlify handles redirect natively, no loop.
3. Clear browser cookies for `rahinisunsetvilla.com` (Chrome: lock icon → Cookies → Manage → delete all). The error page says this for a reason — old redirects are cached.
4. Re-deploy, or wait 1–2 min for the new `netlify.toml` to take effect.

---

## The 3 keys in `index.html` CONFIG block

All wired in. No action needed:

| Key | Value |
|-----|-------|
| `RAZORPAY_KEY` | `rzp_test_1DP5mmOlF5G5ag` (test key — swap to `rzp_live_...` to go live) |
| `EMAILJS_SERVICE_ID` | `service_3l8l24g` ✅ |
| `EMAILJS_TEMPLATE_ID` | `template_ns0esqd` ✅ |
| `EMAILJS_PUBLIC_KEY` | `_DWiAlrM3ovU-0rUT` ✅ |

### Test cards (no real money)
- Card: `4111 1111 1111 1111` · CVV `123` · future expiry · OTP `1234`
- UPI: `success@razorpay` / `failure@razorpay`

### Going live
Edit `index.html`, find:
```js
RAZORPAY_KEY: 'rzp_test_1DP5mmOlF5G5ag',
```
Replace with your live key from https://dashboard.razorpay.com/app/keys. Commit. Push.

---

## Build summary

| Feature | Status |
|---------|--------|
| Real Razorpay payment gateway (UPI/Card/Net Banking/Wallet) | ✅ |
| PDF receipt (jsPDF vector, not screenshot) | ✅ |
| Guest dropdown adapts per room | ✅ |
| Adults / Children / Pets selectors, capacity-aware | ✅ |
| Phone +91 70186 69092 everywhere | ✅ |
| Domain www.rahinisunsetvilla.com | ✅ |
| No Airbnb branding | ✅ |
| Luxury gold logo (SVG) | ✅ |
| EmailJS fully wired (service/template/public key) | ✅ |
| SSL + security headers | ✅ |
| No redirect loops | ✅ |
| Aggressive image caching (1-year immutable) | ✅ |
| Mobile-first responsive | ✅ |
| 12 real property photos (1.31 MB) | ✅ |
| Payment failure + dismissal handled | ✅ |
