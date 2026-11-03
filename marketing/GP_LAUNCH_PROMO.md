# NOVAX — Промо-план глобального запуска Google Play (декабрь 2026)

> Дата: 2026-10-09 · Автор: К3
> Контекст: GP-копии готовы (Flutter 3.47/SDK 36, in_app_purchase, QA пройден), порядок QR → H₂O → Habits, Cleaner позже. Аккаунт — организация (ИП Грузия, DUNS до ~01.11). Цена $2.99, 12 локалей, EN-приоритет.

---

## 0. Ключевой факт, который меняет план

**Аккаунт организации (с верифицированным D-U-N-S) освобождён от правила «12 тестеров × 14 дней»** — оно действует только для личных аккаунтов, созданных после 13.11.2023. Наша Play Console — org-аккаунт на ИП Грузии, значит закрытый тест как обязательный гейт не нужен.

Но: тестеров всё равно стоит собрать — не для гейта, а для (1) ловли багов до публичных отзывов, (2) первых честных оценок. Детали — §3.

Риск на контроле: если Google при верификации орг-аккаунта всё же покажет карточку «Closed testing required» (бывают исключения) — источник истины только дашборд Play Console. Тогда запускаем закрытый трек и набираем 14–15 тестеров (буфер против выбывших) из знакомых + подписчиков ТГ; сдвиг запуска +2–3 недели, это закладываем в сроки.

---

## 1. EN-ASO черновики (на основе RuStore-версий, адаптированы)

Правила EN-карточек: цена в долларах ($2.99 one-time), «free trial 7 days», никаких упоминаний RuStore, рублей и количества языков. GP title ≤30 символов, short ≤80.

### 1.1. NOVAX QR (первый в запуске)

**Title (30):**
```
NOVAX QR — Scanner & Maker
```
(29 символов)

**Short description (80):**
```
Fast QR & barcode scanner. No ads, no subscriptions. $2.99 once, forever.
```
(79 символов)

**Full description:**
```
NOVAX QR is a fast QR and barcode scanner for Android. No ads, no subscriptions, no data collection.

Tired of scanners that show a full-screen ad before every scan? NOVAX QR opens instantly and just works — everything is processed on your device.

Features:
• Instant QR and barcode scanning with the camera
• Recognize codes from gallery photos and screenshots
• Create QR codes: links, Wi-Fi, contacts, text
• Scan history, stored locally
• Light and dark theme
• Works fully offline

Pricing:
7-day free trial, then $2.99 one-time. No subscriptions, ever.

Why NOVAX QR:
🛡️ Scanning happens on your device
🛡️ No personal data collection
🛡️ No ads, no trackers
🛡️ Pay once — use forever

Your data never leaves your phone. No cloud, no analytics.
```

**Keywords (для поля тегов/описания GP):** qr scanner, barcode scanner, qr code reader, qr generator, wifi qr code, offline qr scanner, no ads qr scanner, scan qr from screenshot

### 1.2. NOVAX H₂O

**Title (30):**
```
NOVAX H2O — Water Tracker
```
(25 символов; в EN используем «H2O» обычной двойкой — поисковая строка)

**Short description (80):**
```
Water tracker with gentle reminders. No ads, no subscriptions. $2.99 once.
```
(76 символов)

**Full description:**
```
NOVAX H2O is a simple water tracker. No subscriptions, no ads, no data collection.

Forget to drink water? H2O calculates your personal daily goal and reminds you gently — with respect for Do Not Disturb.

Features:
• Personal daily water goal
• One-tap portions (+250 / +500 ml)
• History and progress by day and week
• Smart reminders that respect Do Not Disturb
• Coffee, tea and other drinks tracking
• Dark theme

Pricing:
7-day free trial, then $2.99 one-time. No subscriptions, ever.

Your data stays on your device. No ads, no analytics, no cloud.
```

**Keywords:** water tracker, drink water reminder, hydration app, water reminder, daily water intake, no subscription water tracker, offline water tracker

### 1.3. NOVAX Habits

**Title (30):**
```
NOVAX Habits — Daily Tracker
```
(28 символов)

**Short description (80):**
```
Build habits that stick. Streaks & reminders. No subscriptions. $2.99 once.
```
(78 символов)

**Full description:**
```
NOVAX Habits is a simple habit tracker. No ads, no subscriptions, no data collection.

Motivation lasts two days — systems last forever. Pick ONE habit, set a reminder at your time, and watch your streak grow. Breaking a streak hurts more than any motivation quote.

Features:
• Create personal habits and check them off daily
• Multiple reminders: morning, afternoon, evening or custom
• Streak counter — see your chain and protect it
• Choose active weekdays per habit
• Multi-completion habits (e.g. "drink water" 3× a day)

Pricing:
7-day free trial, then $2.99 one-time. No subscriptions, ever.

Your data stays on your device. No cloud, no analytics.
```

**Keywords:** habit tracker, build habits, daily habit tracker, streak counter, habit reminders, no subscription habit tracker, offline habits

> Примечание: в GP-текстах Habits триал указан 7 дней (как в актуальной версии +12/13). Если для GP решим вернуть 30 — правится в одном месте, перед заливкой сверить с продуктом.

---

## 2. Стратегия первых отзывов (100% легально)

Что можно и что нельзя (политика GP жёстче RuStore: за incentivized reviews — снятие приложения):

✅ **Можно:**
1. **Тестеры закрытого трека.** Даже без обязательного гейта создаём closed track: 15–20 человек (знакомые, коллеги Стальстройпроекта с Android, подписчики ТГ). Просим: «попользуйся неделю и оставь ЧЕСТНЫЙ отзыв — любой, какой считаешь верным». Просить отзыв легально; просить 5★ или платить за него — нет.
2. **Личные сообщения после релиза:** тёплый круг, шаблон ниже. Просим фидбек, а не оценку.
3. **In-app мягкий rate-prompt** (если есть/появится — задача Роме в бэклог): после 5–7 дней использования, один раз, с кнопкой «не сейчас».
4. **Пост в ТГ в день релиза** с просьбой подписчиков поставить и написать отзыв.

