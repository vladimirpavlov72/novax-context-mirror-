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

## 0.5 gen-l10n СОРТИРУЕТ плейсхолдеры АЛФАВИТНО — позиционные вызовы многоплейсхолдерных строк зеркалят (30.10)

**Влияние:** у строки с 2+ плейсхолдерами (например
`{yearlyPrice} в год … {oneTimePrice} один раз`) генератор строит сигнатуру
по АЛФАВИТУ: `subscriptionComparison(Object oneTimePrice, Object yearlyPrice)`.
Позиционный вызов `(yearly, oneTime)` подставляет значения ЗЕРКАЛЬНО —
«берут 159 ₽ в год, просим 1 990 ₽» (Habits/H₂O/QR, Habits +15 чуть не
ушёл на модерации с этим; поймано аудитом Р-3 30.10). Тот же класс, что
Cleaner duplicatesGroupLine 27.10.
**Обходное решение:** вызывать многоплейсхолдерные строки В ПОРЯДКЕ
АЛФАВИТА ИМЁН плейсхолдеров (как в сгенерированной сигнатуре) — смотреть
сигнатуру в generated перед вызовом.
**Постоянное решение:** правило ревью (Z/Р-2): любой вызов l10n-строки
с 2+ плейсхолдерами — сверка с сигнатурой generated. Комментарий у вызова
с перечислением порядка — обязателен.

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

## 4. Проблемы, вскрытые 24.10 (релизный харднинг)

### 4.1 gen-l10n mtime-пропуск: устаревшие generated-файлы в релизе (24.10)
- **Влияние:** Flutter пропускает `gen-l10n`, если generated-файлы НОВЕЕ ARB по mtime.
  В Cleaner generated gitignored → строки из лета попадали в релизы. Вскрыто как баг
  «Можно освободить 33530» без «МБ» в v2.3.0+4030 (на модерации RuStore с этим багом).
  В Habits/QR/H₂O generated закоммичены — push обновляет mtime, проблемы нет.
- **Обходное решение:** перед КАЖДОЙ релизной сборкой Cleaner принудительно
  `flutter gen-l10n`; вериф — маркер-строкой или grep по APK-выводу на девайсе.
- **Постоянное решение:** CI-шаг/скрипт сборки с принудительной генерацией
  (задача вне скоупа 24.10). Пересборка Cleaner — ждёт решения Владимира.

### 4.2 shrinkResources вырезает drawable, референс которого только в Dart-строке (24.10)
- **Влияние:** release-сборки Habits и H₂O умирали на сплэше:
  `PlatformException(invalid_icon, @drawable/ic_notification could not be found)`.
  Debug маскировал (не шринкует). Референс — только в Dart-строке
  AndroidInitializationSettings, aapt не видит.
- **Обходное решение:** `android/app/src/main/res/raw/keep.xml` с
  `<keep tools:keep="@drawable/ic_notification" />` (эталон — паттерн на оба приложения).
- **Постоянное решение:** применено — Habits `e128fbc`, H₂O `6c8dd0c`, ревериф ок.
  Правило: любой drawable, упомянутый только строкой в Dart, — в keep.xml.

### 4.3 play-флейворы падают на сплэше: краш plugin-регистрации (24.10)
- **Влияние:** play-сборки Habits/H₂O/QR (и Cleaner без exclude) крашились на сплэше:
  `GeneratedPluginRegister` → `NoClassDefFoundError: RuStoreAppUpdateManagerFactory`
  → shared_preferences channel-error. Обёртки review/update жадно инстанцировали SDK
  в `onAttachedToEngine`, а `exclude group: 'ru.rustore.sdk'` в play-флейворах вырезает SDK.
  Из-за краша одного плагина не регистрировались ВСЕ остальные.
- **Обходное решение:** патч вендорных `flutter_rustore_review/update-10.5.2`:
  зонд `Class.forName("ru.rustore.sdk.core.Constants")` в начале
  `onAttachedToEngine` → при отсутствии SDK no-op return. Патч — ОБА плагина.
- **Постоянное решение:** применено; play=86 классов (только ссылки, прецедент pay-SDK,
  красная линия цела), rustore=2294. Обе сборки стартуют, UnhandledExceptions=0.
  ⚠ Дым H₂O/QR на девайсе после патча — не прогонялся (APK-счёт классов ок) — бэклог QA.

### 4.4 `adb install -r` молча не обновляет APK (downgrade silent-fail) (24.10)
- **Влияние:** при установке APK с versionCode НИЖЕ установленного `adb install -r`
  возвращает Success, но приложение НЕ обновляется — QA на старой версии без явного сигнала.
