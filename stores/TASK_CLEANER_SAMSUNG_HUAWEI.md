# TASK: Cleaner — APK и выкладка в Samsung Galaxy Store + Huawei AppGallery

> Статус: выдано Роме-2 · Дата: 2026-10-15 · Приоритет: после Cleaner reminders
> Репо: `C:\dev\apps\novax-cleaner` · База: v2.1.2+2026 → целевые: `v2.3.0+2028-samsung`, `v2.3.0+2029-huawei`
> Контекст площадок: `stores/SAMSUNG_HUAWEI_PLAN.md` · Регистрация Huawei: `stores/TASK_HUAWEI_REGISTRATION.md`

---

## 1. Решение Владимира (фиксирую, отступления — только через меня)

**Не ждать GP. Samsung и Huawei — делаем сейчас, начинаем с Cleaner.**
Монетизация на старте: **бесплатная полная версия** (пейвол отключён через
feature-флаг, без покупок). Цель этапа — база установок и отзывы. IAP для
этих сторов — отдельный трек потом.

## 2. Что сделать (Рома-2)

1. **Feature-флаг «free full version»**: константа/SharedPreferences-флаг `altstore_free=true` → PremiumService считает premium активным, пейвол не показывается, RuStore billing не инициализируется. ВАЖНО: не ломать rustore-флейвор — делаем отдельными build-вариантами (`--dart-define=STORE=samsung/huawei`), rustore-сборка не меняется.
2. **Деконтаминация**: убрать RuStore deeplink/метаданные из samsung/huawei-вариантов (как делали для play-флейвора: dexdump-проверка, 0 RuStore-классов).
3. **Сборки**: release APK arm64 для обоих сторов. Версионирование: samsung +2028, huawei +2029 (годовые коды, как договаривались).
4. **QA на A52** по сокращённому чеклисту: чистка работает, пейвола нет, EULA есть, analyze 0 issues, logcat чистый.
5. **Витринные ассеты**: скрины 1080×1920 (можно переиспользовать RuStore-набор `Downloads\NOVAX_Cleaner_v2.1.2+2026\`), текст краткого описания (80 знаков) и полного — берётся из `cleaner/RUSTORE_ASO_COMPLETE.md`, адаптировать под EN-дефолт каждого стора. Владимир сам вставляет в консоли сторов.

## 3. Блокеры и кто снимает (НЕ Рома-2)

| Блокер | Кто | Статус |
|---|---|---|
| Samsung Seller Portal требует **DUNS** (Commercial Seller) | Владимир | Ждём DUNS (~01.11). APK готовим заранее, регистрация после. Грузия — «агентская» страна, 80/20 |
| Huawei Developer: вопрос о выплатах на российские банки | Владимир | Задать в Huawei support ДО регистрации (`TASK_HUAWEI_REGISTRATION.md`). Сама регистрация — сразу, DUNS не нужен |
| Регионы продвижения | К3-Высокий | Стратегия отдельным файлом, см. план у Ромы-Грузия |

## 4. Definition of Done

1. Два QA'd APK в `C:\dev\releases\` + копии в `Downloads\NOVAX_Cleaner_Samsung_v2.3.0+2028\` и `...\Huawei_v2.3.0+2029\` (APK + описание + скрины).
2. dexdump: 0 RuStore-классов в обеих сборках.
3. `cleaner/CONTEXT_SHORT.md` обновлён, коммит + push в novax-cleaner: `v2.3.0: altstore builds (samsung/huawei, free full, paywall off)`.
4. Отчёт Роме-Грузии. В консоли сторов Владимир публикует сам (или по нашей инструкции с грузинского IP).

---

*Бриф составлен Ромой-Грузия по решению Владимира 15.10. Вопросы по монетизации/ценам — НЕ к исполнителю, ко мне.*
