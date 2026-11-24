# H₂O — пересъёмка kadr1–5 (A52, 29.10.2026, debug с l10n-фиксом «мл»/«кг»)

## Контекст
Гейт К3: H₂O kadr1–5 RU+EN с единицами «мл»/«кг» (было «ml»/«kg» в RU).
Сборка: C:\dev\tmp\h2o_l10nfix_debug.apk, md5 5a6fbb48 (коммит Р-2 00cbb65),
 debug-подпись, install -r через /data/local/tmp. Устройство: Samsung A52
(RZ8R409QVQB, USB), 1080×2400, font_scale 1.3 (не менял). 14T НЕ использован:
Владимир «14T достал, глючит» + HyperOS блокирует install debug
(INSTALL_FAILED_USER_RESTRICTED, обход -f не сработал).

## Файлы (v2, сняты 13:38–14:04)
- 01_home_ring_a52_v2.png / _en.png — главный, «0 мл / 2650 мл», кнопки 250/500/750.
- 03_daily_norm_a52_v2.png / _en.png — Настройки: «2750 мл из 2650 мл», «Вес: 70 кг»
  (EN: «Weight: 70 kg»).
- 04_quick_add_250_500_a52_v2.png / _en.png — главный с «500 мл · 19%».
- 05_drink_diary_a52_v2.png / _en.png — История/History с записью «500 мл».
Флоу каждой локали: pm clear → онбординг (метрические, вес 70) → Отмена на
уведомлениях → кадр 01 → 750×3 + 250×2 (до 100%) → кадр 03 по пути →
затем второй цикл pm clear → 500 мл → кадры 04/05. EN — переключением
СИСТЕМНОЙ локали (Settings UI), после съёмки система возвращена на RU.

## ⚠ DEFECT: kadr2 (цель достигнута) НЕ СНЯТ
Оверлей награды падает: «_RewardOverlayState is a SingleTickerProviderStateMixin
but multiple tickers were created» — красный экран, стабильно RU+EN.
Евиденс: DEFECT_02_reward_ticker_crash_ru.png / _en.png. В release-keepfix 2028
(съёмка 27.10) работал — регресс/проявление в debug. Репорт Р-2 в MAILBOX
29.10 14:10 → жду фикс → пересниму kadr2 RU+EN за 5 минут.

_Р-3, 29.10.2026. Session ID: [СКРЫТО:ID]_
