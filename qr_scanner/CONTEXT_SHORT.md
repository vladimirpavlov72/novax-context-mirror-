# NOVAX QR — Quick Context

## Status: v2.1.0+2027 — залит в RuStore на модерацию 14.09 (светлая тема + переключатель темы). Опубликованная: v2.1.0+10, активно скачивается
## Repo: github.com/vladimirpavlov72/novax-qr-scanner (private)
## Package: com.novax.qrscanner
## App ID RuStore: 2063737357 (уточнить — возможно, заменён на новый аккаунт ИП)
## Product ID: novax_qr_full_access
## Price: 159₽ / $2.99 one-time (из консоли). Trial: 7 days. NO subscriptions.

## Latest
- **v2.1.0[СКРЫТО:ТЕЛ], коммит `2900cb8`)**: светлая тема + theme picker (системная/светлая/тёмная) по запросу дизайнера. Release APK 39.9 MB: `C:\dev\releases\novax_qr_v2.1.0+2027_arm64.apk`; консоль-пакет `Downloads\NOVAX_QR_v2.1.0+2027\` (светлые скрины 9:16 + полные с 14T Pro); старая папка `+10` → `_archive_`. На модерации.
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
1. Уточнить/обновить App ID в контекст-файлах и коде под новый аккаунт ИП (если отличается).
2. Sandbox-тесты платежей: ONE_STEP, restore, отмена шторки.
3. Патч v2.1.1+11: переводы 4 paywall-ключей делаем in-house (Рома+Владимир).
4. Мониторинг, отзывы, ASO-обновления.

## Roadmap
- **v2.1.1:** умная подпись при шаринге QR + ручное переименование записей истории (out of scope до публикации v2.1.0).
- Full plan: `ROADMAP.md`

## ⚡ Google Play (09.10)

Flavors-ветка `feature/play-flavors` в основном репо ЗАВЕРШЕНА: SDK 36,
BillingRepository (rustore/GP), Z-ревью + QA пройдены, AAB play + APK rustore
собраны (`C:\develeases\`). Пейвол: манифест против подписки с ценами по
флейвору. Подробности — `core/CHANGELOG.md` (записи 08–09.10) и DECISIONS.md.
