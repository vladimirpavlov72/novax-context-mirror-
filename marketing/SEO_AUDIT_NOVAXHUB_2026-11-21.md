# SEO-аудит novaxhub.app — 21.11.2026 (SEOmator 8.0.1, безопасный, см. ревью)

> Исполнитель: Рома-маркетинг. Инструмент: seomator (crawl, 5 страниц по
> внутренним ссылкам). Полный отчёт: `marketing/seo_audit_novaxhub_2026-11-21.html`.
> Оценка: **92/100 (A)**. Категории-лидеры: Technical SEO 100%, i18n 100%,
> Mobile 100%, Core 99%. Слабые: Security 70%, Social 75%, AI/GEO 77%.

## 🔴 КРИТИЧНО — мёртвый домен в фундаменте (причина: переезд с Beget)

1. **sitemap.xml: 21 из 24 URL указывают на novaxhub.ru** (домен мёртв —
   сервера после атаки нет). Ошибка `crawl-sitemap-domain`. ФИКС: перебить все
   loc на novaxhub.app. Зона: Р-5 (сборка) / СА.
2. **Canonical-теги указывают на novaxhub.ru** (`core-canonical-loop`,
   `crawl-canonical-form-drift`). ФИКС: canonical каждой страницы → .app.
   Зона: Р-5.
3. **Сканируются всего 5 страниц** — RU-лендинги (telefon-tormozit и пр.)
   невидимы для краулера: внутренних ссылок с хаба на них нет, в sitemap они
   на мёртвом домене. ФИКС: после правки sitemap — внутренняя перелинковка
   с хаба. Зона: Р-5 + моя (тексты ссылок).

## 🟡 Моя зона (тексты/разметка) — беру сам

4. Meta description главной — **1142px** (обрежется в выдаче, лимит ~920px).
   Ужму до ~150 знаков.
5. Title /privacy-en — 22 знака (лимит 30–60). Расширю.
6. /privacy (RU) — **28 слов, thin content** (ошибка content-word-count).
   Расширю до нормального текста.
7. **llms.txt на .app говорит про RuStore и цены в ₽** — смесь контуров
   (GP = доллары, §19). Перепишу llms.txt под GP-контур.
8. twitter:card meta-тег — дам Р-5 готовый сниппет (summary_large_image +
   og уже есть).
9. Пустые href ×3 на главной (links-invalid) — укажу Р-5 места.
10. schema.org: privacy-en без JSON-LD; главной не хватает WebSite schema —
    подготовлю разметку, Р-5 вставит.

## 🟢 Инфра (ограничение GitHub Pages — не баг команды)

11. Security headers (HSTS, X-Frame-Options, CSP по HTTP) на Pages НЕ
    ставятся технически. Частично закрывается `<meta http-equiv="Content-Security-Policy">`
    (без frame-ancestors). При переходе фронта на Cloudflare — закроется
    полностью. Отмечено как известное ограничение.

## ✅ Что уже хорошо

- HTTPS, сертификат, robots.txt, sitemap.xml (сам файл жив), i18n/hreflang
  100%, мобильная вёрстка 100%, Legal Compliance 100%, HTML-валидность 100%.
- llms.txt и /llms/ существуют (GEO-готовность редкость для ниши) — но страницы
  на него не ссылаются (geo-llms-txt warning): добавить <link> в <head>.
- Не мерялось (нужен --render): Core Web Vitals — прогоню отдельно после
  установки chromium (npx playwright install chromium).

## Очередь

1. Р-5/СА: sitemap + canonical → .app (блокер №1, 10 минут работы).
2. Рома: пункты 4–10 (тексты и разметка) — сегодня.
3. Р-5: twitter:card, ссылка на llms.txt в head, пустые href.
4. Рома: повторный аудит + --render после правок (CWV).

---

## ВЫПОЛНЕНО 21.11 (виза Гиа — правки сайта сам, репо novaxhub.github.io)

- 8730bfd: canonical/sitemap/og/JSON-LD/ссылки ×195 — .ru → .app (весь сайт);
  description главной 157→135 зн.; twitter:card + link llms-txt в head;
  GP-плейсхолдеры href="#" ×3 → честные «Soon on Google Play»;
  контакт [СКРЫТО:EMAIL] → [СКРЫТО:EMAIL] ×3 (§19, на .app висел РФ-емейл!);
  llms.txt переписан под GP-контур (EN, без ₽/RuStore).
- 5b414cd: descriptions privacy/privacy-en/privacy-files/eula-files → ≤150 зн.;
  twitter:card + JSON-LD WebPage на все 4 легальные страницы.
- РЕЗУЛЬТАТ: 92 → 93/100. Категории: Crawlability 91→96, Social 75→78,
  Core 99→100. Ошибки sitemap-domain, canonical-loop/drift, twitter-card,
  пустые href — ЗАКРЫТЫ. Ошибки description-pixel-width — ЗАКРЫТЫ все 5 стр.

## ОСТАТОК (не наша зона/не наша вина)

1. Security headers (HSTS, X-Frame-Options, X-Content-Type-Options, redirect
   http→https 301 на http) — ограничение GitHub Pages. Частично лечится
   галкой «Enforce HTTPS» в настройках репо (зона СА/Владимир), полностью —
   при переходе фронта на Cloudflare.
2. content-word-count «28 слов» на 3 RU-страницах — ЛОЖНОЕ срабатывание:
   экстрактор правила считает в основном латиницу; фактически на страницах
   208–241 слово (privacy 220, privacy-files 208, eula-files 241). Google
   кириллицу считает. Фиксировать не требуется.
3. Не мерялось (нужен --render): Core Web Vitals — после playwright chromium.
