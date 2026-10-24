# Отчёт: эталонный харднинг сиротских алармов (уведомления Привычки+)

**Дата:** 2026-10-24
**Автор:** Рома-2 (платформенный контур)
**Контекст:** эскалация Ромы-3 из QA P1 (отчёт `REPORT_2026-10-23_notifications_off_fix.md`,
коммит `fb4a126`). cancelAll плагина flutter_local_notifications 16.3.2 отменяет
будильники ТОЛЬКО по кэшу `scheduled_notifications`; при пустом кэше — ранний
возврат. Факт с A52: в AlarmManager висят сироты прошлых сессий вне кэша —
тумблер их не отменяет.

## ⚠️ Поправка к ранее зафиксированному дизайну (честность прежде всего)

В MAILBOX (сообщение от 24.10 14:10) я зафиксировал дизайн «AlarmManager.cancel
по компоненту ScheduledNotificationReceiver убивает ВСЕ алармы». **Это
технически неверно** — проверил перед кодом: идентичность PendingIntent =
`Intent.filterEquals` **плюс requestCode** (requestCode = notification id —
часть идентичности, подтверждено AOSP/документацией). Одним cancel по
компоненту сироты НЕ убить — нужна полная выборка id, а id у нас хэш-диапазон
32 бит, неперечислим.

## Финальный дизайн (эталон, реализован)

**Рычаг — отключение компонента-получателя.** Пока master-тумблер выключен,
платформа переводит `com.dexterous.flutterlocalnotifications.ScheduledNotificationReceiver`
в `COMPONENT_ENABLED_STATE_DISABLED`:

- сиротский аларм срабатывает → broadcast в выключенный компонент → система
  глотает доставку, уведомление НЕ постится;
- one-shot аларм при срабатывании выбывает из очереди → сироты
  **самоочищаются** за одни сутки (все серии);
- ребут стирает AlarmManager целиком (закон Android);
- `DONT_KILL_APP` — вызов из своего процесса, приложение не убивается;
- класс ресолвится через `Class.forName` — переименование плагином в будущем
  даст graceful-false, а не crash.

**Off-пути** (toggle-off в settings, реконсиляция в ServiceLocator.init):
`NotificationService.cancelAll()` = `_plugin.cancelAll()` (чистка кэша)
+ `PlatformActionsService.hardCancelReminders()` (disable компонента +
best-effort cancel id-0).

**On-пути** (launch-reschedule, toggle-on, save привычки): chokepoint —
`scheduleHabitReminder()` сам зовёт `ensureRemindersReceiverEnabled()`
(идемпотентно, Android-only, web/iOS не трогаются).

## Файлы

- `android/.../MainActivity.kt` — `hardCancelReminders()`,
  `setScheduledReceiverEnabled(enable)`; 2 новых метода канала
  `com.novax.habits/platform`.
- `lib/services/platform_actions_service.dart` — `hardCancelReminders()`,
  `ensureRemindersReceiverEnabled()` (try/catch + debugPrint, как у
  соседних методов).
- `lib/services/notification_service.dart` — хуки в `cancelAll()` и
  `scheduleHabitReminder()` с Android-гардом (`kIsWeb` + `Platform.isAndroid`),
  зеркало гейта `remindersAllowed`.

Код Ромы-3 (fb4a126) не тронут — только добавочные хуки.

## Проверки

- `flutter analyze` — 0 issues.
- `flutter test test/notifications_off_test.dart` — 7/7 зелёные (его тесты
  чистой логики не затронуты: хардинг — платформенный слой).
- debug-сборка rustore — ок (хэш в коммите).

## Остаточные риски (честно)

1. Пока тумблер ВКЛ, сирота от удалённой в прошлой сессии привычки теоретически
   может один раз показать «призрак» — off-путь её не выключал. Митигация:
   её срабатывание за off-период глушится receiver'ом; за on-период —
   прецедент B6 QA: призраки Samsung в dumpsys не firing. Полного перебора id
   не существует — это потолок платформы.
2. Отключённый компонент сбрасывается на ENABLED при обновлении пакета
   (системное поведение) — но off-флаг персистится и реконсиляция на старте
   после обновления вызовет cancelAll → disable снова.

Session ID: [СКРЫТО:ID]
