# Deploy Kisan Haat

Production path: **Docker Compose on a VPS** (API + Postgres + web), fronted by Zyvor **website-server** (TLS + host routing). Optional: admin desk, k3s, EAS store builds.

**Live today:** [https://kishanhatt.zyvor.dev](https://kishanhatt.zyvor.dev) · API [https://kishanhatt.zyvor.dev/api/ready](https://kishanhatt.zyvor.dev/api/ready)

Public docs: [zyvorai.github.io/kisan-haat-docs](https://zyvorai.github.io/kisan-haat-docs/)

## One command (clone)

```bash
./install.sh
```

The installer checks Docker, writes `.env` (with a generated `OTP_PEPPER` if missing), runs `docker compose up -d --build`, and waits for `/ready`.

## 1. VPS (Docker Compose)

### Prerequisites

- Docker Engine 24+ with Compose plugin
- DNS: `kishanhatt.zyvor.dev` (or your brand domain) → VPS
- Open ports `80`/`443` on the edge; compose publishes API `:4310` and web `:30888` on loopback/LAN as needed

### Bring up (manual)

```bash
cp .env.deploy.example .env
# Edit OTP_PEPPER, POSTGRES_PASSWORD, MSG91_*, ADMIN_PHONES, CORS_ORIGINS, Cashfree keys

docker compose up -d --build
curl -fsS http://127.0.0.1:4310/ready
curl -fsS http://127.0.0.1:30888/healthz
```

| Service | Local | Public |
|---|---|---|
| Web | http://HOST:30888 | https://kishanhatt.zyvor.dev |
| API | http://HOST:4310 | https://kishanhatt.zyvor.dev/api |
| Admin (optional) | `docker compose --profile admin up -d --build` → :8081 | — |

Browsers call **`/api/*`** on the same origin; nginx (web) and/or website-server proxy to the API container.

### Edge / bot block

Zyvor `website-server` blocks scraper UAs (`curl`, `okhttp`, …) on main marketing hosts. **Kisan Haat hosts** (`kishanhatt.zyvor.dev`, `kisanhaat.zyvor.dev`) are exempt so Expo Android (OkHttp) and iOS (CFNetwork) can call the API. After changing `botblock.go`, rebuild and restart the website container.

### Production SMS

1. Set `SMS_PROVIDER=msg91`
2. Set `MSG91_AUTH_KEY` and DLT `MSG91_TEMPLATE_ID` (sender / PE as registered)
3. Set `ALLOW_CONSOLE_SMS=false`
4. Keep Fast2SMS keys as optional fallback (`fast2sms` needs a ≥₹100 lifetime wallet top-up before the API route works)

Staging-only: `SMS_PROVIDER=console` + `ALLOW_CONSOLE_SMS=true` (OTP in API logs / `devCode`).

### Staff phones

`ADMIN_PHONES=+91…,+91…` — OTP accounts on this list may call `/admin/*` (reports, audit, invoices, cluster attach, suspend, remove listing).

### Vision (crop scan)

Enabled by default (`VISION_ENABLED=true`). Needs ~1.5 GiB RAM (`API_MEM_LIMIT`). First request downloads MobileNet into the `kisan-transformers-cache` volume. Disable on tiny hosts:

```env
VISION_ENABLED=false
API_MEM_LIMIT=512m
```

### TLS

Production uses website-server TLS on the host. Set `CORS_ORIGINS` to your HTTPS origins (include `https://kishanhatt.zyvor.dev`).

---

## 2. Mobile (EAS)

```bash
cd mobile
npm install
npx eas login
npx eas init   # paste projectId into app.json → extra.eas.projectId
```

Or set GitHub Actions secret `EXPO_TOKEN` and run workflow **EAS Build**.

Profiles in `eas.json` already point preview/production at:

- `EXPO_PUBLIC_API_BASE_URL=https://kishanhatt.zyvor.dev/api`
- Privacy / how-to on the same host

```bash
npx eas build --platform android --profile preview
npx eas build --platform all --profile production
```

Submit credentials go in `eas.json → submit.production` (App Store Connect id + Play service account). Checklist: `mobile/STORE_CHECKLIST.md`.

---

## 3. k3s

See `deploy/k3s/README.md`. Apply secrets from `secret.example.yaml`, then kustomization. API probes use `/ready` and `/health`; memory limit is 1536 Mi for vision.

Images:

- `ghcr.io/zyvorai/kisan-haat-api:latest`
- `ghcr.io/zyvorai/kisan-haat-web:latest`
- `ghcr.io/zyvorai/kisan-haat-admin:latest`

---

## 4. Health checks

| Endpoint | Meaning |
|---|---|
| `GET /health` | Process up |
| `GET /ready` | Postgres reachable |
| `GET /healthz` (web) | nginx up |
| `GET https://kishanhatt.zyvor.dev/api/ready` | Public edge + API + DB |

---

## 5. Checklist before go-live

- [x] Strong `OTP_PEPPER` and DB password (rotate off defaults)
- [x] `SEED_ON_START=false`
- [x] Real SMS provider (`msg91`) + `ALLOW_CONSOLE_SMS=false`
- [x] `ADMIN_PHONES` set for operator desk
- [x] `CORS_ORIGINS` includes live HTTPS origin
- [x] Mobile `eas.json` API URL → `kishanhatt.zyvor.dev/api`
- [ ] EAS `projectId` set (`eas login` / `EXPO_TOKEN`)
- [ ] MSG91 wallet / DLT PE monitored for delivery
- [ ] Store listing + privacy URL on Play / ASC
- [ ] Backups for Postgres volume
