# Kisan Haat web

Static site for **Kisan Haat** — field surplus to neighbour tables. COD only.

**Live:** [https://kishanhatt.zyvor.dev](https://kishanhatt.zyvor.dev)

## Pages

| File | Purpose |
|------|---------|
| `index.html` | Launch LP — brand hero, install, plans, features |
| `browse.html` | Marketplace — search, filters, listing drawer (`?listing=`) |
| `advertise.html` | List-surplus wizard |
| `register.html` / `login.html` | Account OTP |
| `billing.html` / `checkout.html` / `pay.html` | Platform plans + Cashfree |
| `features.html` | Research-backed feature guide |
| `howto.html` | Farmer checklist (hi/en packs) |
| `privacy.html` | Privacy policy |
| `lp.js` / `site.js` / `catalogue.js` | Install copy, browse, wizard, catalogue |
| `install.sh` | One-command bring-up script |

## Serve locally

```bash
npx --yes serve -l 4173
```

Open http://localhost:4173

## API base

Default same-origin `/api` (compose nginx proxy). Override:

```html
<script>window.KISAN_HAAT_API = "https://kishanhatt.zyvor.dev/api";</script>
```

Legacy: `window.HATIA_API` if `KISAN_HAAT_API` is unset.

## Docker

```bash
docker build -t ghcr.io/zyvorai/kisan-haat-web:latest .
docker run --rm -p 8080:80 ghcr.io/zyvorai/kisan-haat-web:latest
```

Compose publishes web on host port **30888** for website-server reverse proxy.
