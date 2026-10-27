# NOVAX Habits — Quick Context

> 30-second read for agents. Source of truth for the latest app state.
> Last updated: 2026-10-20 (блокеры уведомлений/модерации закрыты 16.10 — внесено Джумой по визе Гиа)

---

## ⚡ Google Play — актуальный трек (важно!)

**Два параллельных подхода существуют — выбор архитектуры за Владимиром (запрос 07.10):**
1. **gplay-копии (РЕЗЕРВ, НЕ ТРОГАТЬ — решение 09.10; история 14.09)**: отдельные репо `C:\dev\apps\gplay\novax_habits|novax_h2o|novax-qr-scanner`, свой Flutter 3.47.4 (`C:\dev\flutter_gp`), SDK 36 (требование GP!), биллинг переписан на in_app_purchase, RuStore вычищен, QA на A52 пройден, коммиты: Habits `3f4dcbc`+`7590280`, H₂O `8478512`, QR `df9a6a3`. Контекст: `gplay/CONTEXT_GPLAY.md`, `gplay/CONTEXT_SHORT.md`.
2. **Flavors в основном репо (наш эксперимент, 28.09)**: ветка **`feature/play-flavors`** в `C:\dev\apps\novax_habits` (НЕ master): flavors rustore/play, BillingRepository-абстракция, GooglePlayBillingRepository, AAB собран, Z-ревью пройдено. Master репо НЕ тронут (HEAD `a9856dc`). Плюс flavors: единый код, фиксы не дублируются. Минус: toolchain Flutter 3.24/targetSdk 34 — для GP нужен SDK 36 (надо поднимать).

**РЕШЕНО Владимиром 07.10: flavors в основных репо (подход №2). Трек ЗАВЕРШЁН 09.10:**
ветка `feature/play-flavors` (novax_habits): flavors rustore/play, SDK 36,
BillingRepository + GP-биллинг, Z-ревью ×2 пройдено, QA A52 пройден,
AAB play v1.0.0+13 собран (releases). Пейвол: манифест против подписки
(цены по флейвору через BillingRepository). **Открытый блокер перед GP:
уведомления не срабатывают на A52 (Samsung Adaptive Battery) — фикс по
чек-листу Z в работе (USE_EXACT_ALARM, boot-reschedule, кнопка-помощник).**

---

## Identity

- **Package**: `com.novax.habits`
- **Label**: RU `Привычки +` / EN `Habits +`
- **Brand color**: violet `#8B5CF6`
- **Font**: Inter (Regular / Medium / SemiBold / Bold)
- **RuStore App ID**: `2063741856` (новый аккаунт ИП Павлова В.Е., опубликовано и активно скачивается)
- **Product ID**: `novax_habits_full_access` (one-time, 159 ₽ / $2.99)
- **Trial**: 7 days, no subscription (с v1.0.0+12; раньше было 30)

---

## Current Version

