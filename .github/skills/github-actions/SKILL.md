---
name: github-actions
description: 'Створює GitHub Actions workflow для Rust-проєкту hello, який запускає наявні CI-скрипти на Ubuntu, Windows і macOS та завантажує артефакти. Застосовувати для налаштування кросплатформного CI без дублювання логіки збірки у YAML.'
---

# 1. Призначення і коли застосовувати

Цей skill описує створення workflow GitHub Actions для Rust-проєкту `hello` агентом `build-engineer`. Застосовуй його, коли потрібно налаштувати кросплатформну перевірку через наявні `ci.sh` і `ci.bat` та завантаження каталогу `build/` як артефакту.

# 2. Вхідні дані

- Наявний `ci.sh` у корені репозиторію.
- Наявний `ci.bat` у корені репозиторію.
- Робочий каталог є коренем репозиторію.

# 3. Що створює

Створи `.github/workflows/ci.yml` з такою структурою:

- Тригери `push` і `pull_request` для гілок `develop` і `master`.
- Одну job із матрицею `os: [ubuntu-latest, windows-latest, macos-latest]` і `fail-fast: false`.
- Крок `actions/checkout@v4`.
- На Unix крок, який викликає `bash ci.sh`.
- На Windows крок, який викликає `ci.bat` із `shell: cmd`.
- Крок `actions/upload-artifact@v4` для каталогу `build/`.

Workflow має лише викликати CI-скрипти та завантажувати їхній результат. Не додавай у `ci.yml` логіку збірки або тестування (`cargo build`, `cargo test` тощо) безпосередньо. Не додавай секрети, токени або паролі.

Приклад очікуваної структури workflow:

```yaml
name: Rust CI

on:
  push:
    branches: [develop, master]
  pull_request:
    branches: [develop, master]

jobs:
  ci:
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - name: Run Unix CI script
        if: runner.os != 'Windows'
        run: bash ci.sh
      - name: Run Windows CI script
        if: runner.os == 'Windows'
        run: ci.bat
        shell: cmd
      - uses: actions/upload-artifact@v4
        with:
          name: hello-${{ matrix.os }}
          path: build/
```

# 4. Ідемпотентність

- Перед створенням або зміною наявного `.github/workflows/ci.yml` перевіряй його вміст.
- Наявний `ci.yml` не перезаписуй без перевірки.
- Не дублюй тригери, job, кроки або налаштування.
- Повторне застосування skill не повинно змінювати поведінку workflow без потреби.

# 5. Приклад входу

```text
language: Rust, name: hello, scripts: ci.sh, ci.bat, workflow: GitHub Actions
```

# 6. Очікуваний результат

Створи або, після перевірки, онови один файл:

```text
.github/workflows/ci.yml
```

# 7. Перевірки

1. Переконайся, що YAML у `.github/workflows/ci.yml` валідний.
2. Перевір наявність усіх трьох ОС: `ubuntu-latest`, `windows-latest`, `macos-latest`.
3. Перевір наявність викликів `bash ci.sh` і `ci.bat` із `shell: cmd` для Windows.
4. Переконайся, що в YAML немає безпосередніх викликів `cargo build` або `cargo test`.
5. Переконайся, що workflow не містить секретів, токенів або паролів.
6. Перевір наявність `actions/upload-artifact@v4` із шляхом `build/`.
7. Повідом результати кожної перевірки.

# 8. Типовий сценарій помилки

Якщо `ci.sh` або `ci.bat` відсутні, YAML невалідний або на Windows указано неправильний shell:

- Зупинися на відповідному кроці.
- Повідом назву кроку та точну причину помилки.
- Нічого не видаляй.
- Не вважай роботу успішною, доки причину не усунуто та перевірки не пройдено.