- **Обходное решение:** вериф после установки: `dumpsys package <id> | grep versionCode`
  (+ lastUpdateTime), сравнить с целевым APK. ⚠ Усилено 24.10: downgrade
  ТОЛЬКО через uninstall+install; install -r поверх ВЫСШЕГО versionCode
  молча не применяется (Success врёт), dumpsys сразу после может врать
  двояко — верифить дважды с интервалом. Инструментальное: Dart-строки
  release-APK ищутся не в classes.dex, а в lib/arm64-v8a/libapp.so.
- **Постоянное решение:** QA-чек-лист: версия после install верифицируется всегда.

### 4.5 ОПУБЛИКОВАННЫЙ Habits +13 (RuStore) сломан — живой инцидент (24.10)
- **Влияние:** опубликованные 13.10 Привычки v1.0.0+13 не открываются после
  установки/обновления: холодный старт падает с PlatformException(invalid_icon)
  (тот же shrinkResources-баг, что 4.2 — но keep.xml в мастере появился ПОСЛЕ
  заливки). Подтверждено: md5 заливки == releases\ +13 (`e360ade…`), ресурса нет.
  Живые процессы/доставка уведомлений работают — поэтому жалобы о напоминаниях
  21.10 не противоречат. Отката версий в RuStore нет.
- **Обходное решение:** пользователям — переустановка не поможет (та же сборка);
  команде — hotfix v1.0.0+14 (master готов: keep.xml e128fbc + Z-финал da8f187,
  14/14 тестов). Эскалация Гие 24.10 16:35, ждём решения Владимира.
- **Постоянное решение:** выпуск +14 и обновление в консоли; для будущих релизов
  линейки — правило из 4.2 (keep.xml) + QA релизов только release-сборками
  холодным стартом на девайсе.

### 4.6 R8 vs Gson TypeToken — релиз виснет на сплэше Android 16 (27.10)
- **Влияние:** R8-обфускация ломает Gson TypeToken в vendored
  flutter_local_notifications (`Missing type parameter` в
  loadScheduledNotifications). Исключение в platform-handler → канал не
  отвечает → Dart await вечно → ВЕЧНЫЙ СПЛЭШ. Процесс жив, FATAL нет.
  A52 не ловит (boot-путь не дёргается), Android 16 — дёргает. Истинный
  краш +13 на новых устройствах, скорее всего, это (не только keep.xml).
- **Обходное решение:** proguard-rules: `-keepattributes Signature`,
  `*Annotation*`, `-keep class com.google.gson.**`, `-keep class *
  extends com.google.gson.reflect.TypeToken`, keep плагина. Фикс +14
  (HOTFIX-2), QA 14T+A52 зелёный.
- **Постоянное решение:** применено в Habits. 🔴 НЕ применено в Cleaner
  (reminder_service!) и H₂O — тот же риск на Android 16 при
  boot-reschedule. Добавить те же правила при следующем касании (Р-2).
  Правило линейки: flutter_local_notifications + minify → Gson-правила
  ОБЯЗАТЕЛЬНЫ.

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
| — | Habits: сиротские алармы вне кэша плагина | hardCancelAll через DISABLED-компонент ScheduledNotificationReceiver; chokepoint ensureRemindersReceiverEnabled (`f73bca1`) | 2026-10-24 |
| — | Play-флейворы крашились на сплэше (RuStore SDK в plugin-register) | Вендор-патч review/update: ленивый no-op по зонду `ru.rustore.sdk.core.Constants` | 2026-10-24 |
| — | Release Habits/H₂O умирали на сплэше (invalid_icon) | `res/raw/keep.xml` с keep `@drawable/ic_notification` | 2026-10-24 |
| — | Cleaner «Можно освободить 33530» без «МБ» | ИСТИННЫЙ корень (QA 24.10, v2.3.1): визуальный клип рендера — maxLines:1 + Flexible + clip резали хвост « МБ» при ~5-значных числах (semantic-текст был корректным!). Фикс b2f1fe8 (maxLines 2 + wrap). l10n mtime-риск (4.1) — сопутствующий, не причина | 2026-10-24 |

## Issue: Переводческий долг QR (v2.0.22)
- `eulaBody` — EN fallback на 11 локалях (решение 26.07: юртекст автопереводом не трогаем)
- `loading`, `shareQrText` — EN fallback, автоперевод отложен по решению 26.07
- Исполнитель: in-house (Рома+Владимир); вычитка Кими