❌ **Нельзя (никогда):** покупать отзывы, обменивать отзывы с другими разработчиками («review exchange»), просить именно 5 звёзд, давать бонусы за отзыв, писать отзывы самому с других аккаунтов.

**Шаблон тёплым контактам (EN-релиз):**
```
Привет! Я запустил своё приложение в Google Play — сканер QR без рекламы и подписок. Если не сложно: поставь, попользуйся пару дней и оставь честный отзыв в сторе — любой, хоть три звезды, мне правда важна честность, а не оценка. Ссылка: [GP link]
```

Цель: **10–15 честных отзывов на QR за первые 2 недели** после релиза. Для нового приложения в GP это перелом пустой карточки.

---

## 3. Лендинг `/google-play-launch/` (структура, вёрстка — Рома)

Отдельная EN-страница на novaxhub.ru, задеплоить к релизу QR:

1. **Hero:** «NOVAX is now on Google Play» + подзаголовок «Simple Android apps. No ads. No subscriptions. Pay once — $2.99.» + кнопка Google Play badge.
2. **Блок из 3 приложений** (QR, H2O, Habits): иконка, одна фраза пользы, кнопка стора.
3. **«Why one-time payment?»** — 3 предложения про усталость от подписок (тот же нерв, что в RU-версии).
4. **Trust-блок:** «Data stays on your device. No analytics, no cloud.» + ссылка на Privacy Policy (к релизу нужна EN privacy policy на сайте — требование GP, задача Роме; без неё карточку не опубликовать!).
5. **FAQ (EN):** Is it really one-time? / Does it work offline? / What data do you collect? (ответ: none).
6. **JSON-LD:** SoftwareApplication × 3, `offers` price 2.99 USD.
7. **Связка:** с главной — баннер «Now on Google Play», обратно — ссылка с лендинга на главную.

Важно: страница — единственная «внешняя» ссылка, которую мы даём в EN-сообществах (вместо прямой ссылки на стор, где это запрещено — можно дать сайт).

---

## 4. EN-публикации (только где разрешено, без спама)

**Жёсткое правило:** сначала читаем правила саба/площадки в день постинга (они меняются), постим только в разрешённых местах, аккаунт reddit должен быть «прогрет» — несколько недель обычных комментариев до первого промо-поста. Новый аккаунт с единственным постом-ссылкой = бан и сожжённая тема.

| Площадка | Как постим | Когда |
|---|---|---|
| **r/androidapps** | Только еженедельный promo/app-threads (смотреть актуальные правила в сайдбаре в день поста; исторически самопромо разрешён в weekly thread). Текст: 3 предложения, честно про цену | Неделя релиза QR |
| **r/android** | Прямой промо запрещён — НЕ постим. Можно участвовать в обсуждениях | — |
| **Product Hunt** | Запуск «NOVAX QR» как продукта: maker = Владимир, честное описание, 4–5 скриншотов. Это легальная площадка для запусков | Неделя 2 после релиза QR (когда есть ≥5 отзывов) |
| **AlternativeTo** | Добавить приложения как альтернативу платным трекерам/сканерам с пометкой «one-time payment, no ads» | Неделя 2–3 |
| **EN-Telegram каталоги/каналы** | Только по запросу админов, без холодного спама | Опционально |

**Текст для weekly thread r/androidapps (черновик):**
```
NOVAX QR — a QR & barcode scanner that shows no ads and asks for no subscription. 7-day free trial, then $2.99 once. Everything is processed on-device: scanning, history, QR generation (Wi-Fi, links, contacts). Works fully offline. I built it because I was tired of scanners with full-screen ads. Feedback welcome — good or harsh.
```

---

## 5. Таймлайн декабря

| Дата (план) | Действие | Кто |
|---|---|---|
| до 01.12 | DUNS получен, Play Console org-аккаунт создан и верифицирован | Владимир |
| 01–05.12 | Карточка QR в консоли: EN-ASO из §1.1, скриншоты EN (съёмка по ТЗ из ASO-аудита), privacy policy EN на сайте | К3 тексты, Владимир заливка (20 мин) |
| 05.12 | QR в production review | Владимир (5 мин) |
| Релиз QR | Анонс ТГ/MAX (пост готовлю за день) + weekly thread reddit + сообщения тёплым | К3 + Владимир (20 мин) |
| +7 дней | Product Hunt, AlternativeTo | Владимир (15 мин) |
| +10–14 дней | H₂O в GP по тому же конвейеру | то же |
| конец декабря | Habits в GP | то же |

---

## 6. Что меряем в GP (дополнение к METRICS.md)

- Установки/день первые 14 дней (QR — эталон для остальных).
- CR карточки GP (Store listing visitors → installers) — сравнить с RuStore (у нас ~0,8% для QR — если GP покажет выше, это сигнал про платёжеспособную аудиторию).
- Отзывы: цель 10–15 за 2 недели.
- Первая продажа $2.99 — отдельная веха, фиксируем дату и источник (если видно).

---

*Самопроверка К3: 9/10. EN-тексты адаптированы, а не переведены дословно (валюта, оффер, «H2O» без подстрочной для поиска); освобождение org-аккаунта от 12-тестерного гейта подтверждено по свежим источникам, но заложен fallback; стратегия отзывов полностью в рамках политики GP. Минус балл: правила сабреддитов живые — финальная сверка обязательна в день постинга, это записано в план.*
