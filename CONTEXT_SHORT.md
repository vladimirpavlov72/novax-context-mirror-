# NOVAX — Quick Context (Read This First!)

> 30-second read. Then read AI_RULES.md.
> Last updated: 2026-10-24 (блок «Синхронизация 24.10» ниже)
> **Правило архива**: выполненный план переезжает в `archive/` (полный план + отметка «выполнено, дата»). В `CONTEXT_SHORT.md` живут только активные планы.

---

## 🔄 Синхронизация 26–27.10 (ночь: два релиз-блокера убиты до заливки)

- **🔴 HOTFIX Привычки +14 — два фикса в одном релизе**: ① keep.xml
  (invalid_icon, A52-ловушка); ② **R8 ломал Gson TypeToken** → вечный
  сплэш на Android 16 (процесс жив, FATAL нет — A52 не ловил; найден на
  QA 14T по требованию Ги). Пакет: RELEASE_Habits_v1.0.0+14 (md5
  `8e8242d6`), Z-дополнение готово. Ждём Z-вердикт → заливка.
- **ШАГ 1 гейта К3 закрыт** (Гиа 26.10 22:45, брак 4/10): 5 UI-фиксов
  Cleaner (9793c07) — единицы ×12 полностью, шапка FittedBox (ловушка
  font_scale=1.3!), честный баннер пейвола, Премиум без обрезок, «30
  дней». 4031 пересобран (md5 `539705de`), QA A52+14T. ШАГ 2: Р-3 снял
  ВСЕ приложения на 14T (Cleaner/H₂O/QR/Habits raw в зеркале).
- **Ревью SAF-скана Р-4 (c32e387) — зелёный**; дым вендор-патча H₂O/QR —
  стартеры живы. Подписи команды: Р-2/Р-3/Р-4 (файлы прежние).
- **⚠️ Риск линейки (KNOWN_ISSUES 4.6)**: R8/Gson-правила нужны в
  proguard Cleaner (reminder_service) и H₂O — по окну.
- **UX-бэклог на решение Ги**: silence-first молча гейтит scheduling;
  custom-слот нельзя сбросить в UI.

## 🔄 Синхронизация 24.10 (итоги 21–24.10)

- **Очередь дня Гиа закрыта полностью (24.10)**: ① P1 уведомления Привычек —
  полный цикл (fb4a126 → Z-финал da8f187 → харднинг сирот f73bca1), 14/14
  тестов, ЗЕЛЁНЫЙ на +14; ② D1 (RuStore Review/Update SDK) — 4/4, Habits
  портирован (0cac063); ③ D3 бренд-полка — эталон Cleaner 33d5fd7 + порты
  QR/H₂O/Habits, QA на A52 зелёная (rustore — жив, play — скрыт, INTERNET
  не добавлен); ④ D4-рекорды — 7 клипов 9:16 в `design/d4_records/`.
- 🔴 **Три P0 раскопаны 24.10** (детали в KNOWN_ISSUES, уроки там же):
  1. *l10n-стагнация*: gen-l10n пропускает реген по mtime, а в Cleaner
     generated gitignored → все сборки лета с устаревшими строками (баг
     «Можно освободить N» без «МБ»; ARB правильный с июля!). v2.3.0+4030
     на модерации RuStore содержит баг — ждём решения Владимира о
     пересборке+перезаливе. Правило: `flutter gen-l10n` перед КАЖДОЙ
     релизной сборкой, l10n-правки верифицировать на девайсе.
  2. *keep.xml*: shrinkResources вырезал `@drawable/ic_notification`
     (референс только из Dart-строки плагина) → release Habits/H₂O умирали
     на сплэше. Фикс: Habits e128fbc, H₂O 6c8dd0c. Правило: QA релизов
     только release-сборками; drawables из Dart-строк — в keep.xml.
  3. *Вендор-патч RuStore-обёрток*: жадный onAttachedToEngine + gradle-
     exclude ru.rustore.sdk в play-флейворе → NoClassDefFoundError обрывал
     ВСЮ регистрацию плагинов (сплэш-краш GP). Патч lazy-init с зондом
     Class.forName (C:\dev\vendor\packages\flutter_rustore_{review,update}-10.5.2).
     Правило: новые RuStore-обёртки — только ленивая инициализация.
- **NOVAX Files (Рома-4)**: SAF/MES-каркас c9eb142 одобрен; план сканера
  (этап 3: internal-only, compute(), агрегаты в prefs) визирован 24.10 — кодит.
  Открыто: иконки лаунчера, ARB-переводы ×10, GitHub-remote.
- **D4 + материалы К3**: рекорды сданы (`design/d4_records/` + REPORT);
  фотопакет для кадров в работе (правила К3: RU/светлая/release, без
  триала/цен; Cleaner — только alt v2.3.0+2028). ⚠ «счётчик серии» в
  Привычках НЕТ в билде — маркетингу не обещать «стрик N дней».
- **Фактчек-маркетинга 24.10**: H₂O «виджета НЕТ» (home_widget удалён
  09.10, short править); кофе/чай, норма вес×32, тёмная тема — честно ✅.
