# CAN YOU RETIRE?

Satirical S&P 500 retirement calculator. Single self-contained HTML file.

## Project Structure

```
index.html          # The entire app (HTML + CSS + JS, ~100KB)
public/             # Deploy directory (copy of index.html + favicons)
  index.html
  favicon.ico
  favicon.png
```

## Deployment

Hosted on **Cloudflare Pages**. Live at:
- https://canyouretire.pages.dev
- https://canyouretire.jaye.es (custom domain, CNAME to canyouretire.pages.dev)

### Deploy steps

```bash
# 1. Copy source to public dir
cp index.html public/index.html

# 2. Deploy to Cloudflare Pages
wrangler pages deploy ./public --project-name canyouretire --branch main
```

If `wrangler pages deploy` fails with 500 errors (intermittent Cloudflare API issue), retry. If it consistently fails, try deploying without `apple-touch-icon.png` -- that file has caused upload issues:

```bash
# Minimal deploy (skip apple-touch-icon)
mkdir -p /tmp/canyouretire-deploy
cp public/index.html public/favicon.ico public/favicon.png /tmp/canyouretire-deploy/
wrangler pages deploy /tmp/canyouretire-deploy --project-name canyouretire --branch main
```

### DNS / Custom Domain

- Domain `jaye.es` is on Cloudflare (zone ID: `4908369a2c620db9bfee9079e19341d6`)
- CNAME record: `canyouretire` -> `canyouretire.pages.dev` (proxied)
- Wrangler OAuth token does NOT have DNS write scope -- DNS changes must be done via the Cloudflare dashboard

### Auth

Wrangler is authenticated via OAuth (`wrangler login`). Config at `~/Library/Preferences/.wrangler/config/default.toml`.
