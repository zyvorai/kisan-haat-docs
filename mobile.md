# Kisan Haat — iOS & Android (Expo)

Farm-gate harvest marketplace app for farmers and neighbours. Hindi + Bhojpuri first (plus generated packs). Cash on delivery at the gate. Peer Call / WhatsApp / SMS on listings and deals.

> Scheme `kisanhaat://` · package `in.kisanhaat.app`.

**Production API:** `https://kishanhatt.zyvor.dev/api`  
**Store checklist:** [STORE_CHECKLIST.md](STORE_CHECKLIST.md)

## Run locally

```bash
cd mobile
npm install
npm start          # Expo dev server
npm run ios
npm run android
```

```bash
export EXPO_PUBLIC_API_BASE_URL=http://127.0.0.1:4310
export EXPO_PUBLIC_USE_MOCK=false
```

## Store / EAS builds

```bash
npm i -g eas-cli
eas login
eas init   # paste projectId into app.json → extra.eas.projectId
```

Or set repo secret `EXPO_TOKEN` and run GitHub workflow **EAS Build**.

| Profile | Use |
|---------|-----|
| `development` | Dev client + simulator / APK (mock-friendly local API) |
| `preview` | Internal APK / TestFlight; live `kishanhatt.zyvor.dev` |
| `production` | Store AAB / IPA; live API |

```bash
eas build -p android --profile preview
eas build -p ios --profile production
eas build -p android --profile production
```

## Identity

| Field | Value |
|-------|--------|
| Display name | Kisan Haat |
| Slug | `kisan-haat` |
| Scheme | `kisanhaat` |
| iOS bundle | `in.kisanhaat.app` |
| Android package | `in.kisanhaat.app` |
| Privacy | https://kishanhatt.zyvor.dev/privacy.html |
| How-to | https://kishanhatt.zyvor.dev/howto.html |
