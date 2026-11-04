# Kisan Haat — App Store / Play Store checklist

## Before first build

1. Install EAS CLI: `npm i -g eas-cli`
2. From `mobile/`: `eas login` then `eas init` — paste the new `projectId` into [app.json](app.json) `extra.eas.projectId`
   - Or set GitHub Actions secret `EXPO_TOKEN` (Expo → Access Token) and run workflow **EAS Build**; `project:init` will link and you can copy `projectId` from the Expo dashboard into `app.json`
   - Blocked until Expo login / `EXPO_TOKEN` exists (CLI reports “Not logged in”)
3. Confirm Apple Developer + Google Play Console accounts
4. Replace `submit.production.ios.ascAppId` in [eas.json](eas.json)
5. Add Play service account JSON as `google-service-account.json` (do not commit secrets)

## Build

```bash
cd mobile
eas build --platform android --profile production   # AAB
eas build --platform ios --profile production       # IPA
```

Preview APK for internal testers:

```bash
eas build --platform android --profile preview
```

Production env (already in eas.json):

- `EXPO_PUBLIC_USE_MOCK=false`
- `EXPO_PUBLIC_API_BASE_URL=https://kishanhatt.zyvor.dev/api`

## Privacy labels

| Permission | Why |
|------------|-----|
| Coarse location | Optional village suggestion |
| Camera | Crop scan + listing photos |
| Photos | Gallery crop pick |
| Phone number | OTP account |

Privacy URL: https://kishanhatt.zyvor.dev/privacy.html  
Terms / how-to: https://kishanhatt.zyvor.dev/howto.html

## Billing note for review

Platform membership (₹25/day or ₹399/month) is paid via Cashfree in the system browser. Harvest trade is cash-on-delivery at the farm gate — not digital goods / IAP.

## Smoke after install

1. Language → village → OTP login
2. Browse listings; open detail; reserve (buyer)
3. Sell gated until plan active → Plans → Cashfree → return → plan active
4. Accept / handover / cash recorded on a deal
5. Report from listing or deal
