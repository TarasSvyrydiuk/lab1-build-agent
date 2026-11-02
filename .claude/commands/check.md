\---

description: Лише перевіряє структуру, конфігурацію, складання, тести та YAML. Нічого не змінює в проєкті

\---



\# /check



\*\*Ця команда не створює, не змінює й не видаляє жодного файлу в репозиторії.\*\*

Єдиний виняток: тимчасовий каталог `build/`, який ігнорується Git.



\## Передумови

\- Виконано хоча б `/git-init`.



\## Перевірки (кожна дає OK або FAIL)

1\. Існують `build-engineer.agent.md`, `docs/architecture.md`, `docs/ai-log.md`.

2\. Manifest містить розділи про роль, заборонені дії та критерії завершення.

3\. Кожен skill має призначення, вхід, вихід і перевірки.

4\. Існують усі шість commands.

5\. Існують `src/main.cpp`, `CMakeLists.txt`, `tests/test\_basic.cpp`, `ci.sh`, `ci.bat`.

6\. `cmake -S . -B build` та `cmake --build build --config Release` проходять.

7\. Існує виконуваний файл `hello`.

8\. `ctest --test-dir build -C Release --output-on-failure` проходить; `BasicAddition` успішний.

9\. `.github/workflows/ci.yml` валідний YAML, містить три ОС, тригери push і pull\_request для develop та master.

10\. У workflow немає прямих викликів `cmake` і немає `secrets.`.

11\. `git ls-files build` порожній, а `build/` є в `.gitignore`.

12\. У відстежуваних файлах немає секретів (пошук `password`, `token`, `secret`, `api\_key`).



\## Повідомлення про помилку

Для кожної невдалої перевірки: `FAIL check: <номер і назва> — <причина>`.

Не виправляти нічого автоматично, лише повідомити.



\## Підсумок

Таблиця «перевірка / статус». Якщо всі OK: `OK check`. Інакше: `FAIL check: N з 12 не пройшли`.

