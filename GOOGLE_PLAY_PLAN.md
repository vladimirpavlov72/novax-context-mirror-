# GOOGLE PLAY PLAN — миграция NOVAX (Habits, H₂O, QR) в Google Play

> Создан: 2026-09-14. **АКТУАЛИЗАЦИЯ 09.10: архитектура «отдельные GP-репо»
> ОТМЕНЕНА** — решение Владимира 07.10: flavors в основных репо (ветки
> `feature/play-flavors`, трек ЗАВЕРШЁН 09.10, см. core/CHANGELOG.md).
> gplay-копии — резерв, не трогать. Решения 09.10 (манифест против подписок
> с ценами по флейвору, NOVAX Files, путь запуска internal→closed→production)
> — в DECISIONS.md. Статус: к исполнению после DUNS (~01.11).
> Правило: выполненный план переезжает в `archive/` (см. правило в `CONTEXT_SHORT.md`).
> Cleaner НЕ входит в этот план — по нему будут доп. правки, миграция отдельно позже.

---

## Решения Владимира (зафиксировано 14.09)

1. **Структура: два отдельных проекта на приложение** (RuStore-репо не трогаем, создаём GP-копии). Общие правила — в одном месте, локальные — в каждом проекте под свой магазин. Архитектура с запасом под Samsung / Huawei / Xiaomi / etc.
2. **Аккаунт Google Play: организация через ИП Грузии** (после командировки в Тбилиси). Орг-аккаунт = без closed testing 12×14, но нужен D-U-N-S номер (до ~30 дней, бесплатно).
3. **Порядок выкладки: QR → H₂O → Habits** (подтверждено Z 09.10: QR — проще
   всего для ревью; Habits ждёт фикс уведомлений). Путь треков: internal →
   closed (3-5 тестеров, 3-4 дня) → production — защита от авто-бана свежего
   орг-аккаунта. Пейволы: честный манифест против подписки (цены по флейвору).
4. **Русский язык оставляем** во всех приложениях.
5. **Цена: $2.99 во всех странах**, конвертацию стран делает сам Google Play. Все «159 ₽» и «₽» из кода и текстов убираем.
6. Все упоминания RuStore, rustore.ru, рублей, «российских корней» — убрать из GP-версий (санкционная чистота: бренд NOVAX нейтральный, платёжный обработчик — Google Play).

## Требования Google Play (исследование, 2026)

| Требование | Следствие для нас |
|---|---|
| Только **AAB** (Android App Bundle), APK в production не принимают | Пайплайн `flutter build appbundle`; APK остаётся для локального QA через bundletool |
| **targetSdk 35** сейчас; **36 с 31.08.2026** | QR уже 35; H₂O/Habits 34 → поднять. Все → 36 к концу августа 2026 |
| **16 KB страницы памяти** (обяз. с 01.11.2025 для Android 15+) | Наш Flutter **3.24.0 не подходит** → апгрейд до **Flutter 3.32+**, AGP 8.5+ (лучше 8.7+), NDK r28+, Gradle 8.9+. САМЫЙ РИСКОВАННЫЙ ПУНКТ |
| **Play App Signing** обязателен | Новый upload-key (не тот, что для RuStore); Google переподписывает. Пароли ключей ВЫЧИСТИТЬ из git |
| **Google Play Billing** обязателен для цифровых товаров | Официальный плагин `in_app_purchase: 3.2.3` — УЖЕ в локальном pub-кэше, pub.dev не нужен; billingclient:7.1.1 качается с maven.google.com (доступен) |
| Верификация разработчика (ID/документы) | Владимир в Тбилиси: паспорт + документы ИП |
| **Data safety form**, privacy policy URL (публичная), контентный рейтинг IARC, 18+ (не для детей), декларация «нет рекламы», **DSA trader declaration** (продажи в ЕС) | Чеклист этапа 5; privacy policy хостим на novaxhub.ru |
| Рег. сбор $25 (разово) | — |
| Русский язык в приложении — разрешён | Оставляем (решение 4) |

## Аудит кодовой базы (14.09, факты)

