# NexFort Project Plan

NexFort — кастомный Telegram-клиент на базе AyuGram Desktop с упором на производительность, приватность, удобство и power-user функции.

---

## Phase 0 — Baseline

- [ ] Убедиться, что чистый AyuGram собирается
- [x] Зафиксировать рабочую версию upstream
- [x] Добавить `upstream` remote
- [x] Настроить нормальные ветки
- [x] Добавить `CONTRIBUTING.md`
- [x] Добавить `CHANGELOG.md`
- [x] Добавить `PROJECT_PLAN.md`
- [x] Описать правила PR и code review

### Definition of Done

- Проект собирается без наших изменений
- Оба разработчика могут локально запустить клиент
- Есть понятный workflow через branches + PR

---

## Phase 1 — Audit

### Codebase

- [x] Найти основные директории UI
- [x] Найти настройки
- [x] Найти обработку сообщений
- [x] Найти navigation/chat switching
- [x] Найти hotkeys
- [x] Найти local storage
- [x] Найти network/update layer
- [x] Найти AyuGram-specific код

### Dependencies

- [x] Составить список крупных зависимостей
- [x] Понять, какие реально нужны
- [x] Проверить лицензии
- [x] Проверить устаревшие зависимости
- [x] Не удалять зависимости просто потому, что они выглядят ненужными

### Output

Создать:

`docs/architecture-notes.md`

где будет краткая карта проекта.

- [x] Создана краткая карта проекта и dependency audit

---

## Phase 2 — Cleanup

Цель: убрать ненужные для NexFort функции, но не развалить upstream compatibility.

### Functions

- [x] Составить список функций AyuGram
- [x] Пометить каждую:
  - KEEP
  - MODIFY
  - REMOVE
  - UNKNOWN

- [ ] Убирать только подтверждённо ненужные функции
- [x] Зафиксировать gate для удаления только подтверждённо ненужных функций
- [ ] После каждого удаления проверять сборку
- [ ] После каждого блока изменений запускать клиент

### Important

Не делать огромный commit вида:

`removed useless stuff`

Каждое изменение отдельно:

`remove legacy feature X`

`remove unused setting Y`

`cleanup Z module`

---

## Phase 3 — NexFort Branding

- [x] Название `NexFort`
- [ ] Иконка
- [x] About screen
- [x] Название окна
- [x] Названия настроек
- [ ] Собственная версия приложения
- [ ] Убрать ненужные AyuGram branding элементы
- [ ] Сохранить обязательные license/copyright notices

---

## Phase 4 — UI Redesign

### Design System

- [ ] Определить основной стиль
- [ ] Цветовую систему
- [ ] Размеры отступов
- [ ] Border radius
- [ ] Typography
- [ ] Иконки
- [ ] Hover/active states

### Main UI

- [ ] Sidebar
- [ ] Chat list
- [ ] Chat header
- [ ] Message bubbles
- [ ] Input field
- [ ] Context menus
- [ ] Settings
- [ ] Search
- [ ] Dialog windows

### UX

- [ ] Уменьшить визуальный шум
- [ ] Сделать быстрый доступ к частым действиям
- [ ] Нормальная keyboard navigation
- [ ] Проверить UI на разных размерах окна

---

## Phase 5 — Performance

Важно: сначала измеряем, потом оптимизируем.

### Measurements

- [ ] Startup time
- [ ] RAM usage
- [ ] CPU idle usage
- [ ] CPU при scrolling
- [ ] Размер binary
- [ ] Время открытия большого чата
- [ ] Время поиска

### Optimization

- [ ] Найти реальные bottlenecks
- [ ] Удалить ненужную background работу
- [ ] Проверить excessive logging
- [ ] Проверить ненужные UI updates
- [ ] Проверить тяжёлые animations
- [ ] Проверить cache behavior

### Rule

Никакого:

`эта функция выглядит тяжёлой, удаляем`

Сначала profiling → потом изменение.

---

# NexFort Features

## Feature 1 — Command Palette

Shortcut:

`Ctrl + K`

Открывает глобальную командную строку NexFort.

