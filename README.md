# Lab 1: AI-агент для автоматизації розробки та CI/CD

**ПІБ:** Кілик Ірина Русланівна
**Група:** КІ-403
**Варіант:** 10 (4) (Rust, Cargo, cargo test)

## Опис
Репозиторій містить AI-агента build-engineer (GitHub Copilot), який
генерує, збирає, тестує та перевіряє кросплатформний проєкт Hello World
на Rust, а також налаштовує GitHub Actions для Windows, Linux і macOS.

## Запуск агента
1. Відкрити репозиторій у VS Code з розширенням GitHub Copilot.
2. У Copilot Chat обрати режим Agent.
3. Виконати команду `/init`.

## Запуск проєкту
- Linux/macOS: `bash ci.sh`
- Windows: `ci.bat`
- Готовий файл: `build/hello` (на Windows `build\hello.exe`)

Докладніше: [docs/usage.md](docs/usage.md), [docs/architecture.md](docs/architecture.md)