# iOS Sandbox In-App Purchase Checklist: What I Wasted Hours Finding

I lost a few hours tracking these down, so I'm writing them down for you, or your agent, to read.

## From experience: easy to miss

- [ ] **Base country must match.** The base country in the in-app purchase's pricing must match the base country in the app's **Pricing and Availability** tab.
- [ ] **Select an age rating for the sandbox account on your device.** Set it in the sandbox account settings on the test device.
- [ ] **Developer is now a top-level item in Settings.** In the latest iOS, go to Settings → Developer, not Settings → Privacy & Security.

## The full checklist

Everything that must be true before a sandbox purchase can work. Work top to bottom and stop at the first failing gate.

## 1. Agreements (App Store Connect → Business)

- [ ] Paid Apps Agreement: **Active**
- [ ] Bank account: **Active**
- [ ] Tax forms: **Complete**

Products will not load in the sandbox until all three are done.

## 2. App record and App ID

- [ ] The app record's Bundle ID exactly matches the build.
- [ ] The app is available in the tester's storefront.
- [ ] The App ID is explicit (not a wildcard) and has In-App Purchase enabled.

## 3. The product (Monetization → In-App Purchases)

- [ ] Correct type (e.g. non-consumable for a one-time unlock).
- [ ] The product ID in code matches exactly (case-sensitive).
- [ ] Price set; base country matches the app's Pricing and Availability; availability includes the tester's storefront.
- [ ] Cleared for Sale is on.
- [ ] Display name and description localized; review screenshot added.
- [ ] Status is **Ready to Submit** or later. Sandbox testing does not need App Review.
- [ ] After any edit, allow up to 1 hour to propagate.

## 4. The build

- [ ] It includes your native StoreKit code or SDK. Expo Go and web previews do not.
- [ ] It is a TestFlight build, or a development build signed with a valid profile.
- [ ] It has no Xcode `.storekit` configuration file overriding the sandbox.
- [ ] For TestFlight: processing has finished and the build is assigned to the tester's group.
- [ ] If your app can use more than one purchase path (for example StoreKit or a third-party SDK chosen by a build flag), confirm which one this build uses.

## 5. If you use RevenueCat

- [ ] The app's bundle ID in RevenueCat matches the build.
- [ ] In-App Purchase Key (.p8) uploaded.
- [ ] Product imported and attached to an entitlement.
- [ ] The current offering contains the packages your code expects.
- [ ] The build uses the public Apple SDK key (`appl_…`), not a Test Store key.

## 6. Device

- [ ] For TestFlight: you can buy with your own Apple Account. TestFlight always uses the sandbox, so there is no charge.
- [ ] Or sign in to a sandbox account: Settings → Developer → Sandbox Apple Account. You do not need to sign out of your own account.
- [ ] The sandbox account's region is one where the product is available.
- [ ] An age rating is selected for the sandbox account on the device.
- [ ] Screen Time allows In-App Purchases.

## 7. Product-lookup gate — do not buy until this passes

- [ ] The product request returns your product, not an empty list.
- [ ] The localized price, fetched from StoreKit and not hard-coded, appears in the app.

If nothing comes back, recheck sections 1–5 and wait an hour. Then force-quit the app, because a cached empty result can persist.

## 8. Purchase

- [ ] The App Store sheet says **Sandbox**.
- [ ] The purchase completes and the content unlocks.
- [ ] The transaction is finished.
- [ ] Force-quit and reopen: the unlock persists.
- [ ] Delete, reinstall, Restore Purchases: the unlock returns.

## 9. Reset between runs

- **Sandbox account:** Settings → Developer → Sandbox Apple Account → Manage → Clear Purchase History, then reinstall.
- **Own Apple Account in TestFlight:** the purchase cannot be cleared. Use a sandbox account to test a fresh purchase.

## Reference

[TN3186 — Troubleshooting In-App Purchases availability in the sandbox](https://developer.apple.com/documentation/technotes/tn3186-troubleshooting-in-app-purchases-availability-in-the-sandbox)
