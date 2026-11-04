# Kisan Haat API

Fastify + PostgreSQL modular monolith. Listens on `0.0.0.0:4310`.

```bash
docker compose up -d db
cp .env.example .env
npm install
npm run migrate && npm run seed
npm run dev
npm test
```

Production compose: see root [`DEPLOY.md`](../DEPLOY.md). Live: `https://kishanhatt.zyvor.dev/api`.

### SMS OTP (India)

| `SMS_PROVIDER` | Behaviour |
|----------------|-----------|
| `console` | Logs OTP; returns `devCode` in `/auth/otp/request` (local/staging; needs `ALLOW_CONSOLE_SMS=true` in prod-shaped env) |
| `msg91` | [MSG91](https://msg91.com/) SendOTP v5 + DLT template — preferred for Zyvor GST KYC |
| `2factor` | [2Factor](https://2factor.in/) custom OTP |
| `fast2sms` | Fast2SMS bulk/OTP (API route needs ≥₹100 lifetime top-up) |
| `webhook` | `POST` `{ phone, code, message }` to `SMS_WEBHOOK_URL` |

Auth: `POST /auth/otp/request` → SMS, then `POST /auth/otp/verify` → session token.

### Notable routes

| Area | Paths |
|------|--------|
| Health | `GET /health`, `GET /ready` |
| Listings | `GET/POST /listings`, trust fields, buyer filters, photo upload |
| Markets | `GET /markets`, haat/mandi places |
| Ads | `GET/POST /ads`, withdraw |
| Billing | Cashfree orders + webhooks `/billing/*` |
| Me | profile, blocks, delete, `buyerRole` |
| Admin | `/admin/*` when token phone ∈ `ADMIN_PHONES` |
| Vision | `POST /vision/scan` (crop suggest) |
| Uploads | listing photos (base64) |

Migrations live under `src/db/sql/` (`011_listing_trust_fields`, `012_buyer_role`, …).
