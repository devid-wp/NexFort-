# NexFort Project Plan

NexFort — кастомный Telegram-клиент на базе AyuGram Desktop с упором на
производительность, приватность, удобство и power-user функции.

Главная текущая цель — выпустить первый рабочий прототип `v0.1`. Изменения,
которые не нужны для этого результата, откладываются в дальнейший roadmap.

## Текущее состояние

### Готово

- [x] Зафиксирована рабочая версия AyuGram upstream
- [x] Подготовлены `CONTRIBUTING.md`, `CHANGELOG.md` и правила code review
- [x] Составлены карта архитектуры и audit крупных зависимостей
- [x] Инвентаризированы AyuGram-specific функции и правила безопасного cleanup
- [x] Определены визуальное направление, цветовая система, typography, spacing,
  icons и interaction states
- [x] Добавлены имя NexFort, пользовательская версия `0.1`, About screen,
  названия окна и настроек
- [x] Отделены platform identity, executable name, shortcuts и data profile
- [x] Отключены унаследованные update-сервисы
- [x] Сохранены обязательные license и copyright notices

### Открытые блокеры

- [ ] Добавить и проверить `upstream` remote
- [ ] Создать integration branch `develop` согласно `CONTRIBUTING.md`
- [ ] Выполнить чистую Debug-сборку на основной целевой платформе
- [ ] Запустить клиент и записать результат baseline smoke test
- [ ] Завершить пользовательский branding и release metadata
- [ ] Реализовать Command Palette MVP

## v0.1 — Первый прототип

Прототип должен запускаться как самостоятельный NexFort, не обращаться к
update-инфраструктуре AyuGram и предоставлять рабочую Command Palette для
основных навигационных действий.

### 1. Baseline и release safety

- [ ] Настроить `upstream` и ветки `main`, `develop`, `feature/*`, `fix/*`
- [ ] Подтвердить, что профиль данных NexFort не пересекается с AyuGram или
  Telegram Desktop на каждой поддерживаемой платформе
- [ ] Проверить application ID, executable name, ярлыки, desktop/service files,
  установочные ресурсы и имя каталога данных
- [ ] Проверить, что приложение не запрашивает и не устанавливает обновления из
  каналов AyuGram или Telegram Desktop
- [ ] Для `v0.1` использовать GitHub Releases как ручной канал распространения;
  собственный auto-update в прототип не входит
- [ ] Зафиксировать успешную Debug-сборку и платформу проверки

### 2. Branding

- [ ] Создать временную самостоятельную иконку-монограмму `NF`
- [ ] Подготовить варианты иконки для Windows, macOS и all-other платформ
- [ ] Заменить пользовательские AyuGram-иконки и оставшиеся необязательные
  упоминания AyuGram
- [ ] Обновить README, package metadata и release metadata под NexFort
- [ ] Проверить About, onboarding, settings, tray, menus, notifications, crash UI
  и exported HTML
- [ ] Не изменять обязательные license и copyright notices

Финальная иконка не блокирует прототип: монограмма `NF` должна быть заменяемым
временным asset без изменения platform identity.

### 3. Command Palette

#### Точка входа и жизненный цикл

- [ ] Добавить `Shortcuts::Command::CommandPalette` и имя команды
  `command_palette`
- [ ] Назначить `Ctrl+K` на Windows/all-other и `Cmd+K` на macOS
- [ ] Обрабатывать shortcut только в активном разблокированном окне
- [ ] Не открывать палитру поверх уже показанного blocking layer
- [ ] Повторное нажатие shortcut должно фокусировать открытую палитру
- [ ] Владение состоянием и временем жизни палитры разместить в
  `Window::SessionController`
- [ ] При закрытии отменять принадлежащие палитре сетевые запросы и возвращать
  фокус предыдущему элементу интерфейса, если он ещё существует

#### Интерфейс

- [ ] Реализовать отдельный overlay с полем ввода и списком результатов
- [ ] Добавить состояния: начальное, loading, результаты, пустая выдача и ошибка
- [ ] Поддержать `Esc`, `Up`, `Down`, `Enter`, клик и hover мыши
- [ ] Автоматически выбирать первый доступный результат
- [ ] Использовать строки из `lang.strings` и размеры из `.style`-файлов
- [ ] Проверить day/night themes, interface scaling и узкое окно
- [ ] Не добавлять fuzzy matching, историю запросов и persistent settings в
  `v0.1`

#### Поиск команд

- [ ] При пустом вводе показывать четыре команды MVP
- [ ] Фильтровать команды регистронезависимо по имени и ключевому слову
- [ ] Не выполнять неизвестную или неполную команду
- [ ] Для команды без обязательного аргумента показывать локализованную подсказку
  и оставлять палитру открытой

#### Команды MVP

- [ ] `settings` закрывает палитру и открывает главную страницу настроек
- [ ] `search <query>` закрывает палитру и запускает существующий глобальный
  поиск сообщений через `Dialogs::SearchState`
- [ ] `open @username` разрешает username через существующую session navigation
  и открывает найденный peer
- [ ] `open <chat>` сразу показывает локальные совпадения по известным чатам и
  контактам, затем дополняет список глобальным `Api::PeerSearch`
- [ ] Для глобального поиска использовать `Api::PeerSearch::Type::JustPeers`,
  без sponsored results и без новых MTProto-методов
