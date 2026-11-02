# Архітектура агента build-engineer

## 1. Які проблеми вирішує агент
- Ручне налаштування нового C++-проєкту займає час і містить типові
  помилки (забутий .gitignore, неправильний CMake, відсутні тести).
- Відсутність єдиного CI для Windows, Linux та macOS.
- Відсутність автоматичної перевірки результату після генерації.
- Ризик перезапису вже створених файлів при повторному запуску.

## 2. Skills
| Skill | Призначення |
|-------|-------------|
| project-scaffold | Hello World, README, структура каталогів, .gitignore |
| build-and-test | CMakeLists.txt, функція додавання, тест BasicAddition (GoogleTest) |
| github-actions | ci.yml з matrix (3 ОС), який викликає ci.sh / ci.bat |

## 3. Commands
| Command | Дія |
|---------|-----|
| /git-init | Перевіряє репозиторій, створює або оновлює .gitignore, показує git status |
| /create-project | Викликає project-scaffold |
| /create-build | Викликає build-and-test |
| /create-actions | Викликає github-actions |
| /check | Лише перевіряє структуру, конфігурацію, складання, тести та YAML |
| /init-project | Оркестратор: послідовно запускає всі попередні команди |

Примітка: у завданні оркестратор названо `init`. У файлі команди він
має ім'я `init-project`, щоб не конфліктувати з вбудованою командою
/init у Claude Code.

## 4. Схема взаємодії

    build-engineer.agent.md (роль, правила, заборони)
              |
              v
    /init-project (оркестратор)
       |
       +--> /git-init        --> .gitignore, стан Git
       +--> /create-project  --> skill project-scaffold
       +--> /create-build    --> skill build-and-test
       +--> /create-actions  --> skill github-actions
       +--> /check           --> звіт OK / FAIL

## 5. Дані між етапами
| Звідки | Що передається | Куди |
|--------|----------------|------|
| Користувач | Назва проєкту, мова (C++) | /create-project |
| /git-init | Статус репозиторію, наявність .gitignore | /create-project |
| /create-project | Шляхи: src/main.cpp, README.md, структура каталогів | /create-build |
| /create-build | Ім'я target (hello), ім'я тесту (BasicAddition), ci.sh / ci.bat | /create-actions |
| /create-actions | Шлях до .github/workflows/ci.yml | /check |
| /check | Результат: список перевірок зі статусом OK / FAIL | Користувач |

## 6. Обробка помилок оркестратором
1. Кожна команда повертає статус OK або FAIL з повідомленням.
2. При FAIL /init-project негайно зупиняється й виводить: назву кроку,
   причину, що виправити.
3. Передумови перевіряються до дій: наприклад, /create-build не
   запускається без src/main.cpp.
4. Наявні файли не перезаписуються, тому повторний запуск після
   виправлення безпечний.
5. Фінальний крок /check підтверджує, що результат відповідає вимогам.
6. Помилки не виправляються вручну в результаті: змінюються інструкції
   агента, skills або commands, а помилка записується в docs/ai-log.md.