# NOVAX Cleaner — Store Strategy

## Current Status

**Cleaner v2.0.2+22 — tested OK on Xiaomi 2407FPN8EG. App ID `2063733453` verified in RuStore Console. Version 2.0.0 is under RuStore moderation.**
- Quick Clean realistically finds cleanable junk.
- EULA, permissions, scan, clean, and trial/premium flows work.
- Per-ABI APKs are under 30 MB (arm64-v8a ~19.2 MB).
- **Latest corrected arm64 release APK:** `C:\dev\apps\novax-cleaner\build\app\outputs\apk\release\app-arm64-v8a-release.apk` (~19.3 MB,  
  SHA256 `13a4b3164e9ccc4dbd6fd7e977dbce8f08de1ee749f44de3570e1f2e557b1c2b`).  
  Copies: `C:\Users\Владимир\Downloads\novax-cleaner-v2.0.2+22-arm64.apk` and `C:\dev\releases\novax_cleaner_v2.0.2+22_arm64.apk`.
- Ready for RuStore submission after Z-review, real RuStore app ID and product setup.

## Primary Target

**RuStore** — main launch platform for NOVAX Cleaner.

Reasons:
- Allows `MANAGE_EXTERNAL_STORAGE` for cleaner/utility apps.
- Accepts RuStore Billing (`flutter_rustore_billing`).
- Less strict than Google Play about file-access utilities.

## Google Play Status

**Do NOT upload the current Cleaner build to Google Play.**

Two blockers:

1. **Billing SDK**
   - Current code uses `flutter_rustore_billing`.
   - Google Play requires all digital purchases through **Google Play Billing**.
   - Fix: implement Flutter flavors. RuStore build → `flutter_rustore_billing`; Google Play build → `in_app_purchase`.

2. **MANAGE_EXTERNAL_STORAGE**
   - Google Play allows "All Files Access" only for file managers, antivirus, backup apps.
   - Cleaners are generally rejected.
   - Even if approved, Google may require justification video and an expensive CASA security audit ($5k–$15k).
   - Fix: use **SAF (Storage Access Framework)** with `action.OPEN_DOCUMENT_TREE`, drop `MANAGE_EXTERNAL_STORAGE`, accept lower cleanable volume.

## RuStore Checklist Before Release

- [ ] Privacy policy URL configured in RuStore Console.
- [ ] EULA screen shown on first launch.
- [ ] Release signing keystore ready and not in git.
- [ ] Product `novax_cleaner_full_access` created as non-consumable, 159 ₽.
- [ ] `RustoreBillingClient.initialize("2063733453", "novaxcleaner://iamback")`.
- [ ] Per-ABI APKs under 30 MB each.

## Future Google Play Path

1. Add flavors: `rustore` and `googleplay`.
2. In `googleplay` flavor:
   - Replace RuStore Billing with `in_app_purchase`.
   - Rewrite `ScanService`/`CleanService` to use SAF.
   - Remove `MANAGE_EXTERNAL_STORAGE` permission.
3. Test SAF flow on Android 11+.
4. Prepare policy justification video.
5. Consider CASA audit only if Google explicitly requires it.
