# NOVAX QR — Quick Context

## Status: v2.1.0+10 — новая сине-циановая тема, APK загружен в RuStore
## Repo: github.com/vladimirpavlov72/novax-qr-scanner (private)
## Package: com.novax.qrscanner
## App ID RuStore: 2063737357
## Product ID: novax_qr_full_access
## Price: 159₽ / $2.99 one-time (из консоли). Trial: 7 days. NO subscriptions.

## Latest
- Пересборка по запросу дизайнера: новая сине-циановая палитра и единый градиентный `GradientButton` (#38BDF8 → #1D4ED8).
- Версия поднята с `2.1.0+8` до `2.1.0+10`.
- Для русской локали убран зачёркнутый old price (799 ₽); на paywall остаётся только актуальная цена 159 ₽.
- Исправлен критичный баг: сканирование не сохранялось в историю из-за не-await `PrefsService.addToHistory()` в `scanner_screen.dart`.
- Проведено Z-ревью v2.1.0+10; все BLOCKER-ы и WARNING-и исправлены:
  - Wi-Fi парсинг с учётом escape-символов.
  - Видимый ripple на `GradientButton`.
  - `mounted`-проверки в `scanner_screen.dart`.
  - Дебаунс перегенерации QR в `create_qr_screen.dart`.
  - Android 13+ permission logic (`Permission.photos`).
  - Динамические шрифты: глобальный clamp text scale до 1.2x + `FittedBox` на кнопках/цене.
- `flutter analyze` чистый (кроме info `const` и warning vendored `flutter_lints`).

## Latest Release APK (arm64-v8a)
- **Project path:** `C:\dev\apps\novax-qr-scanner\build\app\outputs\flutter-apk\app-arm64-v8a-release.apk` (~25.3 MB)
- **Releases:** `C:\dev\releases\novax_qr_v2.1.0+10_arm64.apk`
- **Downloads:** `C:\Users\Владимир\Downloads\novax_qr_v2.1.0+10_arm64.apk`

## QA Xiaomi 2407FPN8EG (2026-08-04)
- Установка release APK v2.1.0+10: ✅
- Новая тема на home / paywall / create / result / history / settings: ✅
- Сканирование → запись в историю: ✅
- Скриншоты новой темы сняты и подготовлены для RuStore (1080×1920 JPG).

## Next
1. RuStore: пройти модерацию v2.1.0+10.
2. Создать in-app товар `novax_qr_full_access` в консоли (после регистрации ИП / модерации).
3. Sandbox-тесты платежей: ONE_STEP, restore, отмена шторки.
4. Патч v2.1.1+11: переводы 4 paywall-ключей от Гермеса (если придут).

## Roadmap
- **v2.1.1:** умная подпись при шаринге QR + ручное переименование записей истории (out of scope до публикации v2.1.0).
- Full plan: `ROADMAP.md`
