# Архітектура AI-агента

## Завдання
Агент build-engineer автоматизує створення, складання, тестування та
CI-перевірку мінімального Rust-проєкту Hello World. Він знімає ручну
рутину: структура проєкту, Cargo.toml, тест, скрипти складання, workflow.

## Відповідність файлів ролям (GitHub Copilot)
| Файл | Роль |
|---|---|
| .github/agents/build-engineer.agent.md | Agent manifest |
| .github/skills/project-scaffold/SKILL.md | Skill: створення проєкту |
| .github/skills/build-and-test/SKILL.md | Skill: складання і тести |
| .github/skills/github-actions/SKILL.md | Skill: CI workflow |
| .github/prompts/*.prompt.md | Commands (git-init, create-project, create-build, create-actions, check, init) |
| Cargo.toml | Замість CMakeLists.txt (система складання Rust) |

## Skills
- project-scaffold: Hello World, README, .gitignore.
- build-and-test: Cargo.toml, lib.rs, тест BasicAddition, ci.sh/ci.bat.
- github-actions: .github/workflows/ci.yml (матриця з 3 ОС, лише виклик скриптів).

## Commands
git-init, create-project, create-build, create-actions, check (лише читає),
init (оркестратор).

## Дані між етапами
git-init → стан репозиторію та .gitignore → create-project → src/, Cargo.toml
→ create-build → тест та ci-скрипти → create-actions → ci.yml → check → звіт.

## Обробка помилок оркестратора
init виконує команди послідовно й зупиняється на першій помилці, виводячи
назву кроку та причину. Успіх оголошується лише після того, як check
пройшов без помилок. Повторний запуск не перезаписує наявні файли.