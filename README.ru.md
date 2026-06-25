# Barotrauma Lua Mod Template

**Язык:** [English](README.md) | Русский

Базовый репозиторий-шаблон для Lua-модов Barotrauma через [LuaCsForBarotrauma](https://github.com/evilfactory/LuaCsForBarotrauma).

Шаблон уже содержит минимальную структуру мода, `filelist.xml`, workspace для VS Code, локальные бинарники StyLua/Selene и вспомогательные agent skills для разработки Barotrauma-модов.

## Быстрый старт

1. Скопируйте или форкните этот репозиторий под новый мод.
2. Переименуйте workspace в `.vscode/Your project name.code-workspace`. Рекомендуется использовать префикс `DEV-`, чтобы позже отдельно собирать workshop-версию.
3. Обновите `filelist.xml`: задайте свое имя мода в атрибуте `name`.
4. Заполните `AGENTS.md`: кратко опишите проект, его структуру и правила для ассистентов/агентов.
5. Положите Lua-код в `Lua/<namespace>/<your-project>/...`, а точку входа или загрузчик держите под `Lua/Autorun`.
6. Если вы используете VS Code, клонируйте [Barotrauma-Lua-Annotations](https://github.com/zhu-rengong/Barotrauma-Lua-Annotations) рядом с этим проектом:

```text
LocalMods/
├─ your-project/
└─ Barotrauma-Lua-Annotations/
```

## Структура проекта

```text
.
├─ .codex/                       # Skills для AI-агентов
│  └─ skills/
│     ├─ barotrauma-modding/      # XML, content packages, StatusEffects
│     ├─ luacs-barotrauma/        # LuaCs hooks, networking, runtime Lua
│     ├─ barotrauma-item-art/     # Иконки, sprites, Barotrauma-style art
│     └─ python-script-authoring/ # Python-утилиты для проекта
├─ .vscode/                       # VS Code workspace с Lua annotations
├─ assets/                        # Готовые runtime assets для игры
├─ bin/                           # Локальные бинарники инструментов
├─ bin-deps/                      # Скрипты установки/сборки бинарников
├─ Lua/                           # Lua-код мода
│  └─ Autorun/                    # LuaCs autorun entrypoints
├─ preview/                       # Превью, логотипы и картинки для Workshop
├─ source/                        # Исходники ассетов: изображения, шрифты и т.п.
├─ tools/                         # Python-скрипты проекта
│  ├─ format/                     # Запуск StyLua
│  ├─ lint/                       # Запуск Selene
│  └─ generate/                   # Генераторы проектных файлов
│     └─ selene_std/              # Генерация Selene std из Lua annotations
├─ .stylua.toml                   # Конфигурация StyLua
├─ .styluaignore                  # Исключения для StyLua
├─ AGENTS.md                      # Инструкции для AI-агентов
├─ filelist.xml                   # Корневой content package Barotrauma
├─ selene.toml                    # Конфигурация Selene
├─ selene_std_luacs_client.yml    # Selene std для клиентского LuaCs API
└─ selene_std_luacs_server.yml    # Selene std для серверного LuaCs API
```

## Lua-код

LuaCs загружает файлы из `Lua/`, но для крупных модов удобнее держать код в пространстве имен:

```text
Lua/
├─ Autorun/
│  └─ main.lua
└─ <namespace>/
   └─ <your-project>/
      ├─ logic.lua
      ├─ config.lua
      └─ ui.lua
```

Например, `Lua/yourname/my_mod/logic.lua`. Такой template снижает риск конфликтов с другими модами и проще переносится между проектами.

Документация:

- [LuaCsForBarotrauma docs](https://evilfactory.github.io/LuaCsForBarotrauma/lua-docs/index.html)
- [LuaCsForBarotrauma repository](https://github.com/evilfactory/LuaCsForBarotrauma)
- [Barotrauma modding docs](https://regalis11.github.io/BaroModDoc/)

## VS Code и Lua annotations

Workspace в `.vscode/` настроен под расширение [Lua Language Server](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) и type definitions из [Barotrauma-Lua-Annotations](https://github.com/zhu-rengong/Barotrauma-Lua-Annotations).

По умолчанию workspace ожидает annotations рядом с проектом:

```text
../Barotrauma-Lua-Annotations/Library/Client
```

Для серверного кода можно переключить библиотеку на:

```text
../Barotrauma-Lua-Annotations/Library/Server
```

## Форматирование Lua

Проект использует [StyLua](https://github.com/JohnnyMorganz/StyLua) с `syntax = "Lua52"` в `.stylua.toml`.

Проверить форматирование без изменений:

```powershell
python tools\format\run_stylua.py --check
```

Отформатировать Lua-файлы:

```powershell
python tools\format\run_stylua.py
```

Скрипт запускает локальный бинарник `bin/stylua.exe` для папки `Lua/`.

Если нужно заново скачать StyLua:

```powershell
python bin-deps\formatter\install_stylua.py
```

## Линт Lua

Проект использует [Selene](https://github.com/Kampfkarren/selene) с Lua 5.2 и std-файлами, сгенерированными из `Barotrauma-Lua-Annotations`.

Запустить lint:

```powershell
python tools\lint\run_selene.py
```

Сгенерировать std-файлы заново:

```powershell
python tools\generate\selene_std\generate_selene_luacs_std.py --side client
python tools\generate\selene_std\generate_selene_luacs_std.py --side server
```

По умолчанию генератор ищет annotations в:

```text
../Barotrauma-Lua-Annotations
```

Если annotations лежат в другом месте, передайте путь явно:

```powershell
python tools\generate\selene_std\generate_selene_luacs_std.py --side client --annotations-root <path>
```

Если нужно пересобрать Selene:

```powershell
python bin-deps\linter\install_selene.py
```

Для пересборки нужен `git` и `cargo` из Rust toolchain. Готовый `bin/selene.exe` в этом шаблоне рассчитан на Windows x64.

## Agent skills

Папка `.codex/skills` содержит локальные инструкции для AI-агентов. Их можно использовать не только в Codex: при необходимости переименуйте или перенесите папку под формат своего инструмента.

Содержимое:

- `barotrauma-modding` - работа с XML, content packages, overrides, StatusEffects и официальной документацией Barotrauma.
- `luacs-barotrauma` - LuaCs hooks, client/server runtime logic, networking и API Barotrauma из Lua.
- `barotrauma-item-art` - создание и проверка иконок/спрайтов в стиле Barotrauma.
- `python-script-authoring` - правила для проектных Python-скриптов.

## Публикация в Steam Workshop

Для публикации лучше собирать отдельную workshop-версию и переносить в нее только файлы, необходимые для работы мода в игре:

- `filelist.xml`;
- `Lua/`;
- runtime assets из `assets/`;
- preview-файлы, если они нужны для публикации.

Не стоит публиковать исходники, agent skills, `.vscode`, `bin-deps` и временные файлы разработки.

Пример отдельного build-скрипта для workshop-версии: [barotrauma-medical-icons/tools/workshop_build](https://github.com/WantBeASleep/barotrauma-medical-icons/tree/master/tools/workshop_build).

## Что настроить под свой мод

- `filelist.xml` - заменить `name`, `modversion` и при необходимости `gameversion`.
- `.vscode/Your project name.code-workspace` - переименовать файл и поле `folders.name`.
- `AGENTS.md` - заменить template на описание конкретного проекта.
- `Lua/Autorun/main.lua` - подключить ваши runtime-модули.
- `assets/`, `source/`, `preview/` - заполнить своими файлами.
- `selene.toml` - выбрать client или server std под тип мода.
- `.styluaignore` - добавить generated Lua-файлы или другие Lua-пути, которые не нужно форматировать автоматически.

## Что можно удалить

После настройки проекта можно удалить части шаблона, которые не нужны вашему моду:

- `.codex/` - если не используете agent skills.
- `.vscode/` - если не работаете в VS Code.
- `bin-deps/` - если не планируете скачивать/пересобирать StyLua и Selene.
- `source/` - если не храните исходники ассетов в репозитории.
- `preview/` - если preview-файлы хранятся отдельно.
- `tools/generate/selene_std/` - если std-файлы уже подходят и вы не собираетесь обновлять их из annotations.

Не удаляйте `bin/`, `tools/format/` и `tools/lint/`, если хотите продолжать использовать локальные команды форматирования и линта.
