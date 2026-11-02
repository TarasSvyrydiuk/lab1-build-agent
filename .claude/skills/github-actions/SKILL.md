\---

name: github-actions

description: Створює .github/workflows/ci.yml із matrix Windows/Linux/macOS, який лише запускає ci.sh або ci.bat. Використовувати після build-and-test.

\---



\# github-actions



\## Призначення

Створити CI workflow, який на трьох ОС виконує checkout і запускає скрипти

складання. Уся логіка складання лежить у `ci.sh` / `ci.bat`.



\## Вхід

\- Наявні `ci.sh` та `ci.bat` (передумова)

\- Гілки для тригерів: `develop`, `master`



Приклад входу: `branches=develop,master`



\## Дії

1\. Перевірити наявність `ci.sh` і `ci.bat`.

2\. Створити `.github/workflows/ci.yml`, якщо його немає:

&#x20;  ```yaml

&#x20;  name: CI



&#x20;  on:

&#x20;    push:

&#x20;      branches: \[develop, master]

&#x20;    pull\_request:

&#x20;      branches: \[develop, master]



&#x20;  jobs:

&#x20;    build:

&#x20;      strategy:

&#x20;        fail-fast: false

&#x20;        matrix:

&#x20;          os: \[ubuntu-latest, windows-latest, macos-latest]

&#x20;      runs-on: ${{ matrix.os }}

&#x20;      steps:

&#x20;        - uses: actions/checkout@v4

&#x20;        - name: Build and test (Linux/macOS)

&#x20;          if: runner.os != 'Windows'

&#x20;          run: bash ci.sh

&#x20;        - name: Build and test (Windows)

&#x20;          if: runner.os == 'Windows'

&#x20;          run: ci.bat

&#x20;          shell: cmd

&#x20;  ```

3\. Не додавати секрети та токени.



\## Вихід

\- `.github/workflows/ci.yml`



\## Перевірки

\- YAML валідний (наприклад, `python -c "import yaml; yaml.safe\_load(open('.github/workflows/ci.yml'))"`).

\- Matrix містить `ubuntu-latest`, `windows-latest`, `macos-latest`.

\- Тригери `push` і `pull\_request` для `develop` та `master`.

\- У workflow немає `secrets.` та токенів, а кроки складання не містять

&#x20; прямих викликів `cmake`.

\- Одна job `build` із matrix.



\## Типовий сценарій помилки

\- \*\*Помилка:\*\* відсутній `ci.bat`.

\- \*\*Реакція:\*\* "FAIL github-actions: немає ci.bat. Спочатку виконайте

&#x20; /create-build", workflow не створюється.

\- \*\*Помилка 2:\*\* `ci.sh` без прав виконання на Linux. Тому в workflow

&#x20; використано `bash ci.sh`, а не `./ci.sh`.

