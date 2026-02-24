# Portfolio — Software Engineer

A static one-page portfolio site for GitHub Pages, with support for a custom domain on Cloudflare.

---

## Is it legal to use GitHub Pages for this?

**Yes.** Personal and professional portfolio sites are explicitly allowed under [GitHub’s terms](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service) and [GitHub Pages guidelines](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits).

- **Allowed:** Personal pages, portfolios, company/project pages, open source project sites.  
- **Not allowed:** Using Pages as a general commercial web host (e.g. e‑commerce, SaaS, sites mainly for commercial transactions).  
- **Limited monetization** (e.g. donation links, ads) is OK as long as the site isn’t primarily commercial.

Your use must also follow the [Acceptable Use Policies](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies) (no illegal content, harassment, etc.).

---

## 1. Put this site on GitHub Pages

1. **Create a new repository** on GitHub (e.g. `username.github.io` for a **user/org site**, or any name like `portfolio` for a **project site**).
   - **User site:** repo name must be `YOUR_USERNAME.github.io` → site URL: `https://YOUR_USERNAME.github.io`.
   - **Project site:** any repo name → site URL: `https://YOUR_USERNAME.github.io/REPO_NAME`.

2. **Push this folder** to that repo:
   ```bash
   cd /Users/azmt/portfolio
   git init
   git add .
   git commit -m "Initial portfolio"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```

3. **Turn on GitHub Pages:**
   - Repo → **Settings** → **Pages** (under “Code and automation”).
   - **Source:** Deploy from a branch.
   - **Branch:** `main` (or `master`), folder **`/ (root)`**.
   - Save. The site will be at `https://YOUR_USERNAME.github.io` or `https://YOUR_USERNAME.github.io/REPO_NAME`.

4. **Optional:** Under Pages → **Custom domain**, add your domain **only after** you’ve added the domain in GitHub (see below). Then enable **Enforce HTTPS** when it appears.

---

## 2. Point your Cloudflare domain to GitHub Pages

Do **Step 2.1 (GitHub)** first, then **Step 2.2 (Cloudflare)**. Order matters to avoid subdomain takeover.

### 2.1 On GitHub (do this first)

1. Repo → **Settings** → **Pages**.
2. Under **Custom domain**, enter either:
   - **Apex:** `yourdomain.com`, or  
   - **www:** `www.yourdomain.com`  
   (You can do both; GitHub will redirect between them if DNS is set for both.)
3. Click **Save**. GitHub may add a `CNAME` file to your repo; if you build locally, pull that commit.

### 2.2 On Cloudflare (DNS)

In [Cloudflare Dashboard](https://dash.cloudflare.com) → your domain → **DNS** → **Records**.

**Option A — Use `www` (e.g. www.yourdomain.com)**

| Type  | Name | Content                    | Proxy status   |
|-------|------|----------------------------|----------------|
| CNAME | www  | **YOUR_USERNAME.github.io** | Proxied (orange) or DNS only (grey) |

Replace `YOUR_USERNAME` with your GitHub username. Do **not** include the repo name (e.g. not `username.github.io/portfolio`).

**Option B — Use apex only (yourdomain.com)**

Cloudflare supports CNAME flattening at the apex. Add:

| Type  | Name | Content                    | Proxy status   |
|-------|------|----------------------------|----------------|
| CNAME | @    | **YOUR_USERNAME.github.io** | Proxied or DNS only |

**Option C — Apex with A/AAAA (works with proxy)**

| Type | Name | Content           | Proxy status   |
|------|------|-------------------|----------------|
| A    | @    | 185.199.108.153   | Proxied or DNS only |
| A    | @    | 185.199.109.153   | Proxied or DNS only |
| A    | @    | 185.199.110.153   | Proxied or DNS only |
| A    | @    | 185.199.111.153   | Proxied or DNS only |
| AAAA | @    | 2606:50c0:8000::153 | Proxied or DNS only |
| AAAA | @    | 2606:50c0:8001::153 | Proxied or DNS only |
| AAAA | @    | 2606:50c0:8002::153 | Proxied or DNS only |
| AAAA | @    | 2606:50c0:8003::153 | Proxied or DNS only |

**Recommended:** Set up both apex and `www` (e.g. CNAME for `www` + CNAME or A/AAAA for `@`) so that GitHub can redirect between them and HTTPS works for both.

- **Proxied (orange cloud):** Traffic goes through Cloudflare (CDN, DDoS protection, optional WAF).  
- **DNS only (grey cloud):** Resolves directly to GitHub. Use this if you see SSL or redirect issues.

DNS can take up to 24 hours to propagate; often it’s much faster.

---

## 3. Customize the portfolio

- **index.html:** Update name, tagline, about, skills, projects, and contact links (GitHub, LinkedIn, email).
- **styles.css:** Adjust `:root` variables (colors, fonts) to match your style.
- Replace project placeholders with real titles, descriptions, and links.

No build step required; what you push is what GitHub Pages serves.

---

## 4. Optional: Domain verification (recommended)

To reduce the risk of someone else using your domain on their GitHub repo:

1. Repo → **Settings** → **Pages** → **Custom domain**.
2. Follow the instructions to **verify** your domain (e.g. add a TXT record in Cloudflare if GitHub asks for it).

---

## Quick checklist

- [ ] Create GitHub repo and push this site
- [ ] Enable Pages (branch `main`, root)
- [ ] Add custom domain in GitHub **before** DNS
- [ ] Add CNAME (and optionally A/AAAA) in Cloudflare
- [ ] Wait for DNS and enable **Enforce HTTPS** on GitHub if desired
- [ ] Optionally verify domain in GitHub
- [ ] Edit `index.html` and `styles.css` with your info
