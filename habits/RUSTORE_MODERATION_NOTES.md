# NOVAX Habits — Подготовка к модерации RuStore (для тимлида)

> Актуально для версии **v1.0.0+10**, commit `584ba32`.
> Дата: 2026-08-10

---

## 1. Идентификация приложения

| Параметр | Значение |
|----------|----------|
| Название в сторе | **NOVAX Habits** (RU: «Привычки +») |
| Package | `com.novax.habits` |
| Android label | `Habits +` / `Привычки +` |
| RuStore App ID | `2063741856` (вписан в `android/app/src/main/res/values/strings.xml`) |
| RuStore deeplink | `novaxhabits://iamback` |
| Product ID | `novax_habits_full_access` |
| Цена | 159 ₽ / $2.99, **разовая покупка**, **без подписок** |
| Триал | 30 дней бесплатно |
| Возрастной рейтинг | 3+ / E / Everyone |
| Категория RuStore | Здоровье и фитнес / Инструменты |

---

## 2. Что изменилось в v1.0.0+10

### Исправления в Settings → About
- **Язык** — работает выбор одного из 12 языков вручную.
- **Системный язык** — добавлена опция возврата к языку устройства.
- **Оценить приложение** — открывает страницу `https://www.rustore.ru/catalog/app/com.novax.habits`.
- **Поделиться** — открывает системный share-лист с текстом + ссылкой на RuStore.

### Техническая реализация
- Добавлен `LocaleService` (`SharedPreferences` + `ValueNotifier<Locale?>`).
- Добавлен `PlatformActionsService` + MethodChannel `com.novax.habits/platform`.
- Kotlin `MainActivity` обрабатывает `openUrl` и `shareText`.
- Без новых внешних зависимостей (`url_launcher` / `share_plus` не используются).

## 3. Что реализовано в v1.0.0+9 (базовый функционал)

### Core функционал
- **Onboarding** — 2 шага, объясняет 30-дневный триал и разовую покупку.
- **Home** — умная дата-шапка (Сегодня/Завтра/Вчера), недельные точки, фильтрация по дням недели (`frequencyDays`).
- **Привычки** — добавление, редактирование (long-press / карандаш), удаление свайпом влево с подтверждением (soft-delete).
- **Quick-start presets** — Вода, Тренировка, Чтение, Медитация, Прогулка.
- **Мульти-напоминания** — у любой привычки можно выбрать несколько таймингов: Утро 9:00 / День 13:00 / Вечер 19:00 (multi-select).
- **Мульти-выполнение** — на Home карточка показывает столько галок, сколько выбрано напоминаний; тап заполняет следующую.
- **Weekday frequency** — чипы Пн-Вс.
- **Paywall** — заголовок, буллеты, цена, CTA «Продолжить», restore, ошибочная карточка.
- **Settings** — тема (системная/светлая/тёмная), статус premium, включение/выключение уведомлений, About (язык, оценить, поделиться).
- **Уведомления** — локальные push по расписанию, учитывают `frequencyDays`, восстанавливаются при включении.

### Архитектура / ограничения
- **StatefulWidget + ValueNotifier + SharedPreferences**. Никаких BLoC/Riverpod/Provider/GetX/Firebase/go_router.
- **12 ARB локалей**: en, ru, ar, de, es, fr, hi, ja, ko, pt, tr, zh.
- **Offline-first**: привычки и логи хранятся локально. Интернет используется **только** для RuStore-платежей.
- **Privacy First**: нет рекламы, нет сетевой аналитики, нет сбора персональных данных.

### Монетизация
- 30-дневный триал с момента первого запуска.
- После триала — блокировка добавления новых привычек и paywall.
- One-time purchase `novax_habits_full_access` через RuStore Pay SDK 10.3.1.
- Restore purchases поддерживается.
- Анти-тампер: если пользователь переводит время назад ≥2 раза, триал отзывается.

### Технические детали
- **compileSdk/targetSdk**: 34, **minSdk**: 21.
- **Kotlin warning**: binary metadata 1.9.0 vs 1.7.1 — **не блокирует сборку**, APK собирается и подписывается.
- **Подпись**: `C:/dev/keystores/novax_release.jks`, alias `novax_key`.
- **Release APK**: `C:\dev\releases\novax_habits_v1.0.0+10_arm64.apk` (20.7 MB).