- RuStore SDK изолирован в **одном файле на проект**: Habits/QR — `lib/services/billing_service.dart` / `premium_service.dart` (flutter_rustore_pay 10.3.1, vendored); H₂O — `premium_service.dart` (flutter_rustore_billing 0.0.5, vendored, другой API).
- `in_app_purchase 3.2.3` + `in_app_purchase_android 0.4.0` уже в pub-кэше `AppData\Local\Pub\Cache` — добавляем офлайн.
- Цены: Habits — «159 ₽» захардкожен во ВСЕХ 12 ARB (onboarding, paywall, fallback); H₂O — фолбэк «159 ₽»; QR — фолбэка нет (вечный спиннер без RuStore).
- Тексты: QR-EULA («Оплата обрабатывается магазином RuStore») во всех 12 локалях + хардкод в `privacy_policy_screen.dart:46`; баннеры «RuStore не установлен/авторизуйтесь» в paywall (Habits, QR); ссылки `rustore.ru` «Оценить/Поделиться» в settings (Habits:125,136; H₂O:262,275); мёртвый ключ `paywallOldPrice` «799 ₽» (QR).
- Manifest: диплинки `*:iamback`, meta-data console_app_id (Habits, QR), proguard keep `ru.rustore.**`, debug подписан release-ключом (QR) — чистить.
- **Один общий keystore `novax_release.jks` + пароли в открытом виде в репозитории** (`keystore.properties`, `key.properties`) — для GP сделать новый upload-key, пароли из git убрать (rotate).
- INTERNET у QR с комментарием «only for RuStore billing» — в GP-версии убрать (плюс в Data safety: «не передаёт данные»).
- Модель монетизации везде одинаковая: 7-дневный триал (SharedPreferences, анти-тампер) → разовая покупка full-access; статус кэшируется локально. Покупатели RuStore в GP-версии не мигрируют (чистая установка, новый магазин) — это ок.

## Структура репозиториев (решение 1)

```
C:\dev\apps\
  novax-qr-scanner\        ← RuStore (заморожен для GP-правок)
  novax_h2o\               ← RuStore
  novax_habits\            ← RuStore
  gplay\
    novax-qr-scanner\      ← GP-копия (новый git-репо или orphan-ветка)
    novax_h2o\             ← GP-копия
    novax_habits\          ← GP-копия
```

- **Общие правила** — `novax-context-main/AI_RULES.md` (общий для всех, включая правило глобального бага, см. ниже).
- **Локальные правила** — `AGENTS.md` в каждом проекте: целевой магазин, product id, billing-бэкенд, цена, список «что нельзя трогать».
- **Правило глобального бага** (в общих правилах): баг во всех копиях → фикс в одном, затем по шаблону патча — в остальные; в конце — отметка в `core/CHANGELOG.md` со списком применённых копий. Дисциплина против рассинхрона.
- Мои доводы за flavors (один код) учтены, но решение Владимира — копии; плюсы копий: RuStore-релизы физически не трогаем, независимые git-истории, проще продавать/передавать отдельные активы. Защита от рассинхрона — правило глобального бага + эталонный проект QR.

## Этапы

### Этап 0. Решения и скелет (перед кодом)
- [ ] Утвердить: отдельные репо в `C:\dev\apps\gplay\` (не форки внутри существующих)
- [ ] Общий `AI_RULES.md`: правило глобального бага + чеклист «что проверить после каждого фикса»
- [ ] `AGENTS.md` шаблон для GP-проектов (магазин=Google Play, $2.99, billing=Google)
- [ ] VPN у Владимира: включить на время апгрейда Flutter (pub.dev, storage.googleapis.com). Не критично — обходимся кэшем/GitHub, но снижает риски
- [ ] A52 на шнурке (14T в переезде) — QA-устройство

### Этап 1. Инфраструктура (начинаем с QR-эталона)
- [ ] **Отдельный Flutter SDK для GP**: `C:\dev\flutter_gp\` (3.32+), текущий `C:\dev\flutter\` 3.24 остаётся для RuStore-сборок — ничего рабочего не ломаем. Проекты GP настраиваются на flutter_gp через `FLUTTER_ROOT`/local.properties
- [ ] Апгрейд Flutter 3.24 → 3.32+ (проверка пакетов из офлайн-кэша, `flutter analyze`, сборка, QA на A52)
- [ ] AGP → 8.7+, Gradle → 8.9+, NDK r28+, targetSdk 36
- [ ] `flutter build appbundle --release` пайплайн, скрипт проверки 16 KB ELF alignment
- [ ] Новый GP upload-key (отдельный от RuStore), пароли НЕ в git
- [ ] Прогнать на A52 через bundletool (install apks из AAB)

### Этап 2. Биллинг Google Play
- [ ] `in_app_purchase: 3.2.3` (офлайн из кэша) в GP-проект
- [ ] BillingService: Google Billing (инициализация, покупка, restore, price из SDK)
- [ ] Продукты: `novax_qr_full_access`, `novax_h2o_full_access`, `novax_habits_full_access` (в коде те же id)
- [ ] Фолбэк-цена «$2.99» на всех 12 языках вместо 159 ₽
- [ ] Логика триала не меняется (локальный, 7 дней)

### Этап 3. Деконтаминация GP-версий
- [ ] EULA/политика: «обработчик платежей — Google Play» (все 12 локалей; QR — самый большой блок)
- [ ] Убрать: RuStore-баннеры из paywall, ссылки rustore.ru, диплинки `*:iamback`, meta-data console_app_id, proguard ru.rustore, debug-signing release-ключом
- [ ] У QR убрать INTERNET-разрешение; проверить Data safety формулировки
- [ ] «Оценить/Поделиться» → `https://play.google.com/store/apps/details?id=<package>`

