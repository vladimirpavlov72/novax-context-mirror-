# NOVAX — Quick Context (Read This First!)

> 30-second read. Then read AI_RULES.md.
> Last updated: 2026-10-15 (свежие факты недели — блок «Синхронизация 15.10» ниже)
> **Правило архива**: выполненный план переезжает в `archive/` (полный план + отметка «выполнено, дата»). В `CONTEXT_SHORT.md` и ACTIVE_TASKS.md живут только активные планы.

---

## 🔄 Синхронизация 15.10 (что изменилось за неделю)

- **Маркетинг-конвейер работает**: TASK_001/002 (тексты Habits+, триал 30→7) приняты 9/10, ждали ревью К3 → теперь это К3-Макс (загрузчик `K3_start_Макс.txt`). TASK_003 (GP-витрина) выдана Роме-маркетингу 15.10.
- **Проблема маркетолога подтверждена**: большой трафик просмотров карточек → мало установок. Дожимаем витрину (баннер 1024×500, 8 скринов, short/full описания EN+RU), а не трафик.
- **voice.md создан** — канонический Voice Profile канала (голос, хуки, табу). Обязателен для всех текстов.
- **Apple/xtool изучен** → `stores/APPLE_IOS_BACKLOG.md`: iOS-трек в бэклог до 3+ критериев, грузинский контур его обслужит.
- **Решение 15.10 (Владимир)**: Samsung/Huawei НЕ ждать GP. Cleaner первым, APK под альт-сторы в работе у Ромы-2 (бриф `stores/TASK_CLEANER_SAMSUNG_HUAWEI.md`). Монетизация там: План А — v1 бесплатная полная версия, IAP отдельным этапом (обсуждается с К3-Макс).
- **Cleaner reminders**: аудит показал — в приложении нет системы напоминаний вообще. Бриф выдан Роме-2 (`cleaner/TASK_CLEANER_CLEAN_REMINDERS.md`): 1 раз в 30 дней + порог 500 МБ, opt-in после первой чистки.
- **Зеркало**: создан публичный санитарный репо `novax-context-mirror-` (без секретов) — К3-Макс читает контекст по ссылкам сам.
- **RuStore модерация**: свежих данных нет с 20.09 (H₂O пересдан после отклонения скринов). Продажи: 0 за 2 месяца — факт не изменился.
- **Huawei**: регистрация возможна сейчас (РФ-контур, DUNS не нужен), но сначала вопрос в support про выплаты на российские банки (`stores/TASK_HUAWEI_REGISTRATION.md`).

## Active Now (09.10)

- **Грузинский контур СОЗДАН**: ИП, 1% (SBS), юрадрес, Credo (2 счёта), SIM Magti,
  прокси Bright Data. **DUNS подан 01.10 — ждём до ~01.11** → дозаполнение Play
  Console → $25 → верификация. Декларация rs.ge за сентябрь подана 09.10 ✅.
  Уведомление ФНС — до ~30.10 (`georgia/TASK2_…`).