### MVP

- [ ] Открытие `Ctrl + K`
- [ ] Закрытие `Esc`
- [ ] Поиск команд
- [ ] Keyboard navigation
- [ ] Enter для выполнения

### Commands v0.1

- [ ] `open @username`
- [ ] `open <chat>`
- [ ] `search <query>`
- [ ] `settings`

### Later

- [ ] `files @username`
- [ ] `media @username`
- [ ] `mute @username`
- [ ] `theme dark`
- [ ] `theme light`
- [ ] `note @username`
- [ ] `saved`
- [ ] fuzzy search

Пример:

`Ctrl + K`

`open @arsen`

→ NexFort мгновенно открывает чат.

---

## Feature 2 — Deleted Messages

- [ ] Сохранять доступную локальную историю сообщения
- [ ] Отмечать удалённые сообщения
- [ ] Не путать удалённое сообщение с обычным
- [ ] Настройка включения/выключения
- [ ] Очистка локальной истории

---

## Feature 3 — Edit History

- [ ] Показывать прошлые версии отредактированного сообщения
- [ ] Кнопка `Edit history`
- [ ] Timestamp изменений
- [ ] Возможность отключить хранение

---

## Feature 4 — Local Notes

Локальные заметки, которые не отправляются Telegram.

- [ ] Note for user
- [ ] Note for chat
- [ ] Редактирование
- [ ] Удаление
- [ ] Поиск заметок

---

## Feature 5 — NexFort Tools

Context actions:

- [ ] Copy as Markdown
- [ ] Copy link
- [ ] Save to collection
- [ ] Show edit history
- [ ] Open sender
- [ ] Search messages from sender

---

# Privacy

- [ ] Определить, какие данные NexFort хранит локально
- [ ] Не добавлять собственную telemetry без явного решения
- [ ] Не логировать чувствительные данные
- [ ] Проверить crash logs
- [ ] Проверить debug logs
- [ ] Проверить session storage
- [ ] Документировать privacy-sensitive функции

---

# Security

- [ ] Threat model
- [ ] Dependency review
- [ ] Secrets review
- [ ] Local storage review
- [ ] Input validation
- [ ] File handling review
- [ ] Update mechanism review
- [ ] Code review второго участника для security-sensitive изменений

---

# Upstream Strategy

NexFort основан на AyuGram, поэтому нельзя просто забыть про upstream.

- [ ] Проверять новые версии AyuGram
- [ ] Отдельно отслеживать security fixes
- [ ] Не merge-ить upstream вслепую
- [ ] Перед merge просматривать изменения
- [ ] После merge запускать regression tests

Flow:

AyuGram upstream

↓

review

↓

NexFort integration branch

↓

tests

↓

main

---

# Git Workflow

## Branches

`main`

`develop`

`feature/*`

`fix/*`

`refactor/*`

`security/*`

Примеры:

`feature/command-palette`

`feature/local-notes`

`refactor/chat-list`

`fix/search-crash`

---

## Pull Requests

Каждая задача:

Issue

↓

Branch

↓

Code

↓

Test

↓

Pull Request

↓

Review второго участника

↓

Merge

---

# Releases

## v0.1 — Prototype

- [ ] Clean build
- [ ] NexFort branding
- [ ] Command Palette
- [ ] `open @username`
- [ ] базовая стабильность

## v0.2 — Cleanup

- [ ] удалить подтверждённо ненужные функции
- [ ] базовый UI redesign
- [ ] performance measurements

## v0.3 — Power Tools

- [ ] расширенная Command Palette
- [ ] edit history
- [ ] local notes

## v0.4 — Privacy

- [ ] privacy settings
- [ ] storage controls
- [ ] logging cleanup

## v0.5 — Beta

- [ ] UI polish
- [ ] performance pass
- [ ] bug fixing
- [ ] internal testing

## v1.0

NexFort достаточно стабилен, чтобы использовать его как основной Telegram-клиент.

---

# Current Priority

1. Clean build
2. Architecture audit
3. Command Palette
4. Branding
5. Только потом cleanup и redesign
