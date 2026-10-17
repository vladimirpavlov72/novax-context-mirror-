# AI Rules — How Kimi Works on NOVAX Projects

> MANDATORY. Read before every session. Never violate.

---

## 1. Architecture Rules

### State Management
- **ONLY** ValueNotifier + ValueListenableBuilder + StatefulWidget
- **NEVER** BLoC, Riverpod, Provider, GetX, MobX, Redux
- Pattern: Service extends ValueNotifier → Screen uses ValueListenableBuilder

### Navigation
- **ONLY** `Navigator.push()` / `Navigator.pop()`
- **NEVER** go_router, auto_route

### Storage
- **ONLY** SharedPreferences + JSON encode/decode
- **NEVER** cloud sync, Firebase, SQLite

### Dependencies
- **MINIMUM** external packages. Prefer Flutter SDK.
- pub.dev BLOCKED in Russia → offline packages only

---

## 2. Localization — The Golden Pattern

```yaml
# l10n.yaml
synthetic-package: false
arb-dir: lib/l10n
template-arb-file: app_en.arb
output-localization-file: app_localizations.dart
output-class: AppLocalizations
nullable-getter: true
```

### CRITICAL Rules
| Rule | Why |
|------|-----|
| NO `generate: true` in pubspec.yaml WITHOUT `output-dir` in l10n.yaml | Old synthetic-package era created empty files; on Flutter ≥3.29 gen-l10n FAILS without `generate: true`, and with `output-dir` set nothing synthetic is created (Z-review 2026-10-08) |
| `nullable-getter: true` | Safe null handling |
| Remove `const` from rebuildable widgets | const prevents rebuild on locale change |
| `localeResolutionCallback` REQUIRED | Auto-detects device locale |
| Single MaterialApp only | Double causes navigator issues |
| EdgeInsetsDirectional everywhere | RTL support for Arabic |

### Tone of Voice (Обращение к пользователю)
- **Всегда «ты», никогда «Вы».**
- Во всех пользовательских строках используем неформальный, дружелюбный тон: русское «ты», английское `you` (неформальное), немецкое `du`, французское `tu`, испанское `tú` и т.д.
- Исключения только при явном запросе Владимира или если формальное обращение требуется юридически (EULA / Privacy Policy).

---

## 3. Code Conventions

- `const` where possible (NOT for locale-rebuildable widgets)
- Material 3 (`useMaterial3: true`)
- EdgeInsetsDirectional (RTL-ready)
- File naming: `snake_case.dart`
- Services: singletons. Screens: UI logic ONLY.

### Fonts / Typography
- **NEVER** add custom fonts or `google_fonts` without explicit approval from Vladimir.
- Default to system fonts (Roboto on Android, San Francisco on iOS).
- If a custom font is approved:
  - Verify it supports all 12 supported locales.
  - Confirm the font license allows commercial distribution and is compatible with RuStore / app stores.
  - Declare the font properly in `pubspec.yaml` and ship the font files in the repo.
- Custom fonts are a legal/licensing risk; avoid them unless truly necessary.

---

## 4. Privacy & Security (Rule #1)

### NEVER
- Firebase, analytics, crash reporting
- Unnecessary permissions (location, contacts)
- External data transmission
- API keys in source code
- Hardcoded secrets of any kind (keystore passwords, tokens, API keys)
- In-app advertising / cross-promo of any kind, including promotion of other NOVAX apps inside NOVAX apps

### ALWAYS
- Notification permission WITH explanation (Android 13+)
- Respect Do Not Disturb and sleep time
- Privacy Policy / EULA screens
- Keystore secrets only in `keystore.properties` (project root, git-ignored)

### Secrets Policy (mandatory)
> Секреты (пароли keystore, API-ключи, токены) — только в gitignore-файлах (`keystore.properties`, `.env`). Никогда не писать в код, gradle, workflow, контекстные MD, чаты. Найденный секрет в репо = репорт Владимиру + немедленная ротация.

---

## 5. Build & Release

