# Kisan Haat

Farmer surplus marketplace. When harvest outruns the mandi, **kisans list leftover vegetables**, nearby buyers reserve, and everyone settles **cash on delivery (COD) only**.

- **Live:** [kishanhatt.zyvor.dev](https://kishanhatt.zyvor.dev) · API [`/api`](https://kishanhatt.zyvor.dev/api/ready)
- **Docs (GitHub Pages):** [zyvorai.github.io/kisan-haat-docs](https://zyvorai.github.io/kisan-haat-docs/)
- **Private code:** [zyvorai/kisan-haat](https://github.com/zyvorai/kisan-haat)

[Client overview (PDF)](docs/Kisan-Haat-Client-Overview.pdf)

## Stack

| Piece | Path | Notes |
|---|---|---|
| API | `server/` | Fastify + PostgreSQL — OTP auth, listings, reservations, ads, Cashfree billing, admin |
| Mobile | `mobile/` | Expo 58 (Hindi / Bhojpuri / English + more), offline-first |
| Operator desk | `admin/` | Moderation UI; staff phones via `ADMIN_PHONES` |
| Marketing + browse | `web/` | Static site + `/api` same-origin proxy |
| Compose | `docker-compose.yml` | Production path on VPS |
| k3s | `deploy/k3s/` | Namespace `hatia`, images `ghcr.io/zyvorai/kisan-haat-*` |

## Payments

**Harvest sales (kisan ↔ buyer):** COD only at the gate. Peer Call / WhatsApp / SMS from listing & deal screens.

**Free for villagers: no plan fees, no commission.** The old daily and monthly plans (`plan_daily`, `plan_monthly`, paid via Cashfree) are switched off and not offered; nothing is charged. The owner's decision is that the app is free.

Web: `register.html` · `billing.html` · `checkout.html` · `pay.html` · API `/billing/*`

## Clone

```bash
gh repo clone zyvorai/kisan-haat
# or
git clone git@github.com:zyvorai/kisan-haat.git
```

## Local run

```bash
cd server && cp .env.example .env
docker compose up -d db
npm install && npm run migrate && npm run seed && npm run dev

cd ../web && npx --yes serve -l 4173
cd ../admin && npm install && npm run dev
cd ../mobile && npm install && npm run web
```

API: `0.0.0.0:4310`.

## Deploy (production)

Full VPS / Docker / EAS / k3s runbook: **[DEPLOY.md](./DEPLOY.md)**.

Current production host: **`kishanhatt.zyvor.dev`** (website-server reverse-proxies compose web `:30888` + API `:4310`). Edge bot-block skips kishanhatt hosts so Expo OkHttp/CFNetwork can reach `/api`.

```bash
cp .env.deploy.example .env   # OTP_PEPPER, DB password, MSG91_*, ADMIN_PHONES
docker compose up -d --build
curl -fsS http://127.0.0.1:4310/ready
```

## SMS OTP (India)

Prefer **MSG91** (`SMS_PROVIDER=msg91` + `MSG91_AUTH_KEY` + DLT `MSG91_TEMPLATE_ID`). Alternatives: `2factor`, `fast2sms` (needs ≥₹100 lifetime top-up before API), `webhook`. Production: `ALLOW_CONSOLE_SMS=false`.

## Farmer features (research-backed)

See `reports/` and live guide `web/features.html` · `web/howto.html`.

| Feature | Why |
|---|---|
| Free-form sell + haat/mandi markets | List any crop; attach pickup → market |
| Trust fields (grade, harvest date, photo) | Quality discovery |
| Buyer role (household / kirana / hotel / FPO) | Better matching |
| COD only | Trust; uneven UPI in villages |
| Call / WhatsApp / SMS peer contact | Low-literacy path |
| Hindi ↔ English (+ packs) | Inclusion |
| Daily / monthly registration fee | Platform revenue |

## Docs site

Public documentation is published from the sibling repo **[zyvorai/kisan-haat-docs](https://github.com/zyvorai/kisan-haat-docs)** (GitHub Pages). Source markdown also lives under `docs/` in this repo — sync with `scripts/sync-docs-pages.sh`.

## k3s

See `deploy/k3s/README.md`.
