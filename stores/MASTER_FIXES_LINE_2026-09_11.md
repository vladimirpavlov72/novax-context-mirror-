# МАСТЕР-СПИСОК БАГ-ФИКСОВ ЛИНЕЙКИ (01.09–03.11.2026) → порт на международку

> Задача Гиа 03.11 12:30 (ШАГ 1). Источники: git log master/main + feature/play-flavors
> пяти репо + KNOWN_ISSUES.md. Составил Р-2 (46fba1d2) 03.11.
> Легенда: «в play» = входит в feature/play-flavors (база интл-пакетов);
> «n/a» = play-контура нет / неприменимо. Класс: К = критичный, М = косметический.

---

## 1. CLEANER (master; play-контура НЕТ — решение 09.10 «только альт-сторы»)

| Дата/коммит | Суть фикса | Контур | В play? | Класс |
|---|---|---|---|---|
| 11.09 06ed7c0 | Убран триал; пейвол до чистки; Z-review fixes (модель v2 премиума) | rustore | n/a | К |
| 11.09 761368b | False-positive гейт полного доступа; media-разрешения больше не требуют audio | rustore | n/a | К |
| 15.10 e5db531/1c3f76a | Напоминания о чистке (30дн+500МБ гейт, opt-in); фикс background isolate (init плагина, locale parse, BackgroundIsolateBinaryMessenger, fallback при OEM task-kill) | rustore | n/a | К |
| 17.10 0a1bdd6 | Z-review v2.3.0: stop-by-limit при чистке (continue + сортировка по возрастанию размера); daysRemaining +1; exhausted-копия по фактическим байтам + ICU ×12 | rustore | n/a | К |
| 19.10 e696b67/a1ce3a1/cba629e | MODEL v2: первая чистка без лимита (gift-флаг); free-limit FirstInfo честными словами; Z STOP-cycle B1–B10 (firstCleanAvailable таймстемпом, recordClean bytes-before, exhausted-гейт на входе, mounted-чеки, 7 юнит-тестов FreeLimitService) | rustore | n/a | К |
| 22.10 a46b831 | altstore-маркер first_launch_app_version (основа grandfathering) | altstore | n/a | М |
| 23.10 187450e | StoreSdkService RuStore Review+Update (эталон D1) | rustore | n/a | М |
| 24.10 33d5fd7/8834b2d | Бренд-полка «Другие приложения NOVAX» (INTERNET-аудит); QA-фикс layout карточки полки (кнопка сжимала текст до 150px, «Cleane/r») | оба (полка) | n/a | М |
| 24.10 c278dd4 | **CTA без цены**: «Очистить все • 159 ₽» → «Очистить все (размер)»; честный premium-explainer перед покупкой; грид пейвола по правилу «что увидит юзер» (сняты нереализованные автоочистка/виджет) | rustore | n/a | К (честность) |
| 24.10 b2f1fe8 | Визуальный клип « МБ» в шапке результатов при ~5-значных числах (maxLines+Flexible+clip) | rustore | n/a | М |
| 24.10 ab215a1 | «Used» на главной — хардкод EN на всех локалях → l10n.used | rustore | n/a | М |
| 26.10 9793c07 | UI-фиксы гейта К3: единицы полностью в l10n (unitBytes/KB/MB/GB ×12, formatBytesL, даты yearsAgo, duplicatesGroupLine ×12, storageUsed/free через unitGB); шапка FittedBox; честный баннер пейвола; hero/grid без обрезок; «следующая чистка» 30 дней ×12 | rustore | n/a | М |
| 26.10 724dbe4 | versionCode 2031→4031 (RuStore требует > 4030); правило имени APK = коду | rustore | n/a | М |
| 27.10 101f502 | l10n: порядок аргументов duplicatesGroupLine (сигнатура по алфавиту — KNOWN_ISSUES 0.5); ICU-плюрализация файлов ×12 (рукава {count}, не #) | rustore | n/a | К (ловушка класса) |
| 27.10 3232e25 | Z-review P1: zh nextCleanRecommended 7→30; yearsAgo/monthsAgo/daysAgo/hoursAgo/justNow ×10 локалей (untranslated→EN-фолбэк) | rustore | n/a | М |
| 27.10 8e0eed0 | **proguard R8/Gson-гард** (эталон Habits +14): вечный сплэш Android 16, KNOWN_ISSUES 4.6 | rustore | n/a | **К** |
| 27.10 c9660b0 | AR duplicatesGroupLine: рукав two + zero (Z-замечание дельта-2) | rustore | n/a | М |
| 27.10 0f0954b | K3-микрофиксы: willDeleteForever → ICU-plural ×12 (вызов String→int); ярлык quick-stats FittedBox(scaleDown) при любом font_scale | rustore | n/a | М |
| 02.11 26ee641 | **Галка-фантом «Глубокая очистка системы» снята** (дело Ильи); мёртвые гейты autoClean/homeWidget вычищены из enum/canAccess | rustore | n/a | **К** (честность) |
| 03.11 6cac3a3 | CTA-кнопка и подпись не рендерятся при активном премиуме; **RU-цена без копеек** (getPrice стрипает .00 только для рублёвых меток; $-зона не трогается) | rustore | n/a | М |

**Cleaner — сводка:** play-контура нет, порт на международку не требуется.
Все критичные фиксы (proguard 8e0eed0, v2.3.2) вошли в сборку 4032 (03.11).
⚠ Бэклог: EN-числа без разделителя тысяч («33470 MB Found» — altstore-дым 01.11);
версия в «О приложении» захардкожена v2.1.2 (баг доверия); PurchaseEventListener
не подключён + нет retry-sync (слабые места активации, связь с делом Ильи не доказана).

---

## 2. H₂O (main = rustore-линия; feature/play-flavors = интл + rustore APK с 09.10)

| Дата/коммит | Суть фикса | Контур | В play? | Класс |
|---|---|---|---|---|
| 11.09 5f1ef79 | Notification receivers + ic_notification (краш уведомлений), v1.0.0+13 | оба | ✅ да | К |
| 11.09 939e3cb | Z-review N2H: tools:targetApi, bodyBuilder hoist | оба | ✅ да | М |
| 11.09 94676c3 | Дизайнер-иллюстрации онбординга, Cupertino time picker, reward star overlay (+14) | оба | ✅ да | М |
| 09.10 7313321/e0a6659 | **flavors rustore/play + BillingRepository** (SDK36 toolchain, vendored SDK, maven-repo) | оба | ✅ (родилась) | К (архитектура) |
| 09.10 5c3e5b1 | Z-review: init-флаг после проверок, StateError-ключи, drop retryBilling/dead homeWidget, GP restore productId-guard + ack-once + 2мин таймаут | оба | ✅ да | К |
| 09.10 a5d1f2b | billing QA-found: split sdkInitialized/available; ленивый GP-listener; GP product-not-found → l10n; vendored in_app_purchase_android: defer BillingClient connect | оба | ✅ да | К |
| 09.10 80c026b | stale purchase completer discarded on re-attach; dispose nulls subscription; exclude contains(play) | оба | ✅ да | К |
| 09.10 b01900a/26e7ba6 | Честный якорь против подписок (Z variant 1, решение Владимира 09.10) | оба | ✅ да | М |
| 17.10 24c92f2 | RuStore revoked-статусы aligned (literal compare; старый SDK const class); GP parallel-restore surfaces revoked; refund-revoke one-shot snackbar | оба | ✅ да | К |
| 23.10 8d069c4 | StoreSdkService RuStore Review+Update (порт эталона Cleaner) + nullable-l10n fix в revoked-снэкбаре | rustore | ✅ да (SDK только rustore-флейвор) | М |
| 24.10 3410cee/acdc2cb | Бренд-полка (порт эталона); QA-фикс layout карточки полки | оба | ✅ да | М |
| 24.10 6c8dd0c | **keep.xml — shrinkResources вырезал @drawable/ic_notification → краш релиза на сплэше (invalid_icon)** — тот же класс, что Habits 4.2 | оба | ✅ да | **К** |
| 27.10 f6d344b | proguard R8/Gson-гард (эталон Habits +14) | оба | ✅ в дереве; в пакетах 30.10/01.11 — да | **К** |
| 29.10 00cbb65 | l10n витрин: единицы мл/кг из кода → l10n ×12 (хардкод 'ml'/'kg' на RU-экранах) | оба | ✅ да | М |
| 29.10 40deabc | **Краш оверлея награды**: SingleTickerProviderStateMixin × два AnimationController → красный экран при первом достижении цели; TickerProviderStateMixin (multi) | оба | ✅ да | **К** |
| 29.10 ca1a83b | storeSdkInit-таймаут 3с (Future.any-кап; регресс-риск «QR старт 2 мин») | оба | ✅ да | К |
| 30.10 d06187a | **Баг-зеркало цен**: gen-l10n сортирует плейсхолдеры алфавитно (oneTimePrice, yearlyPrice), вызов был (yearly, oneTime) → цены зеркально; рефраз 'Мы просим' → 'У нас —' | оба | ✅ да | **К** (баг денег) |
| 30.10 5e730f8 | Якоря финал: RU 1 299 ₽; play $9.99→канон $49.99 (последующие коммиты) | оба | ✅ да | М |
| 31.10 6805c27/34b25d3/8f612e4 | К3-пейвол ARB ×12 + перенос верстки (якорь $49.99 без зачёркивания, paywallOnceForever, fallback=oneTimeAnchorPrice, 3 галки, paywallBuyPrice, футер) | оба | ✅ да (пакеты 01.11) | М |
| 31.10 1ce0041 | Кнопка «Напиток» (beverageButton ×12; иконка чашки неочевидна) | оба | ✅ да (пакеты 01.11) | М |

⚠ **РИСК-ЛИНИИ (важно):** rustore-линия H₂O (main) НЕ содержит ни одного
октябрьского фикса (keep.xml 6c8dd0c, revoked, мл/кг, тикер, storeSdkInit-кап,
proguard, зеркало цен). Опубликованный RuStore-релиз +2027 (09.10) — без них.
**Следующий RuStore-релиз H₂O собирать ТОЛЬКО из feature/play-flavors**
(или влить октябрь в main) — иначе повторение краша 4.2 в release.

---

## 3. QR (master; feature/play-flavors = интл + rustore APK с 09.10)

| Дата/коммит | Суть фикса | Контур | В play? | Класс |
|---|---|---|---|---|
| 14.09 2900cb8 | Светлая тема + выбор темы (v2.1.0+2027) | оба | ✅ да | М |
| 09.10 ef29433 | **flavors rustore/play + BillingRepository** (SDK36 toolchain; 0 классов ru.rustore.sdk в play / 1719 в rustore) | оба | ✅ (родилась) | К (архитектура) |
| 09.10 99d2a0e | Z-review: stale purchase completer (re-attach без таймаута); spinner-фолбэк цены (priceUnavailable ×12); Continue disabled при ошибке; dispose; exclude contains(play) | оба | ✅ да | К |
| 09.10 a84812f | Удалён мёртвый ключ paywallOldPrice ×12 (решение Владимира 09.10) | оба | ✅ да | М |
| 09.10 65a2207/87b92e7 | Честный якорь против подписок | оба | ✅ да | М |
| 17.10 939648c | GP parallel-restore surfaces revoked; refund-revoke one-shot snackbar + premiumRevokedNotice ×12 | оба | ✅ да | К |
| 17.10 438cc4d | Восстановлен generated l10n (synthetic gen pass удалил файлы — импорт-цель физические) | оба | ✅ да | К |
| 23.10 264d784 | StoreSdkService (порт эталона Cleaner; SDK только rustore-флейвор) | rustore | ✅ да | М |
| 24.10 8399f24/8b3f1de | Бренд-полка; QA-фикс layout карточки полки | оба | ✅ да | М |
| 29.10 1f9ea65 | l10n: тип QR в истории локализован ('ТЕКСТ' вместо raw 'TEXT'); **бонус storeSdkInit-таймаут 3с** | оба | ✅ да | К |
| 29.10 75d0420 | **История сканов без перезапуска**: IndexedStack держит вкладки живыми; HistoryScreenState.refresh() + GlobalKey | оба | ✅ да | К |
| 30.10 1510bcd | **Баг-зеркало цен** (класс KNOWN_ISSUES 0.5; рефраз 'Мы просим' → 'У нас —') | оба | ✅ да | **К** (баг денег) |
| 31.10 6f7e616/0ec8590/fef6622 | К3-пейвол ARB ×12 + верстка; RU paywallTitle «Разблокируй NOVAX QR» ('ты') | оба | ✅ да (пакеты 01.11) | М |

⚠ Замечание: master QR (2.1.0+2027) не содержит октябрьских фиксов, но
rustore APK с 09.10 (+2028) собран из flavors-ветки, которая всё содержит —
расхождение master↔play в QR умеренное, вливать октябрь в master не обязательно
(ветка flavors = рабочая линия).

---

## 4. ПРИВЫЧКИ+ (master; feature/play-flavors = рабочая линия rustore-релизов +14…+16 и интл)

| Дата/коммит | Суть фикса | Контур | В play? | Класс |
|---|---|---|---|---|
| 11.09 9185994 | Триал 30→7 дней, trial-баннер, expiry-снэкбар, Z-review fixes (+12) | оба | ✅ да | К |
| 11.09 c67898b | Trial-баннер не показывался (isPurchased vs effective premium) (+13) | оба | ✅ да | К |
| 14.09 a9856dc | Success-снэкбар по isPurchased, не isPremium | оба | ✅ да | К |
| 08.10 c5c7720/e0a6659/78dc1d1 | **flavors rustore/play + BillingRepository** (SDK36 toolchain) | оба | ✅ (родилась) | К (архитектура) |
| 08.10 059f26d | play label «Habits+» (решение Владимира 08.10) | play | ✅ да | М |
| 08.10 55696c0 | **proguard keep RuStore pay SDK** — R8 8.12 strips JNI-dispatched plugin chain (найдено через H₂O) | rustore | ✅ да | **К** |
| 09.10 272216b/4e58895/033ecb7 | GP repo restore guard + ack-once + 2мин таймаут; ленивый GP-listener + product-not-found l10n; stale completer | оба | ✅ да | К |
| 09.10 6d9c416/1048aea/620f88b | Якорь против подписок: old-price удалён, честный якорь, манифест {yearlyPrice}/{oneTimePrice} | оба | ✅ да | М |
| 17.10 a2a6a78 | Z-review: helper gated to Android; Settings-константы; GP revoked surfaces; refund-revoke snackbar; ASCII-кавычки en | оба | ✅ да | К |
| 17.10 531ff3d | Восстановлен generated l10n | оба | ✅ да | К |
| 23.10 fb4a126 | **P1: напоминания больше не воскресают после выключения пользователем** | оба | ✅ да | **К** |
| 24.10 f73bca1 | **Харднинг сиротских алармов**: platform hardCancelAll (DISABLED-компонент глотает доставку); chokepoint ensureRemindersReceiverEnabled | оба | ✅ да | **К** |
| 24.10 0cac063 | D1+D3 порты: StoreSdkService (diff=0 с эталоном) + бренд-полка (rustore-гейт) | оба | ✅ да | М |
| 24.10 ded134b | QA-фикс layout карточки полки (эталон Cleaner/QR/H₂O) | оба | ✅ да | М |
| 24.10 da8f187 | Z-финал +14: P1 exact-gate twin, P2 delete/loading, v14-миграция | оба | ✅ да | К |
| 24.10 e128fbc | **keep.xml — invalid_icon, краш релиза на сплэше** (4.2) | оба | ✅ да | **К** |
| 26.10 a0d6ce3 | v1.0.0+14 HOTFIX (собран из flavors-линии) | rustore | ✅ да | К |
| 27.10 552cccc | **HOTFIX-2 +14: R8 ломал Gson TypeToken в vendored flutter_local_notifications → вечный сплэш Android 16**; proguard-rules (эталон гарда линейки) | оба | ✅ да | **К** |
| 27.10 1f2e7ef | v1.0.0+15: гард silence-first (миграция v14 бьёт только апгрейдеров); промт при глобально выключенных уведомлениях ×12 | rustore | ✅ да | К |
| 29.10 0b3ae27 | storeSdkInit-таймаут 3с | оба | ✅ да | К |
| 30.10 766099a | **Баг-зеркало цен** (пойман на +15 на модерации!; класс KNOWN_ISSUES 0.5) | оба | ✅ да | **К** (баг денег) |
| 30.10 ee033b0 | v1.0.0+16 hotfix (замена APK на том же code запрещена — code+1) | rustore | ✅ да | К |
| 31.10 3bacc85/cf1856d/e993d6a/d69f1e3 | Якоря финал + К3-пейвол (этalon для H₂O/QR) | оба | ✅ да (пакеты 01.11) | М |
| 31.10 b395abf | RU «ты» ×7 (paywallTitle «Продолжай строить привычки» + онбординг + «Войди») | оба | ✅ да (пакеты 01.11) | М |

⚠ **master Habits устарел** (сентябрь + 'ты' 31.10): все октябрьские rustore-релизы
(+14…+16) собраны из feature/play-flavors. Master как «источник правды rustore»
больше не является — зафиксировать в реестре/загрузчиках, чтобы никто не собрал
релиз из master по старой памяти.

---

## 5. FILES (main; одна ветка; релиз НЕ вышел)

| Дата/коммит | Суть фикса | Контур | В play? | Класс |
|---|---|---|---|---|
| 23.10 e7e2879 | Scaffold: каркас из Habits, flavors, BillingRepository, l10n ×12 | оба | ✅ (родилась) | К (архитектура) |
| 24.10 cafd884 | nullable-getter по золотому паттерну; l10n ?. + EN-fallback | оба | ✅ да | М |
| 24.10 c9eb142 | SAF/MES-каркас: MES в обоих манифестах; MethodChannel; StoragePermissionService; AccessRationaleScreen (GP-политика) | оба | ✅ да | К |
| 26.10 39bc4e8 | Иконки лаунчера (генератор PIL, эталон линейки) | оба | ✅ да | М |
| 26.10 623b1e7 | **Сканер хранилища (этап 3)**: FileIndexEntry ×8 категорий; Isolate.spawn+SendPort, порции 200; progress ТОЛЬКО из UI-изолята; cancel через isolate.kill; MES-гейт; кэш агрегатов | оба | ✅ да | **К** |
| 26.10 c32e387 | SAF-скан 3.1: DocumentsContract read-only метаданные, порции 200, отмена, гард циклов, единый индекс, EXTRA_INITIAL_URI | оба | ✅ да | **К** |
| 27.10 c8bff90 | **Фикс trees-notifier** (железный дым A52): список SAF-деревьев жил внутри state-нотифаера → вторая карточка зависала на старом N | оба | ✅ да | К |
| 27.10 a66aa99 | unknownModifiedMillis-сентинель + hasKnownModified (SAF-записи без mtime не сваливались в «старые») | оба | ✅ да | М |
| 27.10 13d3fd5 | Иконка янтарь v3 по гейту К3 9/10 | оба | ✅ да | М |
| 29.10 ff75822 | ICU-plural «1 files» ×12 (scanResultSummary/scanProgressFiles) | оба | ✅ да | М |
| 29.10 b41c338 | **Этап 4: категории → список → навигатор папок** (buildFolderTree O(n), хлебные крошки, SAF-секция по индексу) | оба | ✅ да | **К** |
| 30.10 ba1e955 | RU-формы ICU-plural browserFoldersCount/browserFilesCount | оба | ✅ да | М |
| 30.10 49109c9 | Бренд-полка (порт эталона Cleaner, storeSdkInit-кап 3с, INTERNET-аудит, тест-страж) | оба | ✅ да | М |
| 30.10 398af68 | **Этап 5: корзина + операции** (selection mode, sidecar-метаданные, restore/empty, copy/move/rename, share FileProvider, очередь операций, журнал 100, SAF read-only стражами); тесты 114/114 | оба | ✅ да | **К** |
| 30.10 22bf3af/155a297 | Z-цикл этапа 5: B1 (list() без sidecar), B2 (.nomedia в корзине — MediaStore приватность), W1–W5 (guards, честный снэкбар partial, чанки 32, оптимистичное скрытие, onDestroy-cancel); повторка: unhide-on-failure, жизнь очереди, **W2-проводка снэкбара (реальный баг: messenger после onApplied → unmounted)** | оба | ✅ да | **К** |
| 30.10 d1aef02 | Z блокеры свежего прогона: Б1 progress не залипает active (try/finally); Б2 .nomedia фильтры в list/refreshCount/restoreAll/emptyTrash | оба | ✅ да | **К** |
| 30.10 485172d | Z W1-sidecar: emptyTrash не считает sidecar в ok/bytes; sidecar-write/delete в тихом try/catch (не откатывает успех) | оба | ✅ да | М |
| 31.10 de097d6 | **Этап 5.2 (Z пост-дым)**: N-2 BLOCKER (изолят без границы ошибки молча умирал + FS-опы без таймаута → вечный скан; ScanFailed-терминал всегда, таймауты 5с) + N-1 WARNING (дерево нереактивно + нечестное пустое состояние) | оба | ✅ да | **К** |
| 31.10 b249d6f | .novax* (корзина) скрыты из дерева проводника (isServiceInternalPath-фильтр) | оба | ✅ да | М |
| 31.10 0a525d6 | Канон имени «Files NOVAX»; лейбл «Files» | оба | ✅ да | М |
| 31.10 30547b5/f065611 | TASK_010 переводы ×78 ×12 локалей (RU + 11) | оба | ✅ да | М |
| 01.11 ff99aec | l10n: фикс @@locale ×11 (вливка TASK_010 оставила 'en' — gen-l10n падал) | оба | ✅ да | К |
| 01.11 88cc677 | Cleanup-вкладка скрыта во всех версиях до этапа 6 (решение Гиа; RU-очистка отдельным этапом) | оба | ✅ да | М |
| 01.11 6664a54 | Настройки v1 (тема/О приложении/Политика/EULA живые URL) | оба | ✅ да | К |
| 01.11 01fb3f5 | Поиск v1 Everything-style (индекс, маски, чипы категорий, FileProvider ACTION_VIEW) | оба | ✅ да | **К** |
| 01.11 f50c794 | Z-фиксы волны 1: stat-таймаут, дефолт темы system, PackageInfoFlags, <queries> https, поиск без корзины/Android-data, маска *jpg, MIME-fallback | оба | ✅ да | К |
| 01.11 5296e23 | Превью-миниатюры (фото cacheWidth, видео MediaMetadataRetriever + кэш) | оба | ✅ да | М |
| 02.11 0565e2b | openFile: прямой ACTION_VIEW вместо createChooser (система запоминает «Всегда») | оба | ✅ да | М |
| 03.11 f2e0ff5 | Подача доступа: прямой системный экран APP_ALL_FILES_ACCESS вместо общего списка | оба | ✅ да | М |
| 03.11 0528deb | Настройка языка (Системный + 12 локалей, LocaleService persist) | оба | ✅ да | М |

⚠ **ЖИВОЙ БАГ (03.11, в работе у Р-4):** кнопка «Разрешить доступ» не исчезает
после выданных разрешений (A50, руки Владимира; гипотеза Гиа: MES-статус не
перечитывается на onResume). Диагноз ждётся сегодня.

---

## СЕКЦИЯ «ЧТО УПУЩЕНО В ИНТЛ-ПАКЕТАХ»

1. **Первичная волна 30.10 вышла без К3-пейвола** (H₂O/QR/Habits) — закрыто
   пересборкой 9+3 пакетов 01.11 (пакеты в YandexDisk актуальны). Не дефект —
   хронология; зафиксировано для памяти.
2. **Актуальных упущений в интл-пакетах (состояние на 03.11) — НЕТ:**
   все фиксы feature/play-flavors вошли в пакеты 01.11 (К3-пейвол ×3, «Напиток»,
   «ты» Habits, зеркало цен, storeSdkInit-кап, proguard H₂O/Habits).
3. **Cleaner в интл-контур не входит** (нет play-флейвора) — его фиксы
   (v2.3.2, proguard) касаются только RuStore/altstore.
4. **РИСК-НАХОДКА (линии, не пакеты):** rustore-линии H₂O (main) и Habits (master)
   отстали от flavors-веток. Следующие RuStore-релизы этих приложений собирать
   ТОЛЬКО из feature/play-flavors (H₂O main без keep.xml = повторение краша 4.2).
   Рекомендую зафиксировать в AGENTS/загрузчиках.
5. Files не входил в волну (релиз не вышел) — к портированию не применимо.

---

## ВОПРОСЫ К ГИЕ (на сверку, ШАГ 2)

① Подтверди риск-находку №4 — включаем в список решений?
② Класс «честность» (галка-фантом, CTA без цены) оставил отдельным типом внутри
  «критичного» — ок или выносить в третью колонку?
③ Z-пакет по ШАГУ 3 предварительно: H₂O keep.xml/зеркало/тикер (уже в play — не
  нужен), Habits P1-notifications + orphan alarms + keep.xml + Gson (в play есть),
  Cleaner н/п → фактически Z-пакет может оказаться ПУСТЫМ, т.к. критичное уже
  портировано. Подтверди: порт = только тривия (l10n, косметика) + контроль diff?
