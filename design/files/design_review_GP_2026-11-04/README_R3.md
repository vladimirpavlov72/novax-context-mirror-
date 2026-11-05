# ДИЗАЙН-РЕВЬЮ Files NOVAX v1.0 — ПАКЕТ R3 (05.11, вечер)

**От:** Р-4. **Сборка:** play-debug из HEAD `6f65c65` (l10n-контур закрыт
полностью: B1 + W1/W2/W3 ×12, включая ночную вливку Ромы 6d681da).
**Окно съёмки:** A52 (RZ8R409QVQB), подтверждённое, телефон никто не трогал.
**Метод:** каждый кадр — свежий скриншот в момент съёмки, идентичность
экрана проверена `uiautomator dump` по паттерну имён (НЕ яркостью).
Дублей и переименованных старых кадров нет (проверено хэшами, см. ниже).
Старые кадры пакета R2 перезаписаны этими файлами.

## Состав: 26 кадров (01–20 серия + 6 специальных)

| Файл | Локаль/тема/масштаб | Содержимое (dump-проверка) |
|---|---|---|
| 01_home_granted | EN, светлая, 1.0 | «Storage analysis», «Full access is enabled» |
| 02_access_rationale | EN, светлая | «Why we need access» + 3 обещания честности |
| 03_system_mes_detail | системный RU (экран системы) | «Доступ ко всем файлам / Files» — прямой экран MES |
| 04_saf_picker | системный RU | DocumentsUI («A52 пользователя Vladimir»), пикер папки |
| 05_scan_progress | EN, светлая | прогресс-бар, «283 files», «Data read», «Current folder», Cancel |
| 06_home_scanned | EN, светлая | итог «12056 files · 115.3 GB», категории с данными |
| 07_images_feed | EN, светлая | лента Images с миниатюрами, сорт по размеру |
| 08_selection_mode | EN, светлая | «1 selected», панель 5 кнопок, деструктивное ПОСЛЕДНИМ (подписи однострочные) |
| 09_search_results | EN, светлая | запрос «2022», результаты с путём папки |
| 10_folders_root | EN, светлая | проводник, корень /storage/emulated/0 |
| 11_folder_level | EN, светлая | Android → .Trash + media, честный путь |
| 12_more | EN, светлая | Trash / Operations log / Settings |
| 13_trash_empty | EN, светлая | «Trash is empty» (после Empty trash) |
| 14_after_trash_snackbar | EN, светлая | snackbar «Done: 1 of 1» после Move to trash |
| 15_trash_with_item | EN, светлая | корзина с элементом (имя, размер, from-путь) |
| 16_empty_trash_dialog | EN, светлая | «Delete forever? … cannot be undone» |
| 17_operations_log | EN, светлая | журнал с локализованными подписями («Moved to trash · 1 · 29.0 MB») |
| 18_settings | EN, светлая | тема/язык(English)/о приложении/legal |
| 19_language_dialog | RU, светлая | диалог языка: Системный + 12, «한국어» исправлено |
| 20_home_dark | EN, тёмная | главная в тёмной теме с данными |
| b2_panel5_de_scale135 | DE, ТЁМНАЯ, font scale 1.35 | панель 5 кнопок: деструктивное последним, error-цвет, 2 строки |
| b2_panel3_de_scale135 | DE, ТЁМНАЯ, font scale 1.35 | панель 3 кнопок (2 ausgewählt) |
| rtl_ar_home | AR (RTL), ТЁМНАЯ | главная зеркально, «تحليل التخزين» (W1 ar) |
| rtl_ar_selection_panel | AR (RTL), ТЁМНАЯ | выбор: панель зеркально, красное последним, имена НЕ перевернуты (bidi-фикс) |
| rtl_ar_dialog_rename | AR (RTL), ТЁМНАЯ | диалог переименования в RTL, поле имени LTR (bidi-фикс) |
| clean_home_light_ru | RU, светлая, 1.0 | чистая главная «Анализ хранилища» |
| icon_512.png | — | принят К3 в R2, без изменений |

## Проверки целостности (скриптом)

- Все PNG 1080×2400, размер > 30 КБ.
- Попарные хэши: 26 уникальных файлов, совпадений нет.
- README сверен со списком PNG скриптом (имя в таблице = файл на диске).

## Честные пометки

- 03/04 — системные экраны, язык системы (RU), не приложения.
- 19 снят в RU (канон команды); при необходимости EN-версия — за 2 минуты.
- font scale: на A52 значение 1.3 молча не применяется (особенность One UI),
  стресс-тест выполнен на 1.35 — фиксация в CHANGELOG репо Files.
- Ложное срабатывание дыма («Done: 0 of 1, 1 skipped» на устаревшем индексе)
  воспроизведено и задокументировано; честный partial работает как задумано.


---

# ДОПОЛНЕНИЕ R3.1 (05.11, вечер) — закрытие блокеров R3

**Сборка:** play-debug HEAD `2442867` (bidi-фикс + типографика панели).
Полная пересъёмка НЕ производилась — точечные замены.

## Заменённые файлы (4)

| Файл | Что изменилось |
|---|---|
| 14_after_trash_snackbar | переснят С ВИДИМЫМ snackbar «Done: 1 of 1» (скриншот в момент показа) |
| rtl_ar_selection_panel | bidi-фикс: латинские имена читаются LTR, не перевёрнуты |
| rtl_ar_dialog_rename | bidi-фикс: имена в списке LTR + поле ввода Directionality LTR |
| b2_panel5_de_scale135 | типографика панели: FittedBox scaleDown — слова целые (de 1.35) |

## Новые файлы (2) — W3-строки поиска, EN

| Файл | Содержимое |
|---|---|
| search_needs_scan_en | до скана: «Run a scan first — without it search has nothing to look through» (без «index») |
| search_empty_en | после скана, пустой запрос: «Type a file name or mask — search is instant, over the last scan» (без «index») |

## Целостность

- SHA-256 шести файлов выше — в `SHA256SUMS_R3.1.txt`.
- НЕ тронуты (git: только 4 замены + 2 добавления, `git status` по
  папке): 19 серийных кадров (все, кроме 14), b2_panel3_de_scale135,
  rtl_ar_home, clean_home_light_ru, icon_512.png.
- Код bidi-изоляции: lib/utils/bidi.dart + применение в ленте, поиске,
  корзине, дереве, карточке «Папки»; Directionality LTR в диалоге
  переименования. Тест 151/151, analyze 0. Коммит `2442867` (push ✓).
