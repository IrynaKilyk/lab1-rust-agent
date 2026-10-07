---
description: "Послідовно підготувати, зібрати та перевірити Rust-проєкт hello через усі команди build-engineer."
agent: build-engineer
---

# 1. Призначення

Виконай команду `init` як оркестратор повного налаштування Rust-проєкту `hello`. Послідовно виконай `git-init`, `create-project`, `create-build`, `create-actions`, `check` і сформуй підсумковий звіт про кожен крок.

# 2. Передумови

- Поточний каталог є коренем репозиторію.
- Git встановлено; перевір це командою `git --version`.
- Cargo встановлено; перевір це командою `cargo --version`.
- Поточна робоча гілка не є `master` і не є `develop`. Перевір її командою `git branch --show-current`.
- Рекомендована назва робочої гілки має формат `feature/...`.
- Робоча гілка створена від `develop`; перевір це командою `git merge-base --is-ancestor develop HEAD`.
- Існують файли агента `.github/agents/build-engineer.agent.md`, три skills `.github/skills/project-scaffold/SKILL.md`, `.github/skills/build-and-test/SKILL.md`, `.github/skills/github-actions/SKILL.md` і шість prompts `.github/prompts/git-init.prompt.md`, `.github/prompts/create-project.prompt.md`, `.github/prompts/create-build.prompt.md`, `.github/prompts/create-actions.prompt.md`, `.github/prompts/check.prompt.md`, `.github/prompts/init.prompt.md`.
- Якщо будь-який із перелічених файлів відсутній, зупинися, назви причину і нічого не створюй.
- Не запускай оркестрацію з `master` або `develop`.

# 3. Дії

Виконай команди строго в такому порядку, після кожного кроку перевір його результат:

1. `git-init` — перевір Git-репозиторій, підготуй `.gitignore` і `.gitattributes` та передай наступному кроку їхній стан.
2. `create-project` — створи відсутню структуру Rust-проєкту та передай наступному кроку стан `Cargo.toml`, `src/main.rs` і `src/lib.rs`.
3. `create-build` — створи або перевір тест і CI-скрипти та передай наступному кроку стан `tests/basic_addition.rs`, `ci.sh` і `ci.bat`.
4. `create-actions` — створи або перевір workflow та передай наступному кроку стан `.github/workflows/ci.yml`.
5. `check` — виконай повну read-only перевірку всіх попередніх результатів.

Переходь до наступного кроку лише після успішної перевірки поточного. Не приховуй зміни або результати, отримані попередніми кроками.

# 4. Очікувані файли

Після успішного виконання мають існувати:

```text
.gitignore
.gitattributes
Cargo.toml
src/main.rs
src/lib.rs
tests/basic_addition.rs
ci.sh
ci.bat
.github/workflows/ci.yml
```

# 5. Команди перевірки

Фінально використай підсумок команди `check`. Успішним вважай лише результат, у якому всі пункти перевірки мають статус `PASS` і загальний підсумок також `PASS`.

# 6. Обробка помилок

- При першій помилці будь-якого кроку негайно зупинися.
- Виведи назву кроку, точну причину помилки та перелік того, що вже виконано.
- Наступні кроки не запускай.
- Помилки не приховуй і не вважай роботу успішною.
- Оголошуй успіх лише якщо команда `check` повернула загальний `PASS`.
- Нічого не видаляй і не виконуй автоматичних виправлень поза правилами відповідної команди.

# 7. Ідемпотентність

- Повторний запуск `init` на вже готовому проєкті нічого не ламає і не дублює.
- Використовуй ідемпотентність кожної команди: не перезаписуй коректні файли та не дублюй рядки або кроки.
- Повторний запуск має завершуватися `PASS`, якщо стан проєкту вже відповідає всім критеріям `check`.

## Підсумковий звіт

Поверни перелік виконаних кроків і статус кожного:

```text
git-init: PASS
create-project: PASS
create-build: PASS
create-actions: PASS
check: PASS
Загальний результат: PASS
```