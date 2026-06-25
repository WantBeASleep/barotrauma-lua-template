# Selene std generator

This directory contains the generator for Selene standard library YAML files used by the LuaCs lint configuration.

## `generate_selene_luacs_std.py`

Generates Selene standard library YAML files from `Barotrauma-Lua-Annotations` type definitions.

```powershell
python tools\generate\selene_std\generate_selene_luacs_std.py --side client
python tools\generate\selene_std\generate_selene_luacs_std.py --side server
```

By default the script expects the annotations repository next to this mod:

```text
../Barotrauma-Lua-Annotations
```

The generated outputs are:

- `selene_std_luacs_client.yml` from `Library/Client`;
- `selene_std_luacs_server.yml` from `Library/Server`.

Use `--annotations-root <path>` when the annotations repository is elsewhere, and `--output <path>` to write a custom YAML file.

The script intentionally uses the annotation files as the source of truth, rather than scanning this mod's current Lua code. It converts declared globals, tables, and simple function parameter counts to Selene's std format. Complex Barotrauma C# userdata and generated API surfaces are kept permissive with `any: true`, because Selene's std format is not a full LuaLS or C# type system.
