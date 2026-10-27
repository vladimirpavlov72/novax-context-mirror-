# Отчёт: Habits — уведомления (Z-чек-лист) + refund-отзыв premium

> Рома-2 — отчёт · 2026-10-16 · Сессия [СКРЫТО:ID]
> Репо: `C:\dev\apps\novax_habits`, ветка `feature/play-flavors` (master НЕ тронут).
> Refund-фикс портирован также на `novax_h2o` и `novax-qr-scanner` (те же ветки).
> Код НЕ закоммичен — ждёт Z-ревью (пакет свёрстан для Cleaner free-limit;
> этот блок можно смотреть прямо по diff'ам веток, файлы перечислены ниже).

---

## 1. Уведомления на A52 — БЛОКЕР ЗАКРЫТ ✅

Диагноз Z подтверждён и вылечен: Samsung Adaptive Battery резал inexact-
будильники sideload-приложения — напоминания не приходили с августа.

### Чек-лист Z — результат QA на A52 (RZ8R409QVQB, debug rustore v1.0.0+13):

| Пункт | Результат |
|---|---|
| `USE_EXACT_ALARM` в манифесте | ✅ выдано автоматически при установке (`granted=true`), `SCHEDULE_EXACT_ALARM` оставлен фолбэком |
| Exact-режим в системе | ✅ `dumpsys alarm`: `window=0 exactAllowReason=policy_permission` (раньше — обычный/inexact) |
| Напоминание приходит вовремя | ✅ привычка «QA», своё время 18:51 — уведомление доставлено в 18:51:00, канал `habit_reminders`, importance 4 |
| Boot-reschedule | ✅ после перезагрузки устройства будильник восстановлен плагиновым `ScheduledNotificationBootReceiver` (18.10→17.10 18:51, снова exact) |
| Кнопка-помощник «Напоминания не приходят?» | ✅ в настройках под тогглом уведомлений; честный диалог (12 локалей) → открывает системные настройки приложения (`InstalledAppDetails`) |
| Премиум-инъекция для QA | premium через prefs-surgery (debug-сборка), пейвол с манифестом «159 ₽ в год / разовая оплата» проверен в rustore-флейворе |

### Изменённые файлы (Habits)
- `android/app/src/main/AndroidManifest.xml` — `USE_EXACT_ALARM`
- `android/app/src/main/kotlin/com/novax/habits/MainActivity.kt` — канал `openAppSettings` (app details → fallback notification settings)
- `lib/services/platform_actions_service.dart` — `openAppSettings()`
- `lib/screens/settings_screen.dart` — плитка + диалог-помощник
- `lib/l10n/app_*.arb` — 3 ключа × 12 локалей (`notificationsHelpTitle/Message/Open`), SOURCE_KEYS.md обновлён
- `AGENTS.md`, `NOTIFICATIONS.md` — зафиксирован сменившийся подход (старый «не используем USE_EXACT_ALARM» помечен устаревшим)

⚠️ Для GP: задекларировать use case exact alarms в Play Console при релизе
(напоминания — core-функция трекера привычек).

## 2. Refund-отзыв premium ✅ (код, analyze чисто; runtime-ждёт sandbox)

Решение Владимира+Z 09.10: явный REFUNDED/CANCELLED отзывает локальный
premium (GP-политика); пустой список покупок — НЕ отзывает (защита от сбоев).

- Контракт: `hasActivePurchase` → `lookupPurchase` с `enum PurchaseLookup {active, revoked, notFound}`.
- RuStore (Habits/QR): `cancelled/rejected/refunded/refunding/reversed/expired` → revoked; `paid/confirmed` → active; pending-статусы не трогают.
- RuStore (H₂O, клиент `flutter_rustore_billing-0.0.5` со String-статусами): revoked = `CANCELLED`/`CLOSED` — других явных терминальных-неуспехов в этом SDK нет (зафиксировано в отчёте порта).
- Google Play: `_restoreRevoked`-флаг на явный `PurchaseStatus.canceled` в окне restore; иначе notFound → premium сохраняется.
- `BillingService._syncPurchases` в switch: revoked → снимается ТОЛЬКО купленный флаг, триал не трогается.
- Портировано на все 3 приложения, `flutter analyze` — 0 issues во всех трёх.
- Runtime-верификация revoke-пути требует sandbox-покупки+refund в сторе — **невозможна на A52 без RuStore/GP sandbox; отложено до появления консолей.**

## 3. Что осталось
- Z-ревью diff'ов Habits/H₂O/QR (refund + notifications) → коммиты в ветки.
- Пересборка релизов после Z-ок (Habits +13 или +14 — решить судьбу висящего
  на модерации RuStore +13).
- Sandbox-проверка refund на реальном сторе.


---

## Appendix: Z-ответы внесены (17.10, коммиты a2a6a78 + 531ff3d)

Z-ревью пакета `reviews/habits_notifications_refund/` пройдено, правки внесены:

- **Блокер 04 (helper на iOS/web):** кнопка-помощник обёрнута в
  `if (!kIsWeb && Platform.isAndroid)` — MissingPluginException больше
  недостижим на не-Android платформах.
- **02 MainActivity:** `Settings.EXTRA_APP_PACKAGE` /
  `Settings.ACTION_APP_NOTIFICATION_SETTINGS` вместо hardcoded строк;
  порядок fallback перевёрнут (сначала страница уведомлений — это цель
  кнопки); catch сужен до ActivityNotFoundException + SecurityException.
- **06 GP lookupPurchase (и порт в H₂O/QR):** ветка параллельного restore
  теперь возвращает `revoked`, когда `_restoreRevoked == true`
  (раньше молча сводилось к `notFound`).
- **08 (Q3) снэкбар об отзыве:** `PremiumState.revokedNotice` (one-shot
  флаг) → одноразовый неблокирующий snackbar в SettingsScreen с кнопкой
  повторной покупки (PaywallScreen). Портировано в H₂O и QR
  (`premiumRevokedNotice` ARB: Habits en/ru, H₂O en/ru, QR — 12 локалей).
- **09 ARB en:** «Unrestricted» в ASCII-кавычках вместо ёлочек.
- **Блокер 10 (H₂O RuStore):** `_isRevokedState` выровнен с Habits/QR —
  старый SDK (flutter_rustore_billing 0.0.5) не имеет констант
  REFUNDED/REJECTED/REVERSED/TERMINATED → сравнение литералами
  (defensive, raw-строка от бэкенда может их нести).
- **Архитектурный блокер GP refund-path (мёртв на GP):** задокументирован,
  код не менял — GP не возвращает refunded non-consumables через restore;
  вариант с сервером (RTDN) отсутствует по архитектуре (offline-only,
  NOVAX rules). Риск GP-страйка принят на уровне решения 09.10;
  на RuStore отзыв работает (6 статусов).
- Ответы Z: Q1 — декларация USE_EXACT_ALARM в GP-консоли по шаблону Z
  («Будильник или напоминание о времени» + скрин списка напоминаний);
  Q2 — pending = notFound (семантически верно, покрыто таймаутом);
  Q3 — снэкбар (внедрено); Q4 — блокеры выше закрыты.
- analyze: 0 errors на всех трёх приложениях.
- P2-юнит-тесты `_isRevokedStatus`/`_isSuccessfulStatus` — не делались
  (ред-линия «минимум пакетов», тестовая инфраструктура отсутствует) —
  зафиксировано как осознанный техдолг.

- Session ID: [СКРЫТО:ID]
