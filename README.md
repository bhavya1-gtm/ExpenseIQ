# enseIQ — iOS and Android app

This is the native mobile project for the ExpenseIQ brief. It uses React Native, Expo SDK 58, TypeScript, local SQLite storage, and EAS build profiles. The previous `outputs/index.html` is a browser-only visual preview; this folder is the mobile app.

## Included now

- Native dashboard layout with spending chart, balances, budgets, insights, recent activity, and savings goals
- Activity search, offline expense entry, and SQLCipher-encrypted local SQLite persistence; the random key is stored with SecureStore in the device Keychain/Keystore
- Light/dark appearance toggle and native assistant sheet with sample-data answers
- iOS and Android app configuration, app icon, Android App Bundle production profile, and EAS submission profile
- No bank passwords, bank credentials, or transaction data are sent to a server by this demo

The visible financial records and AI answers are sample data. The app is suitable for internal device builds and product review once dependencies are installed. It is not yet a complete personal finance service: online authentication, server sync, licensed Open Banking / Account Aggregator consent, real AI, transaction editing/categorization, budgeting and goal creation, notifications, reports, and admin tools are not implemented. Do not present sample records as real account activity in a store listing.

## Run and create store builds

Use Node.js 22.13 or newer. From this folder:

```sh
npm install
npx expo install --fix
npx expo start
```

SQLCipher is a native module and does not run in the standard Expo Go client. Use a development build (`npx eas-cli build --profile development --platform android` or `ios`) or a production store build for runtime review.

Before building, replace the example reverse-domain identifiers in `app.json` with identifiers controlled by the publisher, then link this folder to the publisher's Expo account:

```sh
npx eas-cli login
npx eas-cli init
npx eas-cli build --platform android --profile production
npx eas-cli build --platform ios --profile production
```

The Android production artifact is an `.aab`. EAS creates the signed iOS `.ipa` using the developer's Apple signing credentials. Upload Android to a Play Console testing track and iOS to TestFlight first. EAS Submit can upload a completed binary; it does not fill in store listing content, pass review, or release the app automatically. See [Expo's store submission guide](https://docs.expo.dev/deploy/submit-to-app-stores/) and [iOS EAS submission guide](https://docs.expo.dev/submit/ios/).

## Checklist before a public store submission

1. **Publisher identity:** Select and verify the final application name, unique iOS bundle ID, unique Android application ID, publisher/team, icon, screenshots, and support contact. `com.expenseiq.ai` is an example and is not guaranteed to be available.
2. **Store accounts:** Enroll in Apple Developer Program and Google Play Console, create the app records, then complete EAS linking and signing credentials. Keep signing keys and access tokens out of this repository.
3. **Privacy:** Publish a real privacy policy and support URL. Accurately disclose device-local financial data and any SDKs in Apple privacy details and Google Play Data safety. If/when networking, AI, analytics, crash reporting, or bank connections are added, update disclosures and consent screens to match actual data flows before release.
4. **Financial functionality:** Obtain the necessary provider agreement, production credentials, user consent workflow, security review, and jurisdiction-specific legal review before enabling bank data. The current “Connect” control explains this integration is not active.
5. **Review readiness:** Provide review notes, tested reviewer account/demo mode if sign-in is later required, accurate screenshots, content rating, export compliance answers, and a tested account/data deletion workflow if accounts are introduced.
6. **Quality gate:** Install dependencies, run `npx expo install --fix`, `npx tsc --noEmit`, `npx expo-doctor`, and a release build on both platforms; manually exercise VoiceOver/TalkBack, large text, offline use, and current iOS/Android devices. Those checks have not been run in this environment.

## Data and security boundary

Transactions entered here are stored in an SQLCipher database on the device. Its generated key is stored with the platform secure storage API. There is no app account, API, cloud sync, or actual AI request. Do not add credentials or bank access tokens to the app bundle. A production bank integration must redirect to an approved provider, record and explain consent, keep provider secrets server-side, encrypt tokens using managed key services, validate webhook signatures, and provide a revocation/deletion path. A production AI endpoint should send only consented, minimized context and must never be needed for deterministic amount calculations.

## Production architecture still needed for the full brief

- Authenticated backend and PostgreSQL tenant model with ownership checks, audit events, and user deletion/export workflows
- Licensed bank-data provider adapter and consent lifecycle; no credential collection from banking sites
- API-side transaction sync, deduplication, categorization, recurring expense detection, and server-side aggregation
- Budget/goal CRUD, analytics/report generation, notification delivery, and receipt handling
- AI service with secrets kept server-side, user controls, safe prompt construction, and grounded answers
- Threat model, privacy/legal review, monitoring, backups, incident response, and accessibility/device QA