- [ ] Удалять дубликаты локальных и удалённых результатов по peer ID
- [ ] Не показывать ответ для старого запроса после изменения текста
- [ ] Отменять поиск при закрытии палитры или запуске нового запроса
- [ ] Обрабатывать неизвестный username, пустую выдачу и отсутствие сети без
  закрытия приложения и без потери введённого текста

### 4. Privacy и compatibility gate

- [ ] Убедиться, что Command Palette не добавляет telemetry и не записывает
  запросы в новое persistent storage
- [ ] Не добавлять новую локальную БД или поля последовательной сериализации
- [ ] Проверить crash, debug и session logs на чувствительные данные
- [ ] Документировать локально хранимые NexFort/AyuGram-specific данные
- [ ] Проверить lifetime всех async callbacks и отмену запросов вместе с owner
- [ ] Провести отдельный review изменений platform identity и сетевого поведения

### 5. Проверка прототипа

#### Статические проверки

- [ ] Просмотреть C++, style, localization и platform-resource diff
- [ ] Проверить отсутствие hardcoded UI dimensions и новых нелокализованных строк
- [ ] Проверить отсутствие необязательного AyuGram branding в пользовательских
  поверхностях и package metadata
- [ ] Проверить LF line endings и отсутствие UTF-8 BOM

#### Command Palette scenarios

- [ ] Shortcut открывает палитру только для активного session window
- [ ] Shortcut не работает на lock screen или поверх blocking layer
- [ ] `Esc` закрывает палитру, стрелки меняют выбор, `Enter` выполняет его
- [ ] `settings` открывает главные настройки
- [ ] `search test query` открывает глобальный поиск с полным текстом запроса
- [ ] `open @username` покрывает найденный, неизвестный и недоступный username
- [ ] `open <chat>` покрывает локальный чат, remote peer, дубликат и пустую выдачу
- [ ] Offline search сохраняет работоспособность локальных результатов
- [ ] Быстрая смена запроса не показывает устаревший remote result
- [ ] Закрытие окна во время запроса не вызывает callback к уничтоженному UI
- [ ] Keyboard и mouse navigation работают при разных масштабах интерфейса

#### Smoke test

- [ ] Выполнить Debug-сборку согласно инструкции для целевой платформы
- [ ] Запустить NexFort с отдельным тестовым профилем
- [ ] Проверить login или открытие существующей тестовой сессии
- [ ] Открыть чат, отправить сообщение и выполнить все четыре команды MVP
- [ ] Перезапустить клиент и подтвердить сохранность данных
- [ ] Записать платформу, результат и известные ограничения прототипа

## Definition of Done для v0.1

- NexFort собирается и запускается под собственной platform identity.
- Приложение не использует update-сервисы AyuGram или Telegram Desktop.
- Во всех пакетах прототипа используется временная иконка `NF`.
- В пользовательском UI и release metadata нет необязательного AyuGram branding.
- `settings`, `search <query>`, `open @username` и `open <chat>` работают с
  клавиатуры и мыши.
- Нет новой telemetry, persistent command history или несовместимых изменений
  локального хранилища.
- Разработчик подтвердил успешную Debug-сборку и smoke test хотя бы на основной
  целевой платформе.
- Известные ограничения записаны в `CHANGELOG.md` или release notes.

## Не входит в v0.1

- Полный redesign главного интерфейса
- Fuzzy search, история и пользовательская настройка Command Palette
- Local Notes, collections и новые context actions
- Новая реализация deleted messages или edit history
- Собственный auto-update
- Оптимизации без предварительных measurements
- Массовое удаление AyuGram-specific функций

## Дальнейший roadmap

### v0.2 — Cleanup и UI foundation

- Удалить только подтверждённо ненужные AyuGram-specific функции
- Реализовать первый slice UI redesign: shell, sidebar и chat list
- Провести baseline measurements startup time, RAM, idle CPU и scrolling
- После каждого блока cleanup выполнять Debug build и smoke test

### v0.3 — Power Tools

- Расширить Command Palette: fuzzy search, `files`, `media`, `mute`, `saved`
- Добавить Local Notes для пользователей и чатов
- Добавить Copy as Markdown, Copy link и Search messages from sender
- Доработать UI chat header, message bubbles, input и context menus

### v0.4 — Privacy

- Добавить пользовательские storage controls
- Провести threat model, logging cleanup и local storage review
- Доработать deleted messages и edit history с понятным обозначением локальных
  данных и возможностью отключить хранение

### v0.5 — Beta

- Завершить UI polish и accessibility pass
- Оптимизировать только подтверждённые profiling bottlenecks
- Провести regression, upgrade и multi-platform testing
- Подготовить собственный безопасный update channel

### v1.0

NexFort достаточно стабилен, чтобы использовать его как основной
Telegram-клиент; задокументированы compatibility, privacy, update и upstream
процессы.

## Рабочие правила

- Каждое изменение проходит через issue, отдельную branch, Debug verification,
  pull request и review второго участника.
- Не объединять cleanup, branding, feature work и unrelated formatting в один
  commit.
- Не merge-ить upstream вслепую; отдельно отслеживать security fixes и после
  интеграции выполнять regression tests.
- Сначала измерять производительность, затем оптимизировать.
- Security-sensitive изменения требуют review второго участника.

## Current Priority

1. Debug baseline и smoke test
2. Проверка platform identity и release safety
3. Временная иконка `NF` и завершение branding
4. Command Palette shell и shortcut
5. Команды `settings` и `search <query>`
6. Команды `open @username` и `open <chat>`
7. Privacy, regression и multi-scale checks
8. Публикация прототипа через GitHub Releases
