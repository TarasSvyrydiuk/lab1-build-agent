\---

description: Викликає skill github-actions і створює CI workflow

\---



\# /create-actions



\## Передумови

\- Існують `ci.sh` і `ci.bat` (інакше: `FAIL create-actions: немає ci.sh/ci.bat. Спочатку виконайте /create-build`).

\- Skill `github-actions` існує.



\## Дії

1\. Прочитати skill `github-actions` і виконати його.

2\. Не перезаписувати наявний `ci.yml`; якщо він є, лише перевірити відповідність вимогам і повідомити про розбіжності.

3\. Не додавати секрети й токени.



\## Очікувані файли

\- `.github/workflows/ci.yml`



\## Команди перевірки

\- `python -c "import yaml; yaml.safe\_load(open('.github/workflows/ci.yml'))"`

\- У файлі є `ubuntu-latest`, `windows-latest`, `macos-latest`

\- У файлі немає `cmake` та `secrets.`



\## Повідомлення про помилку

\- Невалідний YAML: `FAIL create-actions: ci.yml не є валідним YAML`

\- Відсутня ОС у matrix: `FAIL create-actions: у matrix немає <os>`



\## Ідемпотентність

Існуючий `ci.yml` не перезаписується.



\## Успіх

Вивести `OK create-actions`.