## Issue: Домен novaxhub.ru не верифицирован (блокер GEO) — РЕШЕНО
- Сайт мигрирован на собственный VPS `[СКРЫТО:IP]` (nginx + SSL Let's Encrypt).
- GitHub Pages больше не используется как primary хостинг; TXT-запись для GitHub Pages не требуется.
- GEO-пакет (`llms.txt`, Schema.org, интент-лендинги) задеплоен и доступен.

## Issue: Инфраструктура — Kimi CLI не запускал сессии (Git Bash исчез) — РЕШЕНО
- 28.10.2026: Kimi CLI 1.49.0 не запускал сессии из-за отсутствия Git Bash.
  Git for Windows 2.56.0.windows.4 переустановлен в C:\Program Files\Git;
  запуск и возврат к сессиям (kimi -S) восстановлены. Папка .kimi не изменялась.
- Причина исчезновения прежней установки Git — НЕ УСТАНОВЛЕНА (подозрение:
  внешний чистильщик/обновление; наблюдать).
- УРОК: shell-зависимость CLI — проверять `which bash` при «странных» отказах
  запуска до переустановки CLI; Git Bash живёт в C:\Program Files\Git\bin\bash.exe.

## Issue: QR — холодный старт ~2 минуты (реальный сценарий основателя) — НЕ ВОСПРОИЗВОДИТСЯ (цифры ниже), наблюдение
- 28.10: Владимир на 14T открыл опубликованный NOVAX QR для сканирования
  QR самоката (прокат) — старт ~2 мин. Пользователь в реальности удалил бы
  приложение и оставил негативный отзыв. Расследование: Р-2 (замеры am start -W,
  профиль main(), подозрение: ML Kit/модели/блокирующие init).
- 28.10 ЗАМЕРЫ Р-2 (A52, am start -W, 5 прогонов + first-launch):
  опубликованный +10: стабильный 357–405 мс, first-launch 456 мс;
  master +2028: стабильный 463–510 мс, first-launch 1309 мс.
  A52 НЕ воспроизводит — 2 мин там нет. Профиль main() +2028 logcat:
  APP START→LOCALE ~1 мс, далее storeSdkInit (RustoreReview+Update, СЕТЬ,
  без таймаута, await ДО runApp) ~140 мс на Wi-Fi → при плохой мобильной
  сети может висеть до минут. У +10 storeSdkInit нет.
  Камера = qr_code_scanner (ZXing, офлайн) — ML Kit в камере НЕ участвует;
  ML Kit только в галерее, модели bundled (~880 КБ tflite в обоих APK).
  Вывод: ML Kit как причина старта ИСКЛЮЧЁН. Кандидаты: (1) HyperOS
  first-launch dexopt после обновления (one-time, нужен 14T), (2) камера-HAL
  до scan-ready (нужен 14T), (3) storeSdkInit без таймаута (+2028, риск-регресс).
  БЛОКЕР: 14T офлайн по adb (порт закрыт, хост жив) — замеры на нём ждут
  устройства. Скрипт замеров готов: C:\dev\tmp\qr_coldstart_bench.sh.
- 28.10 ЗАМЕРЫ-2 Р-2 (14T подключён, APK +10 переустановлен Владимиром
  руками 21:00): стабильный старт 222–343 мс; сканер до ЖИВОЙ камеры
  ≤1–2 с после системного permission (скрины qa_qr_cam_t*.png). «2 мин»
  на 14T тоже НЕ воспроизводится. A52 авиарежим +2028: 458–469 мс —
  storeSdkInit НЕ блокирует даже офлайн (исключение ловится).
  baseline.prof в обоих APK на месте (first-launch 264–456 мс это
  подтверждает). Инцидент = одноразовый системный фактор (first-launch
  после обновления/HyperOS-заморозка); улики переустановкой стёрты.
  ВЕРДИКТ: приложение не виновато по всем измеримым метрикам. Дёшево
  закрыть регресс-риск storeSdkInit timeout-обёрткой (на визу Ги).

## Issue: l10n placeholder-order — цены на пейволе меняются местами — В РАБОТЕ
- 30.10, поймал Владимир на скрине H₂O (debug-сборка Р-3): экран показывал
  «Другие берут 159 ₽ в год. Мы просим 1 990 ₽ один раз» — ЗЕРКАЛЬНО канону.
- Корень: gen-l10n строит сигнатуру в порядке placeholders из ARB, а не по
  порядку использования в строке; вызовы передают (yearly, oneTime) —
  цифры встают наоборот. Баг класса: ВСЕ строки с 2+ плейсхолдерами во
  всех приложениях. Исполнитель: Р-2 (фикс H₂O ×12 + аудит Cleaner/QR/
  Habits/Files). Урок: multi-placeholder строки верифицировать на экране
  ДО релиза, не по коду.
