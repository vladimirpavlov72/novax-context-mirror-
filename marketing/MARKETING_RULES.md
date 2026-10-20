# NOVAX Apps — Marketing & Promotion Rules

## Goal

Drive organic installs for NOVAX Cleaner, QR Scanner, and Hydrolog/Drinkler on the Russian market through honest, useful content and a lightweight promo site.

## Core Principles

1. **No ads anywhere.** NOVAX apps contain no advertising — including ads for our own apps. Cross-promo banners, "Other apps" sections, onboarding widgets for other NOVAX products, and paid placements inside apps are prohibited.
2. **Privacy first.** No analytics, no crash reporting, no external data transmission.
3. **Honest marketing.** No fake numbers, fake reviews, or fake urgency.

## Core Channels (Russian Market)

1. **Search engines** — Yandex first, Google second.
2. **Recommendation platforms** — Yandex Zen, VK Clips/Articles.
3. **Niche blogs** — VC.ru (business/product), Habr (tech/IT audience if relevant).
4. **App stores** — RuStore catalog optimization, later Samsung/GetApps.
5. **Own promo site** — landing pages for each app with direct store links.

## Promo Site Requirements

### Must-Have Technical Files

- `robots.txt` — allow indexing, allow AI bots (GPTBot, Google-Extended, ClaudeBot).
- `sitemap.xml` — list all landing pages.
- RSS feed (optional but useful for Zen/agregators) — `/feed.xml` or `/rss.xml`.

### Must-Have Meta Tags per Page

- `<title>` — up to 70 symbols, contains main keyword.
- `<meta name="description">` — up to 160 symbols, clear value proposition.
- No `noindex` on public pages.

### Schema.org Markup

- Use `SoftwareApplication` for each app page:
  - `name`, `applicationCategory`, `offers` (price, priceCurrency), `aggregateRating` (when reviews exist), `operatingSystem`.
- Use `FAQPage` for the FAQ section.
- Use `Article` / `BlogPosting` for blog posts.

### Content Structure for Blog Posts

- One `<h1>` per page with main keyword.
- `<h2>` for sections, `<h3>` / `<h4>` for subsections and lists.
- Short paragraphs (3–4 sentences max).
- Bulleted/numbered lists for scanability.
- `<strong>` for key takeaways, but do not overuse.
- Every image must have descriptive `alt` text with a keyword where natural.
- Use `.webp` images, keep page load under 2 seconds.

## Article Writing Checklist

1. **Topic** solves a real user problem:
   - Cleaner: "How to free up storage on Android without deleting photos".
   - QR Scanner: "How to create a QR code for Wi-Fi guests".
   - Hydrolog: "How much water should you drink per day".
2. **Title (H1)**: clear, contains keyword, no clickbait.
3. **Lead paragraph**: answer the user's intent in 2–3 sentences.
4. **Body**: step-by-step instructions, pros/cons, screenshots.
5. **CTA**: link to RuStore / direct APK download at the end.
6. **Meta**: custom title + description for the article page.

## Platform-Specific Notes

### Yandex Zen / Dzen

- Post native articles, not just links.
- First 2–3 sentences are critical for CTR.
- Use cover image 1200×800.
- Add link to app in the first comment and inside text.

### VC.ru

- Storytelling + real numbers + product story works best.
- Mention RuStore link in the end.
- Avoid hard sell; focus on problem + solution.

### Habr

- Only if article has technical depth (e.g., "How we built an honest cleaner on Flutter without fake cache counters").
- No direct ads; community dislikes promo.

### RuStore Catalog

- Optimize app title, short description, full description.
- Use keywords: "очистка памяти", "удаление мусора Android", "сканер QR", "трекер воды".
- Add screenshots in Russian.

## What to Avoid

- Fake storage numbers, fake reviews, fake urgency.
- Keyword stuffing.
- Long walls of text.
- Broken links or missing privacy policy.
- Cross-promo or in-app ads of any kind, including promotion of other NOVAX apps inside NOVAX apps.

## Next Marketing Tasks

1. Maintain the promo site on the owned VPS (`novaxhub.ru` on `[СКРЫТО:IP]`).
2. Write 3–5 seed articles (one per app + one comparative/honest-cleaner story).
3. Set up Yandex.Webmaster and Yandex.Metrica.
4. Create Dzen channel and republish articles.
5. Prepare RuStore ASO texts and screenshots.

## RuStore Screenshot Rules (добавлено 2026-09-12)

1. **Пропорции 9:16.** Скриншоты для телефонов обязаны быть в пропорциях 9:16.
2. **Не ужимать, а обрезать.** Скрин не должен быть сжат/искажён — обрезается красиво (центральная композиция), так чтобы основная информация оставалась на кадре.
3. **Фон по приложению:** светлый для всех приложений, кроме Cleaner (тёмный).
4. Скрины делаем на Xiaomi 14T Pro (Android 15 — красивее), QA/логика — на Samsung A52.
