# Cleaner — Quick Context

## Status: v2.2.0+2027 — clean reminders (2026-10-15, Рома-2)

Фича напоминаний по TASK_CLEANER_CLEAN_REMINDERS.md: workmanager (раз/сутки,
inexact, без exact alarms) + flutter_local_notifications (канал reminders,
стандартный звук); порог >500 МБ (лёгкая оценка: кэши + temp) И ≥30 дней с
чистки; ≤1 уведомление/30 дней; opt-in диалог после ПЕРВОЙ чистки (permission
существующим requestNotificationPermission, без повторных дёрганий); тоггл
в настройках (по умолчанию вкл, откл отменяет задачу). Уведомление — из ARB
по локали устройства (delegate.load в фоне без контекста).
SharedPreferences: reminders_enabled, last_clean_timestamp,
last_reminder_timestamp (добавлен — без него «игнор» не остановить спам),
reminder_prompt_shown. Z-ревью пройдено (3 бага фонового изолята — все исправлены), **QA end-to-end на A52 пройден полностью** (чеклист §7: opt-in после первой чистки, системный запрос, гейт мусора отрицательно И положительно, уведомление RU-локализованное, tap → приложение, штамп last_reminder, тоггл отменяет/возобновляет воркер). QA-found баг №4: Platform.localeName="ru_RU" ломал ARB-загрузку в фоне → парсинг + en-фолбэк. Финальные коммиты: `e5db531` (фича) + `1c3f76a` (Z/QA-фиксы). APK arm64 19.4 МБ:
C:\dev\releases\novax_cleaner_v2.2.0+2027_arm64.apk.
**APK НЕ заливать в RuStore до Z-ревью и решения Владимира (бриф §8).**
QA end-to-end (тест-форс 15 мин + backdated timestamp) — ожидает A52.
Toolchain не тронут (3.24/SDK34). Отступления от брифа: ①
last_reminder_timestamp (обосновано); ② фон-оценка мусора — кэши+temp, не
полный скан (батарея, занижает); ③ 10 не-en/ru локалей — англ. плейсхолдеры
(конвенция проекта).
## Repo: github.com/vladimirpavlov72/novax-cleaner (private)
## Package: com.novax.cleaner
## App ID RuStore: 2063733453
## Product ID: novax_cleaner_full_access
## Price: 159₽ / $2.99 one-time. Trial: 7 days. NO subscriptions.

## ⚡ Стратегия (решения Владимира+Z, 09.10 — DECISIONS.md)

- **Cleaner НЕ идёт в Google Play** (policy: MES не выдаётся cleaner-категории).
  Живёт на RuStore / Huawei AppGallery / Samsung Galaxy Store.
- **SAF-адаптация НЕ нужна** (Z: SAF-клинер бесполезен — нет доступа к
  Android/data чужих приложений). Вместо SAF — **MANAGE_EXTERNAL_STORAGE**
  (альт-сторы выдают лояльно).
- **Toolchain не мигрировать** (3.24/SDK 34), пока сторы не потребуют
  (Huawei требует 33+ — ок). Время — в NOVAX Files (новое GP-приложение).
- Для GP вместо Cleaner'а делается **NOVAX Files** (файловый менеджер,
  com.novax.files, отдельная карточка).

## Latest
- v2.1.2[СКРЫТО:ТЕЛ], коммит `761368b`): fix false-positive full-access gate (купленный полный доступ мог не распознаваться); медиа-разрешения больше не требуют доступа к аудио.
- v2.1.2+2025 (коммит `06ed7c0`): remove trial, paywall before cleaning, Z-review fixes.
- Z-ревью закрыто, все блокеры устранены.
- Ребрендинг label на `Cleaner` запушен (`f898c00`).
- Billing интегрирован через `flutter_rustore_pay` 10.3.1.

## Latest Release APK (arm64-v8a)
- **Project path:** `C:\dev\apps\novax-cleaner\build\app\outputs\apk\release\app-arm64-v8a-release.apk` (~19.0 MB)
- **Releases copy:** `C:\dev\releases\novax_cleaner_v2.1.2+2026_arm64.apk`
- **Downloads copy:** `C:\Users\Владимир\Downloads\NOVAX_Cleaner_v2.1.2+2026\novax_cleaner_v2.1.2+2026_arm64.apk`
- **Note:** v2.0.2+22 — последняя опубликованная в RuStore версия.

## Next
1. **Cleaner reminders: ГОТОВО К РЕЛИЗУ (15.10 вечер, Рома-2)** — QA A52 PASSED,
   Z-ревью PASSED (3 бага фонового изолята исправлены: initialize() уведомлений,
   локаль ru_RU, BackgroundIsolateBinaryMessenger). Коммиты `e5db531`+`1c3f76a`,
   APK v2.2.0+2027 финальный в `C:\dev\releases\`.
   ⚠️ **Секвенсинг (К3-Макс): v2.1.2+2026 и v2.2.0+2027 НЕ публиковать** —
   без trial карточка врёт ещё сильнее. Проверить консоль, не опубликована ли
   +2026. Ждём v2.3.0+2030 + карточку, заливаем вместе одной модерацией.
   Заливка RuStore — руками Владимира. Разбор: `stores/K3_ТЕМА7_cleaner_trial.md`.
   Бриф Роме-2: `TASK_CLEANER_FREE_LIMIT.md` (v2.3.0+2030, после Habits-уведомлений,
   + QA-кейс «обновление с активным trial»). Тексты: TASK_005 у маркетинга
   (🔥 карточка врёт «7 дней бесплатно» — чинить первой очередью + новые
   in-app тексты).
   Известные не-блокеры: ребут-ресilienс не гонялся физически (WorkManager
   persisted по дизайну); 10 локалей — EN-плейсхолдеры (RU/EN полные).
2. Мониторинг, отзывы, ASO-обновления.
3. Sandbox-платежи и метрики после стабилизации загрузок.
