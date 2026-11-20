# Kisan Haat

Farmer surplus marketplace — list leftover harvest, reserve nearby, settle **COD at the gate**.

| Link | URL |
|---|---|
| **Live site** | [kishanhatt.zyvor.dev](https://kishanhatt.zyvor.dev) |
| **API ready** | [kishanhatt.zyvor.dev/api/ready](https://kishanhatt.zyvor.dev/api/ready) |
| **Privacy** | [privacy.html](https://kishanhatt.zyvor.dev/privacy.html) |
| **How to** | [howto.html](https://kishanhatt.zyvor.dev/howto.html) |
| **Private source** | [zyvorai/kisan-haat](https://github.com/zyvorai/kisan-haat) |

## Guides

- [Deploy (VPS, SMS, EAS, edge)](./DEPLOY.md)
- [API server](./server.md)
- [Mobile / Expo](./mobile.md)
- [Operator desk](./admin.md)
- [Web static site](./web.md)
- [Store checklist](./STORE_CHECKLIST.md)

## Production snapshot (2026-11)

- Edge host exempt for Expo OkHttp / CFNetwork (no mobile 403)
- OTP via MSG91; `ALLOW_CONSOLE_SMS=false`
- Staff via `ADMIN_PHONES`
- Free for villagers: no plan fees, no commission (Cashfree plan billing is switched off, not offered)
- EAS `projectId` still needs `eas login` or `EXPO_TOKEN`
