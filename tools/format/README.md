# Lua format tools

This directory contains helper scripts for running StyLua for the LuaCs runtime
code in `Lua/`.

The project uses StyLua with `syntax = "Lua52"` to match the LuaCs/MoonSharp
syntax mode used by Selene.

## Formatter binary

```powershell
python bin-deps\formatter\install_stylua.py
```

The dependency bootstrap script lives in `bin-deps/formatter/`. It downloads
the pinned StyLua release and installs `bin/stylua.exe`.

## `run_stylua.py`

Runs the local StyLua binary against the project's Lua runtime files.

Check formatting without rewriting files:

```powershell
python tools\format\run_stylua.py --check
```

Format files in place:

```powershell
python tools\format\run_stylua.py
```

The script:

- finds the project root from its own location;
- expects StyLua at `bin/stylua.exe`;
- runs StyLua against `Lua/`;
- always passes `--verify`;
- returns StyLua's exit code.
