# NOVAX QR Scanner — Store Strategy

## Primary Targets

**RuStore + Google Play** — QR Scanner is safe for both stores.

## Permissions

Allowed, low-risk permissions:
- `CAMERA` — required for scanning.
- Gallery access — for scanning QR from existing photo or saving generated QR.
  - On Android 10+ use **MediaStore** or **SAF** (Storage Access Framework).
  - Do **not** request `WRITE_EXTERNAL_STORAGE`; it is deprecated and triggers Google Play warnings.

## Billing

- If premium features are added, use store-appropriate billing:
  - RuStore → `flutter_rustore_billing`.
  - Google Play → `in_app_purchase`.
- Consider Flutter flavors when billing is introduced.

## Google Play Notes

- No sensitive file access.
- No background location or contacts.
- Low privacy risk category.
- Should pass review without special justification.

## RuStore Notes

- Standard utility app.
- No extra policy hurdles expected.
