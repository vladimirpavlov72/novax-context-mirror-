# KNOWN_ISSUES.md — Известные проблемы NOVAX

> Дата: 2026-08-05
> Правило: для каждой проблемы — влияние и обходное решение.

---

## 0. gen-l10n НЕ подставляет `#` в ICU-plural рукавах (17.10)

**Влияние:** сообщение вида `{days, plural, one{# день} other{# дней}}` рендерит
буквальную строку «# дней» — плейсхолдер не заменяется числом.

**Обходное решение:** использовать явный плейсхолдер в рукавах:
`{days, plural, one{{days} день} other{{days} дней}}`.

**Постоянное решение:** то же — зафиксировано как обязательный паттерн для
всех будущих ICU-plural в ARB. Поймано QA 17.10 на Cleaner v2.3.0.

---

## 1. `qr_code_scanner` требует ручных правок в pub cache

**Влияние:** После каждого `flutter pub get` правки в пакете могут слетать. Сборка падает с ошибками namespace / Java target.

**Обходное решение:**
- Вручную добавлять `namespace` в `android/build.gradle` пакета.
- Менять `compileOptions` на Java 17 и `kotlinOptions.jvmTarget = '17'`.
- Кэшировать изменённую версию в `C:\dev\vendor\packages\`.

**Постоянное решение:**
- Перейти на `mobile_scanner` или другой пакет с нормальной поддержкой AGP 8 / namespace.
- Или форкнуть `qr_code_scanner` и использовать свой форк.
- **План:** при работе над v2.0.21 перейти на `mobile_scanner`; `qr_flutter` и `mobile_scanner` завендорить в `C:\dev\vendor\packages\` через `vendor_add.sh` (pub.dev заблокирован).

---

## 3. NOVAX Cleaner — активные замечания / техдолг

### 3.0 `CardTheme` deprecated — требует Flutter ≥ 3.29 (Z-review v2.1.2)
- **Влияние:** `CardTheme(...)` в `app.dart` устареет при апгрейде Flutter; замена на `CardThemeData` невозможна на текущем Flutter 3.24 (класс отсутствует).
- **Обходное решение:** Оставить как есть — работает без предупреждений на 3.24.
- **Постоянное решение:** При апгрейде Flutter заменить на `CardThemeData` в `_buildLightTheme`/`_buildDarkTheme`.

### 3.1 Pre-existing warnings (не фиксили в v2.1.2)
- `unused_import` в `scan_result.dart`, unused fields `_isCheckingPermission`, `_cleanService`, `_scanService`, `_currentCategory`; info-линты `always_use_package_imports` / `prefer_const_*` по проекту.

---

## 2. NOVAX Habits — активные замечания / техдолг

### 2.0 Inexact-режим + matchDateTimeComponents нестабилен (Z-review v1.0.0+12)
- **Влияние:** Если пользователь не выдал exact-alarm permission (Android 14+), напоминания планируются в `inexactAllowWhileIdle`. Комбинация `inexact + matchDateTimeComponents: dayOfWeekAndTime` работает нестабильно — повторы недельных уведомлений могут приходить неточно или сбрасываться.
- **Текущее состояние (v1.0.0+12):** fallback на inexact вместо пропуска планирования — уведомления не отключаются полностью.
- **Постоянное решение:** Отказаться от `matchDateTimeComponents` в inexact-режиме: планировать только ближайшее срабатывание и перепланировать следующее при открытии приложения / тапе по уведомлению (`ServiceLocator.init()` уже перепланирует все напоминания на старте).

### 2.1 Kotlin metadata warning (non-blocking)
- **Влияние:** При сборке warning: binary metadata 1.9.0 vs expected 1.7.1.
- **Обходное решение:** Игнорировать; release APK собирается и подписывается корректно.
- **Постоянное решение:** Обновить/синхронизировать Kotlin Gradle plugin при возможности.

### 2.2 `paywallOldPrice` пустой
- **Влияние:** Old-price anchor на paywall не отображается.
- **Обходное решение:** UI отключает old price до бизнес-решения.
- **Владелец:** Кими / Владимир.

### 2.3 ADB gestures не работают на Flutter `InkWell` / `Dismissible`
- **Влияние:** Нельзя автоматизировать тапы/свайпы для QA через `adb shell input`.
- **Обходное решение:** Тестировать жесты вручную на устройстве.
- **Устройства:** Samsung A52, Xiaomi 14.

### 2.4 Xiaomi 14 установка через adb install
- **Влияние:** `adb install` падает с `INSTALL_FAILED_USER_RESTRICTED`.
- **Обходное решение:**
  ```bash
  adb push <apk> /data/local/tmp/novax_habits.apk
  adb shell pm install -r -f /data/local/tmp/novax_habits.apk
  ```

### 2.5 widget_test «App launches» падает: RenderFlex overflow на онбординге (23.10)
- **Влияние:** `flutter test` целиком красный из-за `test/widget_test.dart`
  (overflow на `onboarding_screen.dart:60`, Column переполняет тестовый
  вьюпорт 800×600). Сборку/APK не блокирует — падает только тест-раннер.
- **Обходное решение:** прогонять целевые тесты по файлам
  (`flutter test test/<file>.dart`), общий результат игнорировать.
- **Постоянное решение:** ремонт вёрстки онбординга (Expanded/scroll).
  **Не скоуп P1 напоминаний** — найдено Ромой-3 при фиксе 23.10,
  зафиксировано в бэклог по поручению Гиа.

---

## Архив решённых проблем

| # | Проблема | Решение | Дата |
|---|----------|---------|------|
| — | GitHub push на Windows | Работает через HTTPS + PAT | 2026-07-22 |
| — | Cleaner не собирается на Windows | Убран `generate: true`, Java/Kotlin 17, signing настроен, v2.0.2+22 собран | 2026-07-22 |
| — | `flutter_rustore_billing` не интегрирован | Интегрирован в Cleaner v2.0.2 | 2026-07-22 |
| — | Новая иконка QR Scanner не получена | Иконка от Саши финальная | 2026-07-22 |
| — | Cleaner v2.0.2: отсутствовал `confirm()` покупок RuStore | Добавлен `RustoreBillingClient.confirm()` в `_syncPurchases()` и `_handlePaymentResult()` | 2026-07-27 |
| — | Cleaner v2.0.2: неверный порядок billing init | `initialize()` теперь вызывается перед `available()` | 2026-07-27 |
| — | Cleaner v2.0.2: raw exceptions в UI | `$e` убрано из `errorMessage`, оставлено только в `debugPrint` | 2026-07-27 |
| — | Cleaner v2.0.2: `setState` без `mounted` и hardcoded OK | Добавлены `mounted`-чеки; `"OK"` заменён на `l10n.ok` | 2026-07-27 |
| — | Cleaner v2.0.2: Z-ревью не закрыто | Z-пакет отправлен в chat.z.ai, вердикт — все блокеры закрыты | 2026-07-27 |
| — | Windows Developer Mode отключался | Включён Developer Mode, прописано правило VPN + symlink support | 2026-07-27 |
| — | Vendored flutter_local_notifications не собирался | Патч `bigLargeIcon((Bitmap) null)` задокументирован в `VENDOR_PATCHES.md` | 2026-07-27 |
| — | Xiaomi отменяла adb-установку | Настройка "Install via USB" добавлена в чек-лист QA | 2026-07-27 |
| — | Habits: привычка сохранялась, но не отображалась на HomeScreen | Добавлен `_viewModel.load()` после возврата с AddHabitScreen | 2026-08-01 |
| — | Habits: MainActivity package mismatch | Файл перенесён в `android/app/src/main/kotlin/com/novax/habits/`, package обновлён | 2026-08-01 |

## Issue: Переводческий долг QR (v2.0.22)
- `eulaBody` — EN fallback на 11 локалях (решение 26.07: юртекст автопереводом не трогаем)
- `loading`, `shareQrText` — EN fallback, автоперевод отложен по решению 26.07
- Исполнитель: in-house (Рома+Владимир); вычитка Кими

## Issue: Домен novaxhub.ru не верифицирован (блокер GEO) — РЕШЕНО
- Сайт мигрирован на собственный VPS `[СКРЫТО:IP]` (nginx + SSL Let's Encrypt).
- GitHub Pages больше не используется как primary хостинг; TXT-запись для GitHub Pages не требуется.
- GEO-пакет (`llms.txt`, Schema.org, интент-лендинги) задеплоен и доступен.
