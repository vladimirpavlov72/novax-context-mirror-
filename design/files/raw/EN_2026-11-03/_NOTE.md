# Files — EN raw для видеоинструкции (08.11.2026)

Сырьё для EN-ролика Files (зеркало RU-сценария video_files_scenario_ru.md).
Источник свежих EN-кадров: пакет design_review_GP_2026-11-04 (виза К3 9/10,
commit 85a02a9) + досъёмка.

## Готово (14 кадров в этой папке)
01_home_en (granted), 02_access_why_en (рационале), 03_scan_progress_en,
04_home_scanned_en, 05_images_feed_en, 06_folders_en, 07_folder_level_en,
08_selection_en, 09_trash_empty_en, 10_trash_with_item_en,
11_trash_dialog_en, 12_operations_log_en, 13_search_needs_scan_en,
14_search_empty_en.

## ПРОБЕЛЫ — нужна досъёмка (EN, системные экраны + секция SAF)
G1. Системный экран MES «All files access» (аналог 03_system_mes_detail) —
   EN появится только при SYSTEM-локали en-US (сейчас на A52 DE-дым Р-4).
   Порядок: appops set com.novax.files MANAGE_EXTERNAL_STORAGE default →
   relaunch → карточка доступа → системный экран → screencap → вернуть allow.
G2. Системный SAF-пикер папок EN (аналог 04_saf_picker).
G3. Секция «Выбранные папки» (safFallback folders) в приложении EN.
⚠ A52 08.11 ~16:05 занят DE/AR-дымом Р-4 (b2-фиксы) — окно съёмки
согласовать с Р-4/Гиа. Системную локаль перед G1/G2 переключить на en-US
(без ребута, через Settings UI).

_Р-3, 08.11.2026. Р-3, Session ID: [СКРЫТО:ID]_