### Разрешения (AndroidManifest)
- `INTERNET` — только RuStore платежи.
- `POST_NOTIFICATIONS` (Android 13+).
- `RECEIVE_BOOT_COMPLETED`, `SCHEDULE_EXACT_ALARM`, `USE_EXACT_ALARM` — для напоминаний.

---

## 3. Чего не хватает для модерации

| # | Документ / артефакт | Статус | Комментарий |
|---|---------------------|--------|-------------|
| 1 | **Адаптивная иконка** | 🟡 | PNG mipmaps есть, но adaptive XML ждёт векторный ассет от дизайнера. |
| 2 | **Скриншоты RuStore** | 🟡 | Есть скриншоты v1.0.0+9. Для v1.0.0+10 желательно обновить скрин Settings/About (теперь на English тоже). Индекс: `SCREENSHOTS_INDEX.md` в `C:\Users\Владимир\Downloads\Для тимлида`. |
| 3 | **Privacy Policy** | ✅ | Подготовлен `habits/PRIVACY_POLICY.md` (RU + EN). Нужно разместить на лендинге/сайте. |
| 4 | **EULA / Terms** | ✅ | Подготовлен `habits/TERMS_OF_USE.md` (RU + EN). Нужно разместить на лендинге/сайте. |
| 5 | **ASO-тексты карточки** | ✅ | Подготовлен `habits/RUSTORE_CARD_CONTENT.md` (RU). Локализации на 12 языков — в процессе, делаем in-house (Рома+Владимир). |
| 6 | **In-app товар в RuStore Console** | ✅ | Регистрация ИП и монетизация настроены. Товар `novax_habits_full_access`, 159 ₽, разовая покупка. |
| 7 | **Лендинг `/habits/` на novaxhub.ru** | 🟡 | Необязательно для модерации, но нужен для публикации Privacy Policy / Terms и ASO/GEO. |

---

## 4. QA-статус (v1.0.0+10)

- ✅ Сборка release APK: успешно.
- ✅ Подпись APK: успешно (Gradle signing).
- ✅ `flutter analyze --no-pub`: No issues found.
- ✅ Установка поверх RuStore-версии на Xiaomi 14 (`adb install -r`): успешно.
- ✅ Переключение языка RU → EN и сохранение выбора: успешно.
- ✅ «Оценить приложение» — открылась страница RuStore: успешно.
- ✅ «Поделиться» — открылся системный share-лист: успешно.
- 🟡 Ручной QA multi-reminder / multi-completion: назначен Владимиру на A52.
- 🟡 Sandbox-тесты RuStore: теперь возможны — товар создан, нужен тестовый платёж.

---

## 5. Чек-лист для тимлида (Кими)

- [x] Подготовить `habits/PRIVACY_POLICY.md` и разместить на сайте.
- [x] Подготовить общий EULA или Terms для линейки.
- [ ] Заказать/получить векторную adaptive иконку у дизайнера.
- [x] Подготовить ASO-тексты для RuStore (RU + EN, остальные — in-house, Рома+Владимир).
- [x] Получить чистые скриншоты с Xiaomi 14 после окончания звонка.
- [ ] Подготовить JPG 1080×1920 для RuStore (5–8 штук).
- [x] Создать товар `novax_habits_full_access` в RuStore Console.
- [ ] Провести sandbox-тесты: триал, покупка, restore, отмена.
- [ ] Создать/обновить лендинг `/habits/` на novaxhub.ru.

---

## 6. Блокеры

1. **Adaptive иконка** — дизайнер.
2. **Лендинг с Privacy Policy / Terms** — нужен публичный URL для карточки RuStore.
3. **Локализации ASO / Privacy / Terms** — делаем in-house (Рома+Владимир).

---

## 7. Контакты / артефакты

- Код: `C:\dev\apps\novax_habits`
- Release APK: `C:\dev\releases\novax_habits_v1.0.0+10_arm64.apk`
- APK для RuStore: `C:\dev\releases\novax_habits_v1.0.0+10_arm64.apk` (можно скопировать в `Для тимлида` при необходимости)
- Контекст: `C:\dev\novax-context-main\habits\`
- Стартовый файл: `C:\Users\Владимир\Desktop\NOVAX_Habits_checkpoint_2026-08-05.txt`
