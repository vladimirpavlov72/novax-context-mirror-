# Отчёт: D3 QA (бренд-полка) на A52 + инцидент регистрации плагинов

**Дата:** 2026-10-24
**Автор:** Рома-2
**Объект:** novax_habits @0cac063 (D1+D3) + QA-фиксы, debug-сборки обоих флейворов

## D3 QA — ВСЕ ПУНКТЫ ЗЕЛЁНЫЕ

A52 (RZ8R409QVQB), скрины `C:\dev\tmp\qa_h3_shelf_*.png`:

1. ✅ **Экран полки виден** (rustore, STORE=rustore): Настройки → О приложении →
   тайл «Другие приложения NOVAX» → экран с 3 карточками (Cleaner/H₂O/QR),
   локальные иконки, подзаголовок «Одна команда. Честные приложения…».
2. ✅ **Тап «Открыть в магазине»** → `topResumedActivity=ru.vk.store/.app.
   MainActivity` — RuStore открыл карточку (qa_h3_shelf_06_store.png).
3. ✅ **INTERNET-аудит**: в коде полки нет сетевых вызовов (открытие через
   intent); INTERNET в манифесте — только billing, досуществующий.
4. ✅ **Пустой список = скрыт** (play, STORE=play): тайла нет, приложение
   работает (qa_h3_play_settings.png).

## Вскрытый и закрытый дефект №1: layout карточки (все 4 репо)

Скрин qa_h3_shelf_05_screen.png: кнопка «Открыть в магазине» в строке карточки
сжимала текстовую колонку до ~150 px, названия рвались посимвольно
(«NOVA/X/Cleane/r»). Фикс: тексты на полную ширину, кнопка — внизу карточки
по правому краю. Коммиты: Cleaner `8834b2d` (эталон), QR `8b3f1de`,
H₂O `acdc2cb`, Habits — в коммите ниже. Девайс-ревериф Habits: ок
(qa_h3_shelf_10_final.png).

## Вскрытый и закрытый дефект №2: 🔴 play-флейвор умирал на сплэше (P0)

**Симптом:** play-debug после D1 валился на старте: `Unhandled Exception:
PlatformException(channel-error, Unable to establish connection on channel:
"dev.flutter.pigeon.shared_preferences_android.SharedPreferencesApi.getAll")`
→ runApp не вызывался → вечный сплэш. Rustore-флейвор работал. Детерминировано.

**Диагностика (по шагам):**
1. fb4a126 (до моих D1/D3) play работает → регрессия моих коммитов.
2. Dex-дифф падавшего и рабочего APK: классы `ru.rustore.sdk.*` есть в rustore
   (2294 строк), в play — только 86 ссылок-дескрипторов в обёртках плагинов.
3. Лог: `GeneratedPluginsRegister: Caused by: NoClassDefFoundError:
   ru/rustore/sdk/appupdate/manager/factory/RuStoreAppUpdateManagerFactory at
   FlutterRustoreUpdatePlugin.onAttachedToEngine`.

**Корень:** `android/app/build.gradle` (Z-мера платёжного трека) исключает
группу `ru.rustore.sdk` из всех play*-конфигураций — красная линия
GP-деконтаминации. Но регистрация плагинов flavor-слепа:
GeneratedPluginRegistrant регистрирует ВСЕ плагины pubspec в обоих флейворах.
Обёртки review/update SDK-зависимости тянут жадно (клиент создаётся прямо в
onAttachedToEngine) → NoClassDefFoundError → **цепочка регистрации обрывается**
→ SharedPreferencesPlugin (позже по списку) не аттачится → приложение мертво.
Pay-плагин выживал только потому, что его onAttachedToEngine SDK не трогает.

**Фикс (вендор-патч, общий для всех flavors-репо):** обёртки
`C:\dev\vendor\packages\flutter_rustore_{review,update}-10.5.2` — проба
`Class.forName("ru.rustore.sdk.core.Constants")` в onAttachedToEngine; при
отсутствии SDK — инертный no-op (Dart-сторона StoreSdkService всё равно в
try/catch; вызывается только из main_rustore). Красная линия не нарушена:
SDK-классов в play-APK по-прежнему нет (86 ссылок-дескрипторов — прежний
уровень, тот же прецедент, что и pay-обёртка).

**Ревериф A52:** play — 0 unhandled exceptions, старт ок, полка скрыта;
rustore — 0 exceptions, полка ок, SDK-классы на месте.

**Распространение:** H₂O и QR имеют тот же gradle-exclude → их play-флейворы
бы умерли так же (живых post-D1 сборок у них нет — APK от 09.10).
Вендор-патч чинит глобально: их следующие сборки подхватят. Cleaner
флейворов не имеет (rustore-only) — не затронут.

**Урок-эталон для flavors-репо:** любой новый RuStore-обёрточный плагин ОБЯЗАН
лениво инициализировать SDK или не аттачиться при его отсутствии — иначе
убивает всю регистрацию плагинов GP-флейвора. Записать в KNOWN_ISSUES при
следующей правке доков.

Session ID: [СКРЫТО:ID]
