# Huawei AppGallery — регистрация (делает Владимир, можно СЕЙЧАС)

> Статус: октябрь 2026, НЕ начато. Контур: РФ (ИП Павлова В.Е.), из Москвы
> обычным интернетом, без прокси. В грузинский контур Huawei НЕ входит.
> ⚡ Обновлено 15.10 (решение Владимира + оппонент К3-Макс): первым идёт
> **Cleaner** (не Habits!), очередность Cleaner → QR → H₂O → Habits.
> v1 — бесплатная полная версия без IAP (Тема 1, DECISIONS 15.10): регистрация
> и витрина выплат НЕ требуют. Аккаунт = канал дистрибуции. Дублировать на
> грузинский ИП НЕЛЬЗЯ (бан за дубли). Вопрос про выплаты — до гейта IAP 500/90.

## Пошагово (1 вечер)
1. **Завести Huawei ID** на [СКРЫТО:EMAIL] (на HP, обычный браузер).
2. **developer.huawei.com** → AppGallery Connect → регистрация разработчика:
   - Тип: частный разработчик / организация — ИП РФ (Павлова В.Е., ИНН из
     свидетельства РФ-ИП, паспорт).
   - Страна: **Россия** (не меняется потом! платежи Мир/СБП живут в РФ).
   - Оплата разработчика: Huawei взимает $0 сейчас (проверить при регистрации).
3. **Создать приложение**: NOVAX **Cleaner** первым (очередность: Cleaner →
   QR → H₂O → Habits; Habits — только после фикса уведомлений по чек-листу Z).
   Тексты карточки — по TASK_004 (бесплатная v1, формула «Полностью бесплатно»),
   НЕ копировать RuStore-ASO с триалом/ценой.
   - Huawei-название: до 64 симв. — главный ASO-фактор, влезает поисковый хвост
     (напр. «NOVAX Cleaner — очистка памяти телефона от мусора и кэша»).
   - Короткое описание: 80 знаков. Обязательна ссылка на privacy policy.
   - Модерация первого релиза: до 5 дней.
4. **Скриншоты/иконка**: отдельный комплект без кадров пейвола/цен (см. TASK_004,
   заказ дизайнеру). Разрешения скринов Huawei: 450–1600 px, соотношение 9:16 ок.

## Открытый вопрос (задать в Huawei support ДО настройки выплат)
> Выплачиваете ли вы доход российским разработчикам на российские банки
> (SWIFT/SEPA сейчас недоступны)? Если нет — какие варианты: банк в Грузии,
> Payoneer, корсчёт?
- Канал: support в AppGallery Connect / [СКРЫТО:EMAIL].
- Запасной путь (из плана): тот же грузинский счёт Credo.

## Монетизация (ПОЗЖЕ — только после гейта 500 установок/90 дней)
- HMS IAP Kit, товар `novax_cleaner_full_access`, цена ~159 ₽ (рублёвая
  витрина через Мир/СБП/мобильный счёт). Ценник к гейту пересверить.
- Код (huawei flavor) готовит Рома-2; перед сборкой huawei-release он
  попросит `agconnect-services.json` из кабинета (СЕКРЕТ — не гит).
- Grandfathering: маркер первого запуска (Тема 1) — «ранние» получают
  lifetime бесплатно, анонс в changelog.

## Красные линии
- Huawei-аккаунт живёт на HP обычной жизнью, в грузинские профили не лезет.
- Пароли/ключи кабинета — только в менеджере Владимира, никуда не пересылать.

## Обновление 20.10: ответ Huawei получен — вопрос переезжает в тикеты

Письмо на [СКРЫТО:EMAIL] (18.10) → ответ 19.10: «денежные вопросы —
через Support Desk: https://developer.huawei.com/consumer/en/support/feedback/#/».
Это стандартная процедура, не отказ. План:
1. Завести Huawei ID на [СКРЫТО:EMAIL] (не обязывает).
2. Подать тикет с текстом ниже (подготовлен).
3. Параллельно — регистрация разработчика РФ-контур (выплаты не блокер
   бесплатной v1; решение Темы 2).

### Текст тикета (копировать в форму обратной связи)
Category: Monetization / Payments
Subject: App revenue payouts — Russian developer with Georgian business account

I am an individual developer preparing to publish a free app on AppGallery
(HMS IAP Kit planned later). Before registration I must confirm payout
options, because the account country cannot be changed after registration
and SWIFT to Russian banks is unavailable.

Situation: Russian citizen and sole proprietor in Russia; I also hold a
sole proprietorship in Georgia with a business account (Credo Bank, Tbilisi).

Questions:
1. Does Huawei pay app revenue to Russian developers' bank accounts in
   Russia? If yes — which methods (RUB local transfer, MIR, card payout)?
2. Can payouts go to a Georgian business bank account if the developer
   account country is Russia? What documents are required?
3. Should I instead register the developer account under the Georgian
   entity? Pros and cons?

The payout method determines my account setup. A detailed answer would be
much appreciated.

Account email: [СКРЫТО:EMAIL]

## ПАУЗА 19.10 (решение Владимира): ждём DUNS, регистрируемся как Предприятие

Новый факт в процессе: Huawei **блокирует новые регистрации ID из РФ**
(геоблок по IP — санкционная политика). VPN пробил блок, но ID создан только
с регионом Грузия (телефон+страна должны совпадать, иначе SMS-шлюз отказывает).
Вывод Владимира: если из РФ «не дают» — не зря; регистрируемся «как положено»:
ждём DUNS (~01.11, грузинское ИП) → тип «Предприятие» → **грузинский контур
Huawei** (единый с Samsung/GP; выплаты на Credo без тикетов).
Статус: на паузе до DUNS + короткого вердикта К3-Макса (факт геоблока
отправлен, переворачивает ли он Тему 2). ID Huawei (Грузия) создан и ждёт —
ничего необратимого не сделано.

## РЕШЕНО 19.10 вечер (К3+Владимир): ГРУЗИНСКИЙ контур, тип «Предприятие», после DUNS

### План
1. ⏳ DUNS (~01.11; задержка >2 недель → пинок D&B через Interfax-Kazakhstan)
2. 📄 Документы Предприятия готовить ЗАРАНЕЕ: свидетельство ИП Грузия,
   паспорт, данные Credo (для выплат) — всё в KeePassXC/конверте
3. 📨 ТИКЕТ Support Desk — можно подать УЖЕ СЕЙЧАС (Huawei ID создан):
   текст ниже. Ответ придёт ДО DUNS → закроет последний вопрос
4. После DUNS: регистрация «Предприятие» → имя разработчика NOVAX →
   верификация (1–3 дня) → карточка Cleaner (тексты готовы: TASK_004)

### Текст тикета (переписан под новый контур, 19.10)
Category: Monetization / Payments
Subject: Payouts & user payments — Georgian sole proprietorship (Enterprise)

I am registering as an Enterprise developer (Georgian sole proprietorship,
DUNS pending). Before completing setup I need to confirm:
1. Can app revenue be paid to a business bank account in Georgia
   (Credo Bank, Tbilisi)? Which payout methods are supported for Georgia?
2. Will users in Russia be able to purchase in-app items (MIR / SBP /
   mobile billing) if the developer entity is Georgian? Any restrictions?
3. Is there a list of countries where paid apps/IAP are unavailable for
   Georgian-registered developers?
4. What documents are required for Enterprise verification of a Georgian
   sole proprietorship (certificate of registration, DUNS)?
Account email: [СКРЫТО:EMAIL]