### Этап 4. QA
- [ ] AAB → A52: онбординг → триал → paywall (license tester) → покупка → restore
- [ ] Светлая/тёмная темы, 12 языков, повороты, edge-to-edge
- [ ] 16 KB check, размер AAB (< 200 MB)

### Этап 5. Пакет для консоли (готовим до аккаунта)
- [ ] Листинги: названия, описания (en + ru обязательно, остальные по желанию), иконка 512, feature graphic 1024×500
- [ ] Скрины 9:16 (есть) + планшетные (опционально)
- [ ] Privacy policy URL на novaxhub.ru (без RuStore-упоминаний)
- [ ] Тексты для: Data safety, IARC-опросник, 18+, нет рекламы, DSA trader declaration
- [ ] $25 регистрация — после Тбилиси

### Этап 6. После аккаунта (Тбилиси)
- [ ] Верификация ИП + D-U-N-S → org-аккаунт
- [ ] Создать in-app товары $2.99, license testers
- [ ] Internal track → production (org-аккаунт: без 12×14)
- [ ] Повторить этапы 1–5 для H₂O, затем Habits

## Риски

| Риск | Митигация |
|---|---|
| Апгрейд Flutter сломает пакеты (pub.dev закрыт) | Проверка кэша до апгрейда; vendored-копии критичных пакетов |
| 16 KB не пройдёт на старых плагинах | Скрипт проверки ELF alignment на этапе 1, до всего остального |
| Копии проектов рассинхронятся | Правило глобального бага + QR как эталон + CHANGELOG |
| D-U-N-S задержит org-аккаунт | Подавать запрос сразу в Тбилиси (до 30 дней), параллельно этапам 1–5 |
| Google отклонит за RU-контент | Чистка этапа 3; бренд NOVAX нейтральный; язык ≠ «российские корни» |

## Открытые вопросы

- [ ] Репозитории GP-копий: новые git-репо на GitHub Владимира? (нужен доступ/права)
- [ ] Feature graphic 1024×500 — рисует Рома?
- [ ] Privacy policy для GP: страница на novaxhub.ru без RuStore-упоминаний — готовим сами


## Этап 1 выполнен (QR, 14.09) — находки для копий H₂O/Habits

Статус: ✅ Flutter 3.47.4 (`C:\dev\flutter_gp`) + QA debug APK на A52. Коммит `df9a6a3` в `C:\dev\apps\gplay\novax-qr-scanner`.

Итоговый стек (обязателен для GP-копий): **Gradle 8.14.3, AGP 8.12.0, Kotlin 2.2.20, compileSdk/targetSdk 36, NDK 28.2.13676358, Java 17**. Flutter 3.47.4 отклоняет ниже: Gradle <8.14, AGP <8.11.1, Kotlin <2.2.20. Варнинги «will soon be dropped» (Gradle 9.1/AGP 9.0.1/Kotlin 2.3.20) — пока только варнинги, AGP 9 не трогаем (new DSL only).

Подводные камни (все решены):
1. `intl` ^0.19 → ^0.20.3 (требование flutter_localizations 3.47).
2. Нужен `flutter: generate: true` в pubspec + убрать `synthetic-package` из l10n.yaml (gen_localizations падает без флага).
3. `qr_code_scanner` требует `coreLibraryDesugaringEnabled true` + `desugar_jdk_libs:2.1.4` (проверка AAR metadata в AGP 8.12).
4. `image_gallery_saver` (vendored): удалить неиспользуемый импорт `PluginRegistry.Registrar` — v1 embedding удалён (патч безопасен и для RuStore 3.24).
5. `path_provider_android 2.3.x` тянет `package:jni` (CMake/ninja) → падает на кириллическом pub-кэше (`C:\Users\Владимир`). Запинен `path_provider_android: 2.2.15` через dependency_overrides.
6. A52: GP-debug конфликтует с установленной RuStore-версией по подписи → `adb uninstall` перед install (данные QA-девайса теряются, это норма).

QA-сценарий этапа 1 (прошёл полностью): EULA ru («магазином Google Play») → home (триал 7 дн.) → debug expire trial → пейволл $2.99 (фолбэк, спиннера нет) → «Восстановить покупки» → «Предыдущие покупки не найдены» → «Продолжить» → карточка «Ошибка биллинга» (ожидаемо: приложение не опубликовано) → тёмная тема. Logcat чистый.

Биллинг: `premium_service.dart` переписан на in_app_purchase 3.2.3 с тем же публичным API; PremiumState.isStoreReady вместо isRuStoreInstalled/isRuStoreAuthorized; getPrice() фолбэк '$2.99'; покупка — buyNonConsumable + completePurchase (acknowledge) в стриме; restore — restorePurchases() + проверка _premiumKey.
