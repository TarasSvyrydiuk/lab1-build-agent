\---

description: Викликає skill project-scaffold і створює початковий проєкт

\---



\# /create-project



Аргументи: `language` (за замовчуванням `cpp`), `project\_name` (за замовчуванням `hello`).



\## Передумови

\- Репозиторій ініціалізований (`/git-init` виконано).

\- Skill `project-scaffold` існує в `.claude/skills/project-scaffold/SKILL.md`.



\## Дії

1\. Прочитати skill `project-scaffold` і виконати його з переданими аргументами.

2\. Не перезаписувати наявні файли; для кожного пропущеного вивести `SKIP`.



\## Очікувані файли

\- `src/main.cpp`

\- `README.md` із розділом про складання й запуск

\- `.gitignore` із `build/`

\- каталоги `src/`, `tests/`, `docs/`



\## Команди перевірки

\- `Test-Path src/main.cpp` повертає `True`

\- `git check-ignore build/` повертає `build/`



\## Повідомлення про помилку

\- Непідтримувана мова: `FAIL create-project: мова '<x>' не підтримується. Доступно: cpp`

\- Немає репозиторію: `FAIL create-project: спочатку виконайте /git-init`



\## Ідемпотентність

Існуючі файли не змінюються.



\## Успіх

Вивести `OK create-project` і список створених та пропущених файлів.