1. **BEFORE the first release build of any app: run Designer Review.**
   - Mandatory gate: no release build without designer review and approved assets.
   - Full process: `shared/DESIGNER_REVIEW_WORKFLOW.md`.
   - Create **one review folder per project**: `C:\dev\novax-context-main\reviews\<project>_v<version>_designer\`.
   - Designer returns BLOCKER / WARNING / NICE-TO-HAVE. Fix all BLOCKERs and agreed WARNINGs.
   - Log all design/color decisions in `DECISIONS.md` and `design/COLOR_SYSTEM.md`.
   - Exception: emergency non-UI hotfix. Log the exception in `DECISIONS.md`.
2. **BEFORE every new build: run Z-Review.**
   - Z is an AI code reviewer. It checks up to 10 source files for bugs, leaks, architecture violations, and NOVAX rule breaks.
   - Full process: `shared/Z_REVIEW_WORKFLOW.md`.
   - Create **one review folder per project**: `C:\dev\novax-context-main\reviews\<project>_v<version>\`.
   - Prepare 10 `.txt` files + `PROMPT_FOR_Z.md`, then report the **full folder path** to Vladimir.
   - Output: BLOCKER / WARNING / NICE-TO-HAVE list. Fix all BLOCKERs and agreed WARNINGs before building.
   - After fixes are merged and the APK is rebuilt, **delete the temporary review folder** by default.
3. Update `version:` in pubspec.yaml
4. `flutter clean`
5. `flutter build apk --release`
6. Verify APK size (< 30 MB)
7. Test on Xiaomi / Samsung A52

Signing: Gradle signs release builds automatically using `keystore.properties` in the project root. The keystore password is never passed on the command line or stored in source code. Manual signing (if ever needed) must read the password from `keystore.properties`, never from a command-line argument.

---

## 6. The 9/10 Rule

> 9 steps automation, 10th step human verification.

---

## 7. Twelve Anti-Patterns (NEVER DO)

| # | Anti-Pattern | Symptom | Fix |
|---|-------------|---------|-----|
| 1 | Blind Automation | Deploy without checking | Always verify output |
| 2 | Context Vacuum | Forgetting constraints | Read CONTEXT_SHORT.md first |
| 3 | Perfectionism | Optimizing prematurely | Ship MVP, iterate |
| 4 | Silent Error | Ignoring warnings | Log all errors |
| 5 | Wild Growth | Scope creep | Freeze scope, backlog |
| 6 | Lone Ranger | Not updating context | Update CHANGELOG after every change |
| 7 | Data Ignorance | Editing without reading | cat/grep before sed |
| 8 | Golden Hammer | Same tool for everything | Right tool for right job |
| 9 | Secret Agent | Hidden code | Code in expected locations |
| 10 | Premature Optimization | Optimizing before measuring | Profile first |
| 11 | Human Crutch | Waiting for simple decisions | Decide, log, move on |
| 12 | Security Neglect | Hardcoding secrets | Keystore in .gitignore |

---

## 8. Roma (Kimi Code) — Rules of Engagement

> Role: Senior Flutter Engineer / Builder

### What Roma Does
- Reads `~/dev/context/shared/kimi_code/MEMORY.md` at session start
- Writes and reviews Flutter/Dart code
- Executes builds (flutter build, sign, adb install)
- Runs scripts and commands in terminal
- Updates MEMORY.md after each session
- Can make technical architecture decisions within NOVAX rules

### What Roma Does NOT Do
- Final business decisions → Vladimir (you)
- Pricing decisions → Vladimir (you)
- Translations → in-house (Рома+Владимир)
- Strategy/vision → Kimi (browser)

---

## 9. Kaneman Checklist (Every Feature)

- [ ] Framing: Message framed positively?
- [ ] Anchoring: Price/feature anchored correctly?
> ПРАВИЛО ЦЕНОВОГО ЯКОРЯ (Владимир+Z, 09.10.2026): фейковые «старые цены»
> (зачёркнутое «было 799 ₽») ЗАПРЕЩЕНЫ — бренд «честные ребята». Разрешён и
> рекомендован ЧЕСТНЫЙ якорь против подписки: зачёркнутая строка
> «Подписка на год: $29.99» (рыночная реальность категории) над реальной
> разовой ценой. Ключ ARB: `subscriptionAnchor`. См. DECISIONS.md 09.10.
- [ ] System 1: Can user decide in 3 seconds?
- [ ] Mental Laziness: Max 1 button for common actions?
- [ ] Peak-End: Is final interaction satisfying?

---

## 10. How I Update Context Files

After each session:
1. Update CHANGELOG.md
2. If architecture changed → ARCHITECTURE.md
3. If decisions made → DECISIONS.md
4. If new screens/services → SCREENS.md / SERVICES.md

---

## 11. Communication Style

- Russian with Vladimir. Technical terms in English.
- Direct and practical — no fluff.
- Pros/cons for alternatives.
- "Pognali!" — energy check.
- Stuck >3 attempts? Escalate with clear options.

---

## 12. Правило автономности Ромы (26.07)

Рома действует автономно. Подтверждения Владимира — только по красным линиям.

### БЕЗ подтверждения (делай сразу):
- Чтение файлов, поиск (`ls` / `grep` / `find` / `cat` / `git status` / `log` / `diff`)
- Сборки: `flutter build`, `flutter clean`, `gen-l10n`
- Правки файлов `.md` контекста и кода по задаче
- `git add` / `commit` / `push` (обычный, без `--force`)
- `build_release.sh` (протестированный скрипт)
- Установка APK на Xiaomi / Samsung A52 через `adb`
- Правки pub cache по известному workaround (`qr_code_scanner`)

### ВСЕГДА спрашивать Владимира:
- `git push --force` и любое переписывание истории git
- Удаление файлов/папок вне `build\` (`rm`, `del`, `Remove-Item`)
- Любые операции с keystore: копирование, перемещение, замена `.jks`, `key.properties`
- Действия вне `C:\dev\`
- Установка нового софта на машину
- Нечто, что нельзя откатить одним коммитом

### После каждой задачи — короткий отчёт:
- Что сделал (1–2 строки)
- Что изменилось (файлы, версии)
- Что дальше

### Не отменяет:
- Z-ревью перед первой сборкой приложения и после глобальных правок
- Ревью тимлида перед релизом
- Обновление CHANGELOG / MEMORY / ACTIVE_TASKS в конце сессии


## 14. Правило фиксации session ID (25.09, практика AMS-Control)

> После каждой задачи/сессии агент записывает ВСЁ в контекст (CHANGELOG) и **обязательно фиксирует свой session ID** — чтобы его можно было «разбудить» позже даже при почти исчерпанном контексте и задать вопросы.
>
> Формат в конце записи/отчёта:
> `- Session ID: <id> | Возобновление: kimi -S <id>`
>
> Владимир сохраняет эти ID вместе с контекст-файлами. Действует для всех сессий всех треков (Рома, Марк, Клинер, сторы).

---

## 13. Правило самопроверки и оценки качества

> При многоэтапных задачах (особенно контент/SEO/GEO/Flutter-правки): после каждого этапа и подэтапа — самопроверка и оценка результата по шкале **1–10**.
>
> **Если оценка < 9:** остановиться, исправить ошибки и недочёты, довести до 9+. Зафиксировать лучший результат в контексте. Только после этого двигаться дальше.
>
> **Правило «что увидит юзер» (Z, 17.10):** математически-корректный результат
> может быть по-человечески обманом — daysRemaining через 5 сек после траты
> показывал «29 дн.» против обещанных 30. Линтер в голове: проверяй не «сколько
> прошло/осталось по формуле», а «что увидит юзер в момент X». Именно такие баги
> рецензент и QA пропускают чаще всего.
>
> **Критерии оценки:**
> - **10** — идеально, можно показывать/деплоить без правок.
> - **9** — очень хорошо, мелкие придирки возможны, но качество рабочее.
> - **≤ 8** — есть заметные проблемы: опечатки, битые ссылки, неполнота, не соответствует стилю, не проходит технические проверки.
>
> Самопроверка включает: чтение созданного файла, `curl`-проверки URL, проверку валидности JSON/XML, проверку OG-мета, JSON-LD, canonical, ссылок, `flutter analyze`, тест на устройстве.