**v1.0.0+13** — коммит `c67898b` (2026-09-11). RuStore-папка готова: `C:\Users\Владимир\Downloads\NOVAX_Habits_v1.0.0+13\` (APK + описание + скрины, APK совпадает с `C:\dev\releases\novax_habits_v1.0.0+13_arm64.apk`).

### Что нового в v1.0.0+13 (2026-09-11)
- **Фикс триал-баннера**: баннер с оставшимися днями пробного периода не показывался вообще — `isPurchased` проверялся вместо effective premium (isTrialActive). Исправлено.
- **14.09, коммит `a9856dc` (после сборки +13)**: фикс paywall — restore во время активного триала ложно показывал «Премиум разблокирован» (`_onPremiumChanged` проверял `isPremium`, true при триале). Теперь требуется `isPurchased`. Тот же фикс ушёл в GP-копию (этап 3). В опубликованной сборке +13 фикса нет — попадёт в RuStore с релизом +14.

### Что нового в v1.0.0+12 (2026-09-10)
- **Trial 30 → 7 дней**.
- Триал-баннер на главном экране + snackbar об окончании trial.
- Z-review fixes.

### Что нового в v1.0.0+11 (2026-08-21)
- **Фильтрация по дням недели**: на home-экране показываются только привычки, запланированные на выбранный день (`frequencyDays`).
- **Своё время напоминания**: 4-й chip «Своё время…» открывает модал с двумя `ListWheelScrollView` (часы/минуты) + haptic-щелчки. При выборе заменяет пресетные тайминги.
- **Выбор иконки**: в форме добавления/редактирования — кнопка с текущей emoji-иконкой, открывающая грид из 36 emoji.
- **Z-ревью fixes**: исправлен `copyWith(question)`, уведомления планируются по `dayOfWeekAndTime` с учётом `frequencyDays`, weekdays отображаются 2 буквами, добавлена защита от потери данных при corrupted SharedPreferences.

### Временно отключено
- **Награда за 21 день** — код оставлен (`lib/services/streak_service.dart`), но диалог не показывается. Вернём после решения основных блокеров.

### Known non-blocking
- Persistent Kotlin warning: binary metadata 1.9.0 vs expected 1.7.1 — APK собирается.

---

## Architecture (NOVAX standard)

- **State**: `StatefulWidget` + `ValueNotifier` only. NO BLoC / Riverpod / Provider / GetX / Firebase / go_router.
- **DI**: hand-written `ServiceLocator`.
- **Persistence**: `SharedPreferences`.
- **Billing**: RuStore Pay SDK 10.3.1 vendored at `C:/dev/vendor/packages/flutter_rustore_pay-10.3.1`.
- **Notifications**: `flutter_local_notifications` 16.3.2 vendored.
- **l10n**: 12 ARB locales, `flutter gen-l10n`, `synthetic-package: false`, `nullable-getter: true`.

---

## Active Blockers

| # | Blocker | Owner | Impact |
|---|---------|-------|--------|
| 1 | ~~Notifications не срабатывают на A52~~ ✅ ЗАКРЫТО 16.10 (REPORT_notifications_refund: USE_EXACT_ALARM, exact-режим, QA A52 пройден) | Roma | Закрыто. |
| 2 | ~~RuStore moderation rejection~~ ✅ ЗАКРЫТО: +13 ОПУБЛИКОВАН 13.10.2026, 100% аудитории (аудит 16.10) | Vladimir | Закрыто. |
| 3 | Adaptive Android icon vector asset | Designer | Не критично для повторной модерации. |

---

## Tech Debt

- `lib/services/streak_service.dart` и флаги наград в `StorageService` не используются — оставлены для будущего включения.
- Нужно перепроверить notifications и в H₂O, т.к. там та же библиотека `flutter_local_notifications` и схожая схема разрешений.

---

## Build & Deploy Cheat Sheet

```bash
# Switch to project
cd C:/dev/apps/novax_habits

# Generate l10n (after ARB edits)
flutter gen-l10n

# Analyze
flutter analyze --no-pub

# Release build
flutter build apk --release --target-platform android-arm64

# Copy to releases
cp build/app/outputs/flutter-apk/app-release.apk C:/dev/releases/novax_habits_v1.0.0+13_arm64.apk

# Install on Samsung A52
adb -s RZ8R409QVQB install -r C:/dev/releases/novax_habits_v1.0.0+13_arm64.apk
```

---

## Next Steps (short term)

1. ✅ ГОТОВО 16.10: уведомления (USE_EXACT_ALARM) — QA на A52 пройден, отчёт `habits/REPORT_2026-10-16_notifications_refund.md`.
2. ✅ ГОТОВО: +13 опубликован 13.10.
3. После фикса уведомлений — полный QA: фильтрация по дням, своё время, иконки, напоминание.
4. Проверить notifications в H₂O (аналогичная проблема возможна).
5. Переснять RuStore-скриншоты с новой функциональностью.
6. ✅ ГОТОВО: +13 залит и опубликован 13.10.2026.

---

## Files

- Code: `C:\dev\apps\novax_habits`
- Branch: `feature/habits-v11-improvements`
- Releases: `C:\dev\releases\`
- Screenshots: `C:\dev\releases\habits_screenshots\`
- Z-review: `C:\dev\novax-context-main\reviews\habits_v1.0.0+11\`
- Context: `C:\dev\novax-context-main\habits\`
