# NOVAX QR v2.0.21 — Техзадание для Ромы (v1, 26.07.2026)

**От:** Тимлид (Кими) | **Проект:** `C:\dev\apps\novax-qr-scanner` | **Репо:** vladimirpavlov72/novax-qr-scanner
**SCOPE ЗАМОРОЖЕН.** Всё, чего нет в этом ТЗ — в backlog, не в эту версию (анти-паттерн №5).

---

## Цель

Первый публичный релиз QR в RuStore: **freemium** — trial 7 дней (все функции) → разовая покупка 159 ₽ (`novax_qr_full_access`), плюс генерация QR и ребрендинг в **NOVAX QR**. Модель v2.0.20 (paid-only) отменена решением 26.07 (DECISIONS.md).

## Железные рамки

- AI_RULES.md целиком + раздел 12 (автономность). Перед сборкой — Z-ревью.
- Стек: Flutter 3.24.0, Dart 3.5.0, AGP 8.1.0, Gradle 8.0.2, Kotlin 1.8.22, Java 17 — как в Cleaner v2.0.2.
- Архитектура: StatefulWidget + ValueNotifier + SharedPreferences. Сервисы — синглтоны. NO BLoC/Provider/Firebase/go_router.
- Локализация: 12 ARB, golden pattern (`synthetic-package: false`, `nullable-getter: true`), NO `generate: true`, NO хардкод-строк, EdgeInsetsDirectional.
- `flutter_rustore_billing: ^0.0.5` (проверенная в Cleaner версия, НЕ новее).
- pub.dev заблокирован → всё через `C:\dev\vendor\packages\`.

---

## Шаг 0. Vendor

- Завендорить `qr_flutter` (стабильная 4.1.0, совместимая с Dart 3.5) через vendor-процесс.
- `mobile_scanner` НЕ вендорить — см. решение в шаге 1.

## Шаг 1. Сканер остаётся на qr_code_scanner (решение тимлида)

- В v2.0.21 миграции на mobile_scanner НЕТ. Причина: сканер — core-фича, пересадка в перегруженном релизе = риск срыва. Миграция — отдельная задача v2.0.22 с полным Z-ревью.
- Сделать постоянное решение KNOWN_ISSUES #2: пропатченный `qr_code_scanner` (namespace, Java 17) закэшировать в `C:\dev\vendor\packages\` и подключать локальным path, чтобы `pub get` больше не ломал сборку.
- Добавить запись в DECISIONS.md:

```markdown
## 2026-07-26 — QR v2.0.21: сканер без миграции, патч в vendor

**Решение:** v2.0.21 остаётся на qr_code_scanner; пропатченная версия закэширована в vendor как постоянное решение. Миграция на mobile_scanner — задача v2.0.22.

**Почему:** scope freeze; пересадка core-фичи в перегруженном релизе = риск. Vendor-кэш снимает ежедневную боль ручных правок pub cache.
```

## Шаг 2. Trial + billing (реюз, не с нуля)

- **База 1:** git-тег `v2.0.19-trial` — там готовые TrialState, TrialService, PaywallScreen (Kaneman ~~$9.99~~ → $2.99/159₽) и 15 локализованных ключей. Поднять и адаптировать.
- **База 2:** Cleaner v2.0.2 `PremiumService` — паттерн реального billing: `_initBilling()`, `_billingInitialized` guard, graceful-ошибки с карточкой Retry, trial живёт при недоступном billing.
- Product ID: `novax_qr_full_access`, 159 ₽, trial 7 дней со всеми функциями.
- Deeplink схема в AndroidManifest: `novaxqr://iamback` (по образцу `novaxcleaner://iamback`).
- ProGuard `-dontwarn` правила — из Cleaner.
- Гейтинг: истёкший trial → paywall на сканер И генератор (обе фичи premium после trial).

## Шаг 3. Экран генерации QR

- Источник: `qr_scanner/z_upgrade/CREATE_QR_SCREEN.dart` + `QR_UPGRADE_NOTES.md` (получены от тимлида 22.07).
- Адаптация под NOVAX-правила: все строки в ARB, EdgeInsetsDirectional, сервисы-синглтоны, ноль хардкода.
- Типы: текст, URL, Wi-Fi, контакт (vCard).
- Превью QR + кнопки «Поделиться» / «Сохранить». Разрешения — минимально необходимые (Privacy First), обосновать каждое в коде комментарием.
- Сгенерированные коды — в историю (по QR_UPGRADE_NOTES; если тип записи новый — immutable model с fromJson/toJson, как ScanRecord).
- Корзина (TrashScreen, soft delete) должна работать и для сгенерированных — правило №5.

## Шаг 4. Ребрендинг + навигация

- Имя приложения: **NOVAX QR** (app_name, заголовки, About).
- BottomNavigationBar, 4 таба: **[Сканировать] [Создать QR] [История] [Настройки]**.
- Иконка Саши — финальная: проверить mipmaps и store-версию 512×512 PNG.
- Убрать следы «paid-only» из текстов (About/EULA/Settings).

## Шаг 5. Локализация

- Новые ключи (генерация, trial, paywall, ребрендинг) во все 12 ARB. Часть ключей поднять из `v2.0.19-trial`.
- `flutter gen-l10n`, прогнать переключение всех языков на девайсе (был баг 19.07 — не повторить).
- RTL: проверить арабский на экране генерации.

## Шаг 6. Privacy Policy / EULA

- Текст о монетизации вернуть на freemium: «7 дней бесплатно, затем разовая покупка через RuStore; обработка платежа — магазином». (В v2.0.20 текст переписывался под paid-only.)
- Обновить дату политики.

## Шаг 7. Версия и сборочный скрипт

- `pubspec.yaml`: `version: 2.0.21+1`.
- Адаптировать `build_release.sh` из Cleaner под QR (gen-l10n → clean → build → sign → verify → copy), включая авто-обновление версии в main.dart и settings_screen.

## Шаг 8. Z-ревью — СТРОГО перед сборкой

- Подготовить топ-10 файлов как .txt + короткий промпт → отдать Владимиру (он гоняет chat.z.ai / GLM-5.2).
- Применить правки Z → только потом сборка. Глобальные правки без Z-ревью = брак по определению.

## Шаг 9. Сборка, установка, QA

- `bash build_release.sh` → подпись → установка на Xiaomi 2407FPN8EG.
- Отдать Владимиру QA-чеклист:
  1. Trial стартует на 7 дней после EULA, счётчик дней на Home
  2. Истёкший trial → paywall (debug reset в Settings)
  3. Генерация всех 4 типов QR, Поделиться/Сохранить
  4. История: сканы + сгенерированные, корзина (удалить → восстановить)
  5. Смена языка на лету, минимум RU/EN/AR
  6. Офлайн-режим полностью рабочий
  7. Billing-ошибка (без интернета) → карточка Retry, не stack trace

---

## Отчётность (правило автономности)

- Commit + push после каждого шага. Keystore/key.properties — никогда в git.
- После каждого шага — короткий отчёт Владимиру: что сделал / что изменилось / что дальше.
- **Стоп-точки (эскалация к тимлиду через Владимира):** qr_flutter не вендорится; CREATE_QR_SCREEN требует переписи >50%; конфликт flutter_rustore_billing с minSdk проекта (в v2.0.19 его убрали из-за minSdk 24 — проверь текущий minSdk первым делом!). Застрял >3 попыток — стоп, варианты наверх.

## НЕ входит в scope (backlog v2.0.22+)

- Миграция на mobile_scanner
- Сканирование QR из картинки/галереи
- Google Play billing
- Кастомизация дизайна QR (цвета, логотип в центре)
