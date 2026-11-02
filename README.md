# Lab 1: AI-агент build-engineer

**ПІП:** Свиридюк Тарас Русланович
**Група:** КІ-408
**Варіант:** 1 (№ 19) — C++ / CMake / GoogleTest / Claude Code (CLI), Claude Opus 5.5

## Опис

AI-агент `build-engineer` автоматично створює у новому репозиторії
мінімальний Hello World-проєкт на C++, налаштовує систему складання CMake,
додає юніт-тест на GoogleTest і CI-workflow для GitHub Actions
(Windows, Linux, macOS).

## Структура агента

- `build-engineer.agent.md` — manifest агента
- `.claude/skills/` — skills: project-scaffold, build-and-test, github-actions
- `.claude/commands/` — commands: git-init, create-project, create-build,
  create-actions, check, init

## Запуск

1. Встановити Git, CMake, компілятор C++ та Claude Code CLI.
2. У корені репозиторію виконати `claude`.
3. Запустити команду `/init` (оркестратор).
4. Локальна перевірка:

   cmake -S . -B build
   cmake --build build
   ctest --test-dir build --output-on-failure

Докладніше: `docs/usage.md`, `docs/architecture.md`.