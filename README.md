# Latent — asklatent.com

Bilingual (EN/ZH) marketing site. Single-page HTML, zero build step, zero dependencies.

- **Primary domain:** `asklatent.com` (Cloudflare Registrar)
- **Defensive redirect:** `justlatent.com` → 301 → `asklatent.com`

---

## Deploy path: Cloudflare Pages (recommended)

Your domain is already on Cloudflare Registrar — stay on Cloudflare for everything. Zero DNS config needed, everything auto-wires.

### Step 1. Push to GitHub (2 min)

```bash
cd ~/Documents/GitHub/latent3108/asklatent-site
git init
git add .
git commit -m "initial landing page"

# Create a GitHub repo (via gh CLI or the web UI), then:
git remote add origin git@github.com:<you>/asklatent-site.git
git push -u origin main
```

### Step 2. Connect Cloudflare Pages (3 min)

1. https://dash.cloudflare.com → **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**
2. Select the `asklatent-site` repo
3. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Build output directory:** `/`
4. Click **Save and Deploy**

First deploy takes ~30 seconds. You get a `*.pages.dev` preview URL immediately.

### Step 3. Bind `asklatent.com` (2 min)

1. Same Pages project → **Custom domains** → **Set up a custom domain**
2. Enter `asklatent.com` — Cloudflare detects same account, auto-configures DNS
3. Add `www.asklatent.com` as a second custom domain

SSL + CDN auto. Live in ~1 minute.

### Step 4. 301 redirect `justlatent.com` → `asklatent.com` (3 min)

1. Cloudflare dashboard → `justlatent.com` zone
2. **Rules** → **Redirect Rules** → **Create rule**
3. Rule config:
   - **When:** Hostname equals `justlatent.com` (create a second rule for `www.justlatent.com`)
   - **Then:** Static redirect, URL `https://asklatent.com$1`, Status `301`
4. Test: `curl -I justlatent.com` should return `301 Location: https://asklatent.com`

---

## Demo booking (5 min)

Pick one:

| Tool | Free tier | Best for |
|---|---|---|
| **Cal.com** | ✅ Unlimited | Open source, self-host possible |
| **Calendly** | ✅ 1 event type | Polished UX |
| **飞书妙约** | ✅ | China team + clients |

Sign up, create a 30-min "Latent Demo" event, copy the booking URL. Then edit `index.html`:

```js
const BOOKING_URL = 'https://cal.com/asklatent/demo';  // ← replace
```

Push to git → auto-redeploy in 30s.

---

## Email routing (5 min, free, unlimited addresses)

1. Cloudflare dashboard → `asklatent.com` zone → **Email** → **Email Routing** → **Enable**
2. Cloudflare auto-adds MX records. Add forwards:

| Address | Forwards to |
|---|---|
| `hello@asklatent.com` | your-email@gmail.com |
| `sales@asklatent.com` | your-email@gmail.com |
| `demo@asklatent.com` | your-email@gmail.com |
| `support@asklatent.com` | your-email@gmail.com |

3. Verify your destination Gmail. Done.

**Want to SEND from `hello@asklatent.com`?** Email Routing is receive-only. Options:

- **Gmail "Send As"** via Resend/SendGrid SMTP (free tier)
- **Zoho Mail** — full mailbox, 5 free
- **Google Workspace** — $6/user/mo if you need corporate-grade

---

## ICP 备案 (optional)

- Cloudflare CDN makes the site usable globally including inside China
- For full 备案 performance: Aliyun / Tencent Cloud, 2-4 weeks, requires Chinese entity
- Defer until you have real China SaaS revenue motion

---

## Edit content

Whole site is `index.html`. Bilingual via `data-en` / `data-zh` attributes:

```html
<h1 data-en>English copy</h1>
<h1 data-zh>中文文案</h1>
```

Push → auto-deploy.

## Local preview

```bash
python3 -m http.server 8000
open http://localhost:8000
```

---

## File tree

```
asklatent-site/
├── index.html       # The entire site
├── vercel.json      # Unused under Cloudflare Pages (kept as fallback)
├── README.md
└── .gitignore
```

---

## Tech

Plain HTML/CSS/JS. No framework, no build, no runtime deps (except Google Fonts).
i18n via `display: revert` toggled by `html[lang]`. Intersection Observer for fade-ins. LocalStorage for lang persistence. Inline SVG favicon.
