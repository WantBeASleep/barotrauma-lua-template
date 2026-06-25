# Barotrauma Lua Mod Layout

**Language:** English | [Русский](README.ru.md)

A base repository template for Barotrauma Lua mods built with [LuaCsForBarotrauma](https://github.com/evilfactory/LuaCsForBarotrauma).

The template includes a minimal mod structure, `filelist.xml`, a VS Code workspace, local StyLua/Selene binaries, and helper agent skills for Barotrauma mod development.

## Quick Start

1. Copy or fork this repository for a new mod.
2. Rename the workspace file in `.vscode/Your project name.code-workspace`. Using a `DEV-` prefix is recommended if you plan to build a separate Workshop version later.
3. Update `filelist.xml`: set your mod name in the `name` attribute.
4. Fill in `AGENTS.md`: describe the project, its structure, and rules for assistants/agents.
5. Put Lua code in `Lua/<namespace>/<your-project>/...`, and keep the entrypoint or loader under `Lua/Autorun`.
6. If you use VS Code, clone [Barotrauma-Lua-Annotations](https://github.com/zhu-rengong/Barotrauma-Lua-Annotations) next to this project:

```text
LocalMods/
├─ your-project/
└─ Barotrauma-Lua-Annotations/
```

## Project Structure

```text
.
├─ .codex/                       # Skills for AI agents
│  └─ skills/
│     ├─ barotrauma-modding/      # XML, content packages, StatusEffects
│     ├─ luacs-barotrauma/        # LuaCs hooks, networking, runtime Lua
│     ├─ barotrauma-item-art/     # Icons, sprites, Barotrauma-style art
│     └─ python-script-authoring/ # Python utility script guidance
├─ .vscode/                       # VS Code workspace with Lua annotations
├─ assets/                        # Runtime assets used by the game
├─ bin/                           # Local tool binaries
├─ bin-deps/                      # Scripts for installing/building binaries
├─ Lua/                           # Mod Lua code
│  └─ Autorun/                    # LuaCs autorun entrypoints
├─ preview/                       # Preview images and Workshop artwork
├─ source/                        # Source assets: images, fonts, etc.
├─ tools/                         # Project Python scripts
│  ├─ format/                     # StyLua runner
│  ├─ lint/                       # Selene runner
│  └─ generate/                   # Project file generators
│     └─ selene_std/              # Selene std generation from Lua annotations
├─ .stylua.toml                   # StyLua configuration
├─ .styluaignore                  # StyLua ignore paths
├─ AGENTS.md                      # Instructions for AI agents
├─ filelist.xml                   # Root Barotrauma content package
├─ selene.toml                    # Selene configuration
├─ selene_std_luacs_client.yml    # Selene std for the client LuaCs API
└─ selene_std_luacs_server.yml    # Selene std for the server LuaCs API
```

## Lua Code

LuaCs loads files from `Lua/`, but larger mods are easier to maintain when project code lives under a namespace:

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

For example: `Lua/yourname/my_mod/logic.lua`. This layout reduces conflicts with other mods and is easier to move between projects.

Documentation:

- [LuaCsForBarotrauma docs](https://evilfactory.github.io/LuaCsForBarotrauma/lua-docs/index.html)
- [LuaCsForBarotrauma repository](https://github.com/evilfactory/LuaCsForBarotrauma)
- [Barotrauma modding docs](https://regalis11.github.io/BaroModDoc/)

## VS Code And Lua Annotations

The workspace in `.vscode/` is configured for [Lua Language Server](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) and type definitions from [Barotrauma-Lua-Annotations](https://github.com/zhu-rengong/Barotrauma-Lua-Annotations).

By default, the workspace expects annotations next to this project:

```text
../Barotrauma-Lua-Annotations/Library/Client
```

For server-side code, switch the library to:

```text
../Barotrauma-Lua-Annotations/Library/Server
```

## Lua Formatting

The project uses [StyLua](https://github.com/JohnnyMorganz/StyLua) with `syntax = "Lua52"` in `.stylua.toml`.

Check formatting without rewriting files:

```powershell
python tools\format\run_stylua.py --check
```

Format Lua files in place:

```powershell
python tools\format\run_stylua.py
```

The script runs the local `bin/stylua.exe` binary against the `Lua/` directory.

To download StyLua again:

```powershell
python bin-deps\formatter\install_stylua.py
```

## Lua Linting

The project uses [Selene](https://github.com/Kampfkarren/selene) with Lua 5.2 and std files generated from `Barotrauma-Lua-Annotations`.

Run lint:

```powershell
python tools\lint\run_selene.py
```

Regenerate std files:

```powershell
python tools\generate\selene_std\generate_selene_luacs_std.py --side client
python tools\generate\selene_std\generate_selene_luacs_std.py --side server
```

By default, the generator looks for annotations at:

```text
../Barotrauma-Lua-Annotations
```

If annotations are elsewhere, pass the path explicitly:

```powershell
python tools\generate\selene_std\generate_selene_luacs_std.py --side client --annotations-root <path>
```

To rebuild Selene:

```powershell
python bin-deps\linter\install_selene.py
```

Rebuilding requires `git` and `cargo` from the Rust toolchain. The included `bin/selene.exe` is built for Windows x64.

## Agent Skills

The `.codex/skills` directory contains local instructions for AI agents. They are not limited to Codex: rename or move the directory if your tool expects a different layout.

Contents:

- `barotrauma-modding` - XML, content packages, overrides, StatusEffects, and official Barotrauma documentation.
- `luacs-barotrauma` - LuaCs hooks, client/server runtime logic, networking, and Barotrauma API access from Lua.
- `barotrauma-item-art` - creating and validating Barotrauma-style icons and sprites.
- `python-script-authoring` - rules for project Python scripts.

## Steam Workshop Publishing

For publishing, it is better to build a separate Workshop version and copy only files required by the mod at runtime:

- `filelist.xml`;
- `Lua/`;
- runtime assets from `assets/`;
- preview files, if needed for publishing.

Do not publish source files, agent skills, `.vscode`, `bin-deps`, or temporary development files.

Example Workshop build script: [barotrauma-medical-icons/tools/workshop_build](https://github.com/WantBeASleep/barotrauma-medical-icons/tree/master/tools/workshop_build).

## What To Configure For Your Mod

- `filelist.xml` - replace `name`, `modversion`, and `gameversion` if needed.
- `.vscode/Your project name.code-workspace` - rename the file and the `folders.name` field.
- `AGENTS.md` - replace the template with a description of the actual project.
- `Lua/Autorun/main.lua` - load your runtime modules.
- `assets/`, `source/`, `preview/` - fill with your own files.
- `selene.toml` - choose the client or server std for the mod side.
- `.styluaignore` - add generated Lua files or other Lua paths that should not be formatted automatically.

## What Can Be Removed

After configuring the project, you can remove template parts that your mod does not need:

- `.codex/` - if you do not use agent skills.
- `.vscode/` - if you do not use VS Code.
- `bin-deps/` - if you do not plan to download or rebuild StyLua and Selene.
- `source/` - if you do not store source assets in the repository.
- `preview/` - if preview files are stored elsewhere.
- `tools/generate/selene_std/` - if the std files already fit your needs and you do not plan to update them from annotations.

Do not remove `bin/`, `tools/format/`, or `tools/lint/` if you want to keep using the local formatting and lint commands.
