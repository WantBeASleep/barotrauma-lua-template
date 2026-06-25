# Project Notes

## Overview

TODO PROJECT: Replace this section for each real mod before active development.

- Project name: TODO PROJECT: `Your project name`
- Mod side: TODO PROJECT: choose `client`, `server`, or `client-server`
- Short description: TODO PROJECT: describe what the mod does in 1-3 sentences
- Main gameplay/domain area: TODO PROJECT: examples: medical items, submarine systems, UI, events, creatures, balance

## Project Structure

The project is structured as a Barotrauma Lua mod workspace.

```text
.
├─ assets/                        # Runtime assets loaded by the game
├─ Lua/                           # Runtime Lua files loaded by LuaCs
│  └─ Autorun/                    # LuaCs autorun entrypoints
├─ preview/                       # Workshop preview/logo images
├─ source/                        # Source assets such as layered images, fonts, references
├─ tools/                         # Project utility scripts
└─ filelist.xml                   # Barotrauma content package definition
```

TODO PROJECT: If the mod adds folders, generated files, build outputs, or a custom namespace under `Lua/`, document them here.

## Lua Structure

Keep Lua runtime code under `Lua/`. For project code, prefer a namespace structure:

```text
Lua/
├─ Autorun/
│  └─ main.lua
└─ TODO_NAMESPACE/
   └─ TODO_PROJECT_SLUG/
      ├─ config.lua
      ├─ logic.lua
      └─ ui.lua
```

TODO PROJECT: Replace `TODO_NAMESPACE` and `TODO_PROJECT_SLUG` with the real namespace and module folder names before active development.

Use `Lua/Autorun/main.lua` as the entrypoint or loader for project modules. Avoid placing large mod logic directly in `Autorun` files when it can live under the namespaced folder.

## Lua Version

Lua runtime code targets Lua 5.2 syntax through LuaCsForBarotrauma/MoonSharp.

- Use Lua 5.2-compatible syntax and standard library behavior.
- Do not assume LuaJIT, Lua 5.3+, or Roblox/Luau features.
- Keep client-only and server-only API usage separated when the mod supports both sides.
- When in doubt about LuaCs hooks, networking, or Barotrauma API access, check LuaCsForBarotrauma documentation or local examples before changing runtime behavior.

## Formatting

The canonical Lua format command is:

```powershell
python tools\format\run_stylua.py
```

Check formatting without rewriting files:

```powershell
python tools\format\run_stylua.py --check
```

Run the format workflow after changing:

- Lua runtime files under `Lua/`;
- StyLua configuration files such as `.stylua.toml` or `.styluaignore`;
- formatting helper scripts under `tools/format/`.

Generated Lua files that should not be formatted automatically must be listed in `.styluaignore`.

## Linting

The canonical Lua lint command is:

```powershell
python tools\lint\run_selene.py
```

Run the lint workflow after changing:

- Lua runtime files under `Lua/`;
- Selene configuration files such as `selene.toml`;
- generated Selene std files such as `selene_std_luacs_client.yml` or `selene_std_luacs_server.yml`;
- lint helper scripts under `tools/lint/`.

Selene is already configured for Lua 5.2 and LuaCs globals through generated std files.

## Build And Publishing Workflow

TODO PROJECT: Replace this section with the real build/publishing workflow before release work begins.

Recommended baseline:

- Keep development-only files out of Steam Workshop builds.
- Publish only files required by the game, such as `filelist.xml`, `Lua/`, runtime assets from `assets/`, and required preview files.
- If the project adds a workshop build script, document the command here.

## Agent Guidelines

- Agents must never create git commits in this repository unless the user explicitly asks for a commit.
- Store generated Lua runtime files under a clear generated folder and add that folder to `.styluaignore` when formatting would be unsafe.
- Scripts must not hard-code absolute paths. Compute paths from the current script, project root, or known Barotrauma directory structure.
- Scripts must not depend on external files outside the Barotrauma install/project tree. If a script needs support files such as fonts, images, templates, or references, copy those files into the appropriate project folder and load them from there.
- Before editing Lua runtime behavior, check the relevant LuaCs or Barotrauma documentation when API names, hooks, networking, XML attributes, or side-specific behavior are uncertain.
