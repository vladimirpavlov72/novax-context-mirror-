# NOVAX Hydrolog / Drinkler — Store Strategy

## Primary Targets

**RuStore + Google Play** — low-risk utility, safe for both stores.

## Permissions

- Typically no sensitive permissions.
- Local notifications (optional) — use standard Flutter notification plugins.
- No file storage, no camera, no location, no contacts.

## Billing

- If premium/reminder features are added later, use store-appropriate billing:
  - RuStore → `flutter_rustore_billing`.
  - Google Play → `in_app_purchase`.

## Store Risk

- Very low.
- No expected Google Play policy blockers.
- Standard review process.
