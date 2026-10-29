# QR — пересъёмка kadr1/2/4/5 (A52, 29.10.2026, debug с l10n-фиксом «ТЕКСТ»)

## Контекст
Гейт К3: QR kadr1 home в FREE (был premium), kadr2/kadr4/kadr5 EN без системного
«Мероприятия», kadr5 RU после фикса «ТЕКСТ», kadr6 — живая камера (см. ниже).
Сборка: C:\dev\tmp\qr_l10nfix_debug.apk, md5 6d4fb6d2 (коммит Р-2 1f9ea65),
 debug-подпись. Пакет: com.novax.qrscanner (НЕ com.novax.qr!). Устройство:
Samsung A52 (USB), 1080×2400, font_scale 1.3 (не менял). Статус-бар на всех
кадрах чистый — «Мероприятия» нет.

## Файлы (v2, сняты 14:06–14:35)
- home_free_a52_v2_en.png / _ru.png — home FREE после pm clear: «7 days left» /
  «Осталось 7 дн.» (триал-плашка = честное free-состояние, спека Ги).
- scan_from_gallery_result_a52_v2_en.png — результат скана из галереи:
  LINK · https://example.com/novax-qr-test (сэмпл samples/01_url.png).
- barcode_result_a52_v2_en.png — штрих-код samples/05_barcode.png →
  TEXT · NOVAX-QR-2027.
- history_a52_v2_en.png / _ru.png — история после сканов: «TEXT · Scan» +
  «LINK · Scan» (EN), «ТЕКСТ · Сканировать» (RU) — ЛОКАЛИЗАЦИЯ ТИПА ВИДНА.
Флоу: pm clear → EULA (скролл до конца → Принять) → EN-цикл (системная локаль
EN через Settings UI; галерея: Photo access → пикер → сэмпл → Done) →
переключение системы на RU → pm clear → RU-цикл (скан штрих-кода из галереи).
⚠ Известный баг (виза Р-2): история не обновляется при первом открытии вкладки
после скана (IndexedStack) — лечится force-stop + relaunch; так и снято.

## kadr6 (живая камера) — отдельным файлом
scanner_live_camera_a52_v2_{ru,en}.png — если в папке есть; снимается с
помощью Владимира (A52 камерой к монитору, образец samples/01_url.png
фулскрином на ПК). Без рук в кадре.

_Р-3, 29.10.2026. Session ID: [СКРЫТО:ID]_
