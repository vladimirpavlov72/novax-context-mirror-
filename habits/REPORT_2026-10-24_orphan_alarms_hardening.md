# Эскалация: сиротские алармы вне кэша плагина — находка QA + предложение харднинга

> Рома-3 · 2026-10-24 · найдено при QA P1 на A52 (RZ8R409QVQB)
> Контур: паттерн уведомлений → эскалация Роме-2 (платформа). Доклад для Z-оценки.

## Находка

`cancelAllNotifications` во flutter_local_notifications 16.3.2
(vendored, `FlutterLocalNotificationsPlugin.java:1744`) отменяет будильники
ТОЛЬКО по своему кэшу `scheduled_notifications` (SharedPreferences):

```java
ArrayList<NotificationDetails> scheduledNotifications = loadScheduledNotifications(...);
if (scheduledNotifications == null || scheduledNotifications.isEmpty()) {
    result.success(null);
    return;   // ← AlarmManager не тронут вообще
}
```

Алармы, чьи id неизвестны кэшу, выключение напоминаний не отменяет никогда.

## Факт на A52 (24.10)

- После `adb uninstall` + чистой установки debug-сборки в AlarmManager
  остались будильники прошлой сессии (18:51/20:59, серии до 29.10) —
  Samsung не снял их при удалении пакета, кэш плагина при установке пуст.
- `dumpsys alarm` показывает отменённые будильники как «призраков» (A52),
  поэтому критерий отмены — факт не-срабатывания + состояние кэша:
  QA-2 в 12:46 после cancelAll НЕ сработала (доказательство эффективности
  отмены по кэшу), призраки в dumpsys при этом остались.

## Риск

Пользователь с потерянным кэшем (очистка данных / сбой записи кэша /
особенности OEM) выключает напоминания — известные алармы отменятся,
«сироты» продолжат приходить. Это потенциальный хвост жалобы P1
(гипотеза №4 брифа: «удалила/выключила, а запланированное осталось»).
Вероятность низкая, эффект = повторение P1-жалобы.

## Предложение харднинга (на визу Ромы-2)

В off-пути (`toggleNotifications(false)` и реконсиляцию init) после
`cancelAll()` добавить кэш-независимую отмену по известному id-пространству:

```dart
await notifications.cancelAll();
final habits = storage.getHabits().where((h) => !h.isDeleted);
for (final h in habits) {
  await notifications.cancelHabitReminder(
    h.id,
    reminderTimes: h.reminderTimes,
    frequencyDays: h.frequencyDays,
  );
}
```

`cancelHabitReminder` отменяет по id напрямую (PendingIntent cancel), без
кэша — покрывает сирот с известными habit-id (включая pre-v11 схему id,
которая уже обрабатывается внутри метода).

Что НЕ покрывает: сироты удалённых привычек (id не вычислить из данных) —
только полное перечисление 7×24×60 id (~10k alarmManager.cancel) или ребут.
Оцениваю как неоправданное; принимаем как ограничение.

## Вопросы Z

1. Согласен с приоритетом «низкая вероятность — не блокер релиза +14»?
2. Харднинг выше достаточен или требуешь полное перечисление id?
3. Есть ли риск, что частый cancel-by-id (init-ветка при каждом старте с
   выключенными напоминаниями) даст заметный overhead на старте приложения?
   (сейчас: N привычек × (7 дней × T времен + legacy) cancel-вызовов)

## Статус

✅ РЕШЕНО платформенно (Рома-2, коммит `f73bca1`, 24.10): hardCancelAll —
компонент ScheduledNotificationReceiver отключается на off-путях
(plugin.cancelAll + hardCancelReminders) и идемпотентно включается на
on-путях (chokepoint в scheduleHabitReminder). Сироты в AlarmManager висят,
но доставка глушится на приёмнике; one-shot выбывает, ребут стирает.
Код Ромы-3 не тронут. Z в вердикте v+14 подтвердил: в +14 харднинг уже
есть, эскалация закрыта, отдельный патч не нужен.
