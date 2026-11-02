\---

description: Викликає skill build-and-test і створює конфігурацію складання та тести

\---



\# /create-build



Аргументи: `project\_name` (за замовчуванням `hello`).



\## Передумови

\- Існує `src/main.cpp` (інакше: `FAIL create-build: відсутній src/main.cpp. Спочатку виконайте /create-project`).

\- Skill `build-and-test` існує.



\## Дії

1\. Прочитати skill `build-and-test` і виконати всі підкоманди: cmake-config, add-function, add-test, add-scripts.

2\. Не перезаписувати наявні файли й не дублювати рядки в `CMakeLists.txt`.

3\. Не підганяти очікуване значення в тесті під результат функції.



\## Очікувані файли

\- `CMakeLists.txt`

\- `src/math\_utils.h`, `src/math\_utils.cpp`

\- `tests/test\_basic.cpp` (тест `BasicAddition`)

\- `ci.sh`, `ci.bat`



\## Команди перевірки

\- `cmake -S . -B build`

\- `cmake --build build --config Release`

\- `ctest --test-dir build -C Release --output-on-failure`



\## Повідомлення про помилку

\- Помилка конфігурації або складання: вивести фрагмент логу й `FAIL create-build: <етап>`.

\- Тест не пройшов: `FAIL create-build: BasicAddition не пройшов`.



\## Ідемпотентність

Повторний запуск лише пропускає наявні файли.



\## Успіх

Вивести `OK create-build`.

