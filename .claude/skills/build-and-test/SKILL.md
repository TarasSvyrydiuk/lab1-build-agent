\---

name: build-and-test

description: Створює CMakeLists.txt, функцію додавання, юніт-тест BasicAddition на GoogleTest та скрипти ci.sh/ci.bat. Використовувати після project-scaffold.

\---



\# build-and-test



\## Призначення

Налаштувати складання CMake, додати тестову функцію та юніт-тест, а також

скрипти складання `ci.sh` і `ci.bat`.



\## Вхід

\- `project\_name` — назва проєкту (target виконуваного файлу: `hello`)

\- Наявність `src/main.cpp` (передумова)



Приклад входу: `project\_name=hello`



\## Підкоманди

1\. \*\*cmake-config\*\* — створити `CMakeLists.txt` (C++17, FetchContent для

&#x20;  GoogleTest, `enable\_testing()`, target `hello`, target `unit\_tests`,

&#x20;  `gtest\_discover\_tests`).

2\. \*\*add-function\*\* — створити `src/math\_utils.h` і `src/math\_utils.cpp` із

&#x20;  функцією `int add(int a, int b)`.

3\. \*\*add-test\*\* — створити `tests/test\_basic.cpp` із тестом

&#x20;  `TEST(Basic, BasicAddition)`, який перевіряє `add(2, 3) == 5`.

4\. \*\*add-scripts\*\* — створити `ci.sh` та `ci.bat`, які виконують

&#x20;  конфігурацію, складання й `ctest` (для Windows із `-C Release`).



\## Ключові фрагменти

`tests/test\_basic.cpp`:

```cpp

\#include <gtest/gtest.h>

\#include "math\_utils.h"



TEST(Basic, BasicAddition) {

&#x20;   EXPECT\_EQ(add(2, 3), 5);

}

```



`ci.sh`:

```bash

\#!/usr/bin/env bash

set -euo pipefail

cmake -S . -B build -DCMAKE\_BUILD\_TYPE=Release

cmake --build build --config Release

ctest --test-dir build -C Release --output-on-failure

```



`ci.bat`:

```bat

@echo off

cmake -S . -B build || exit /b 1

cmake --build build --config Release || exit /b 1

ctest --test-dir build -C Release --output-on-failure || exit /b 1

```



\## Вихід (очікувані файли)

\- `CMakeLists.txt`, `src/math\_utils.h`, `src/math\_utils.cpp`

\- `tests/test\_basic.cpp`, `ci.sh`, `ci.bat`



\## Перевірки

\- `cmake -S . -B build` завершується без помилок.

\- `cmake --build build` створює виконуваний файл `hello`.

\- `ctest` показує, що тест `Basic.BasicAddition` пройшов.

\- `build/` не відстежується Git (`git ls-files build` порожній).

\- Повторний запуск не дублює рядки в `CMakeLists.txt`.



\## Типовий сценарій помилки

\- \*\*Помилка:\*\* немає `src/main.cpp`.

\- \*\*Реакція:\*\* "FAIL build-and-test: відсутній src/main.cpp. Спочатку

&#x20; виконайте /create-project", файли не створюються.

\- \*\*Помилка 2:\*\* тест падає (`add` повертає неправильне значення). Агент не

&#x20; підганяє очікуване значення в тесті, а виправляє функцію `add` або

&#x20; повідомляє про проблему.