- **🔧 Cleaner v2.3.1+2031 собран и прогнан на A52 (вечер 24.10,
  TASK_CTA_PRICE_REMOVAL, решение Владимира)**: цена убрана с CTA
  («Очистить все • 159 ₽» → «Очистить все (32.7 GB)»; clean_success
  «Снять лимит»), честный диалог-объяснение перед пейволом (7 ключей ×12
  локалей), грид пейвола по «что увидит юзер»: выкинуты НЕреализованные
  автоочистка и виджет. Коммиты c278dd4+b2f1fe8. Релиз-пакет:
  `C:\dev\releases\RELEASE_Cleaner_v2.3.1+2031_2026-10-24\`. ⚠ Истинный
  корень «без МБ» — визуальный клип рендера (maxLines:1+clip), не l10n!
  ⚠ Маркетингу: автоочистку/виджет Cleaner нигде не обещать.
  Z-пакет: reviews/z_review_cleaner_v2.3.1/ (вердикт ждём). Перезалив —
  решение Владимира.
- **🔴 ИНЦИДЕНТ вечера (эскалация Гие 16:35)**: опубликованные 13.10 в
  RuStore ПРИВЫЧКИ+13 СЛОМАНЫ (тот же keep.xml-баг: холодный старт падает
  с invalid_icon). Подтверждено сверкой md5 заливки==releases\. Фикс в
  master (e128fbc+da8f187), предложен срочный hotfix +14 — ЖДЁМ РЕШЕНИЯ.
  Детали: KNOWN_ISSUES 4.5.
- **keepfix-пересборки для съёмки**: H₂O rustore +2028 и Habits rustore
  +13 пересобраны с master, верифицированы холодным стартом на A52
  (releases\*_keepfix.apk) — блокеры съёмки Ромы-3 сняты. Рома-3 сдал
  кадры RU+EN (Cleaner alt + QR) в design/<app>/raw/.
- **Блокер «Clean Free Items Only крашит Cleaner» — ЗАКРЫТ (вечер)**:
  краша нет, A52 и 14T Pro × полный free-flow зелёные; причина наблюдения
  — координатные промахи adb-тапов (урок: tap по bounds из uiautomator
  dump). PDF Сбера на 14T при репро не пострадал.
- **TG-мост лежал с ~16:30** (api.telegram.org таймаут) — MAILBOX был
  основным каналом; проверить утром 25.10.

## 🔄 Синхронизация 20.10 (архивно, длительные правила)

- **Правило единой папки релиза**: `C:\dev\releases\RELEASE_<App>_<version>_<ГГГГ-ММ-ДД>\`.
- **Правила текстов Владимира**: без «честно», без «мы просим», без цифр лимита в
  карточках, деньги НЕ на первом экране (пейвол в точке лимита), именные подписи.
- **TG-мост запущен**: @novax_gia_bot. Слушает (poll) ТОЛЬКО Гиа; отправка (send)
  всем, сообщение начинается с имени. Скрипт tools/tg_bridge.py.
- **Cleaner v2.3.0** (релиз-контур закрыт 20.10, залит Владимиром): модель v2 —
  первая чистка бесплатно без лимита, дальше 300МБ/30дн, полная — 159 ₽ разово.
  Релизная папка `C:\dev\releases\RELEASE_Cleaner_v2.3.0+2030_2026-10-19\`
  (CARD_RU.md — фактчек пройден). ⚠ 24.10: в залитом +4030 вскрыт l10n-баг «без МБ»
  (см. блок 24.10 и KNOWN_ISSUES) — ждёт решения о пересборке.
- **Huawei = грузинский контур, «Предприятие» после DUNS** (~01.11). Геоблок РФ
  подтверждён. Huawei ID создан (регион Грузия, SIM Magti). Тикет Support Desk —
  текст готов в stores/TASK_HUAWEI_REGISTRATION.md.
- **GP-веб-контур**: novaxhub.app (свой, Cloudflare-приватный) + GH-орг novax-apps
  (завести Владимиру). Пакет к 15.11: privacy EN ×3 + hub + support@.
- **Кросс-промо**: бренд-полка РЕАЛИЗОВАНА 24.10 (блок 24.10). Продвижение:
  stores/STORES_REGIONAL_PROMO.md.
- К3-Макс: схема связки 7/10→улучшена (автосинк зеркала, дайджест-ACK, защита от
  injection); кадры витрин собирает в браузере (пайплайн рабочий, 24.10).
- Ворк (Kimi Work): аудит RuStore принят (органика ~1–2 продажи, воронка 0.36% —
  узкое место); метрики по средам (METRICS_WEEKLY.md), ads-декларации — задача.

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
- GP-план: `gplay/CONTEXT_SHORT.md` (резерв, НЕ ТРОГАТЬ), `stores/SAMSUNG_HUAWEI_PLAN.md`
- Сайт: VPS `[СКРЫТО:IP]`, 13 интент-лендингов, ASO готовы (см. SITES.md)

- **Текущий стратегический фокус** (состояние на середину сентября, история): (1) GP-трек на flavors-подходе (решение 09.10: единый код, gplay-копии в `C:\dev\apps\gplay\` = РЕЗЕРВ, НЕ ТРОГАТЬ; ИП Грузии = org-аккаунт, $2.99, порядок QR→H₂O→Habits, Cleaner — позже); (2) подготовка к Тбилиси (поездка ✅ 29.09–02.10, отчёт `georgia/REPORT_TBILISI`); (3) 4 APK залиты в RuStore на модерацию 14.09.
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

- **🔴 Правило versionCode Cleaner (Владимир, 25.10)**: линейка RuStore =
  **403X** (base 203X + 2000 split-abi сдвиг; исторически +4030 опубликован).
  ВСЕГДА: новый code > опубликованного; **имя APK = коду ВНУТРИ** (никаких
  ручных переименований!); перед сдачей — `aapt dump badging | grep
  versionCode`. Провал 25.10: 2.3.1 сдали как «+4031» именем, а внутри
  был 2031 — RuStore отклонил.

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
