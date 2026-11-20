# Kisan Haat operator desk

English desk for a moderation operator: sign in, read clusters and villages, open reports, audit, invoices, and staff actions.

Hindi and Bhojpuri stay in the mobile app. Staff access is gated by **`ADMIN_PHONES`** on the API (OTP accounts whose phone is listed).

## Run

```bash
cd admin
npm install
npm run dev
```

Open http://127.0.0.1:4317

Mock data is the default, so the desk runs with no API server. Sign in with any 10-digit number. The dev code is `123456`. Seeded pilot phones: Sunita `9876543210`, Ramesh `9876543211`, Geeta `9876543212`, Arun `9876543213`, Kavita `9876543214`.

## Live API

```bash
npm run dev:live
```

Or:

```bash
VITE_API_MODE=live VITE_API_BASE_URL=http://127.0.0.1:4310 npm run dev
```

Production API base: `https://kishanhatt.zyvor.dev/api` (set `VITE_API_BASE_URL` accordingly; CORS must allow the desk origin, or use the Vite proxy).

| Flag | Default | Meaning |
|---|---|---|
| `VITE_API_MODE` | `mock` | `mock` or `live` |
| `VITE_API_BASE_URL` | `http://127.0.0.1:4310` | API origin, no path prefix |

The Connection panel overrides those values for the current browser tab.

## Live staff capabilities

When the signed-in phone is in `ADMIN_PHONES`:

| Action | API |
|---|---|
| List / resolve / dismiss reports | `GET/PATCH /admin/reports` |
| Reservations needing attention | `GET /admin/reservations` |
| Audit log | `GET /admin/audit` |
| Invoices | `GET /admin/invoices` |
| Attach village to cluster | `POST /admin/villages/:id/cluster` |
| Suspend account / remove listing | admin suspend & remove routes |

Non-admin phones can still sign in and use account-scoped endpoints; the desk shows a limited reports view from the session cache.

Published support hours on the desk: 9:00–18:00 IST.