- **GP flavors track ЗАВЕРШЁН (08–09.10):** Habits/H₂O/QR на едином коде, ветки
  `feature/play-flavors` в основных репо, SDK 36, AAB собраны, все Z-циклы и
  QA пройдены. gplay-копии (`C:\devpps\gplay\`) — резерв, НЕ ТРОГАТЬ.
- **Решения Владимира+Z (09.10, обязательны — DECISIONS.md):**
  ① пейволы: честный манифест против подписки с ценами ПО ФЛЕЙВОРУ
  (плейсхолдеры {yearlyPrice}/{oneTimePrice} через BillingRepository:
  play «$29.99/$2.99», rustore «1 990 ₽/159 ₽»); ② **NOVAX Files** — новое
  приложение для GP (com.novax.files, SAF+MES, очистка v2, flavors сразу);
  ③ Cleaner — только альт-сторы (MES, без SAF, без GP, toolchain не трогать);
  ④ GP-запуск: internal→closed (3-5 тестеров, 3-4 дня)→production,
  порядок QR → H₂O → Habits.
- **В работе (Рома-2):** уведомления Habits (чек-лист Z — блокер GP-релиза),
  refund-отзыв premium, план NOVAX Files.
- **RuStore:** на модерации Habits +13, H₂O 1.0.1+2027, Cleaner +2026, QR 2.1.0+2027.
- **Владимир занят Стальстройпроект до ~15.01**; автономность Ром до красных линий.


## Быстрые ссылки

- Грузия: `georgia/` (MASTER_PLAN, TBILISI_OPS_PLAN, INFRA, KYC_STORY, TASK1, TASK2, REPORT)
- GP-план: `GOOGLE_PLAY_PLAN.md`, `gplay/CONTEXT_GPLAY.md`, `stores/SAMSUNG_HUAWEI_PLAN.md`
- Сайт: VPS `[СКРЫТО:IP]`, 13 интент-лендингов, ASO готовы (см. SITES.md)

- **Текущий стратегический фокус**: (1) параллельный GP-проект для Habits/H₂O/QR — план `GOOGLE_PLAY_PLAN.md` (отдельные репо в `C:\dev\apps\gplay\`, ИП Грузии = org-аккаунт, $2.99, ru-язык оставляем, порядок QR→H₂O→Habits, Cleaner — позже); (2) подготовка к Тбилиси (вылет середина/конец сентября); (3) 4 APK залиты в RuStore на модерацию 14.09.
- **RuStore**: на модерации Habits +13, H₂O 1.0.1+2027, Cleaner +2026, QR 2.1.0+2027. Опубликованные: Habits +10, H₂O 1.0.0 (код 2012), Cleaner +22, QR +10.
- **GP-этап 1 (QR) ✅ 14.09**: `C:\dev\apps\gplay\novax-qr-scanner` собран на Flutter 3.47.4 (`C:\dev\flutter_gp`, отдельно от RuStore 3.24) — Gradle 8.14.3/AGP 8.12/Kotlin 2.2.20, SDK 36+NDK r28, биллинг on in_app_purchase ($2.99), RuStore вычищен (код+тексты 12 локалей), QA на A52 пройден, коммит `df9a6a3`. Дальше по шаблону: H₂O → Habits.
- **Сайт**: сайт `novaxhub.ru` мигрирован на собственный VPS `[СКРЫТО:IP]` (nginx + SSL). Создано и задеплоено 13 интент-лендингов (9 P0 + 4 P1): `/telefon-tormozit/`, `/malo-pamyati/`, `/zabyvayu-pit-vodu/`, `/kak-vyrabotat-privychku/`, `/qr-ne-skaniruetsya/`, `/pamyat-telefona-zapolnena/`, `/tracker-vody/`, `/prilozhenie-dlya-privychek/`, `/skaner-qr-koda/`, `/ocistit-telefon-ot-musora/`, `/napominanie-pit-vodu/`, `/kak-ne-brosit-privychku/`, `/sozdat-qr-kod/`. Обновлены `llms.txt` (22 URL в `sitemap.xml`), `/llms/apps.json`, `/llms/intents.json`. Подготовлены ASO-тексты для Cleaner, QR, Habits+, H₂O. См. `SITES.md`, `app/INTENT_RESEARCH.md`, `app/INTENT_LANDING_PLAN.md`, `app/GEO_TEST_RESULTS.md`.

## App Status

**NOVAX Habits v1.0.0+13**
- Status: **на модерации в RuStore** (залито 14.09) — фикс триал-баннера (не показывался во время trial), trial 30→7 дней (с +12).
- Package: `com.novax.habits`.
- RuStore App ID: `2063741856`. Product ID: `novax_habits_full_access`.
- Release APK 21.9 MB arm64: `C:\dev\releases\novax_habits_v1.0.0+13_arm64.apk`; пакет для консоли в Downloads (`NOVAX_Habits_v1.0.0+13`, APK совпадает с releases по md5).
- Опубликованная версия: v1.0.0+10. Next: Владимир заливает +13; после — sandbox-платежи. Подробности — в `habits/CONTEXT_SHORT.md`.

**NOVAX H2O v1.0.1+2027**
- Status: **на модерации в RuStore** (залито 14.09) — тот же билд, что +18 (экран награды дизайнера), но versionCode 2027: предыдущая версия в RuStore имела код 2012, +18 с кодом 18 отклонён. Годовые коды от 2027+.
- Package: `com.novax.h2o`. RuStore App ID: `2063737766`. Product ID: `novax_h2o_full_access`.
- Release APK 37.8 MB arm64: `C:\dev\releases\novax_h2o_v1.0.1+2027_arm64.apk`; пакет для консоли в Downloads (`NOVAX_H2O_v1.0.1+2027`, актуальные скрины 08_reward с 14T).
- Price: 159₽ one-time. Trial: 7 days. NO subscriptions.
- Next: после модерации — создание товара `novax_h2o_full_access`, sandbox-тест. Подробности — в `h2o/CONTEXT_SHORT.md`.

**NOVAX Cleaner v2.1.2+2026**
- Status: **на модерации в RuStore** (залито 14.09) — фикс false-positive full-access gate (купленный доступ мог не распознаваться), медиа-разрешения без доступа к аудио, устранена петля диалога разрешений.
- Release APK 19.0 MB arm64: `C:\dev\releases\novax_cleaner_v2.1.2+2026_arm64.apk`; пакет для консоли в Downloads (`NOVAX_Cleaner_v2.1.2+2026`, APK совпадает с releases по md5).
- Опубликованная версия: v2.0.2+22. Next: мониторинг, ASO-обновления.

**NOVAX QR Scanner v2.1.0+2027**
- Status: **на модерации в RuStore** (залито 14.09) — добавлена светлая тема (выбор системная/светлая/тёмная в настройках, ThemeService + SharedPreferences), контраст-фиксы текста, прозрачная иконка. QA светлой и тёмной на 14T пройден. Тёмная тема не изменилась.
- Release APK 39.9 MB arm64: `C:\dev\releases\novax_qr_v2.1.0+2027_arm64.apk`; пакет для консоли в Downloads (`NOVAX_QR_v2.1.0+2027`, светлые скрины 9:16 и полные с 14T). Старая папка `+10` → `_archive_`.
- Опубликованная версия: v2.1.0+10. Next: после модерации — мониторинг, ASO-обновления.

## Backlog / Планируемые приложения

**Портфель NOVAX: 10 мобильных приложений.** Опубликованы 3 (QR, Cleaner, Habits+). H2O на модерации в RuStore. В бэклоге 6.

| # | Приложение | Русское название | Статус |
|---|------------|------------------|--------|
| 5 | **NOVAX Habits** | Привычки + | ✅ v1.0.0+10 опубликовано; +13 готов к заливке (фикс триал-баннера). |
| 6 | NOVAX Password Manager | Менеджер паролей | ⚪ Бэклог |
| 7 | NOVAX Calculator Pro | Калькулятор Pro | ⚪ Бэклог |
| 8 | NOVAX Notes / Memos | Заметки | ⚪ Бэклог |
| 9 | NOVAX Timer / Focus | Таймер / Фокус | ⚪ Бэклог |
| 10 | **NOVAX Files** | Файловый менеджер | 🔥 **GP-first, в разработку после решений 09.10** (com.novax.files, SAF+MES, flavors сразу; Cleaner для GP не идёт) |
| 11 | NOVAX Battery Saver | Экономия батареи | ⚪ Бэклог |

## Tech Reminders (CRITICAL)

1. NO `generate: true` in pubspec.yaml
2. `nullable-getter: true` in l10n.yaml
3. Remove `const` from rebuildable widgets
4. `localeResolutionCallback` REQUIRED
5. Single MaterialApp only
6. pub.dev BLOCKED → offline packages
7. EdgeInsetsDirectional for RTL

## Next Files to Read

1. `AI_RULES.md` — coding rules + 12 anti-patterns
2. `h2o/CONTEXT_SHORT.md` — детали текущего релиза H2O
3. `PROJECT.md` — philosophy, Kaneman, finance
4. `SITES.md` — сайты NOVAX (для задач по novaxhub.ru и лендингам)
