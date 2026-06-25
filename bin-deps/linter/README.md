# Linter binary dependency

This directory contains the bootstrap script for the Lua linter binary used by
the project.

## `install_selene.py`

Builds and installs the pinned Selene release:

```powershell
python bin-deps\linter\install_selene.py
```

The script:

- clones `https://github.com/Kampfkarren/selene.git` at tag `0.31.0`;
- uses `_selene_build_src` in this directory as a temporary build directory;
- patches Selene's binary crate manifest before building;
- runs `cargo build --release -p selene`;
- copies the built executable to `bin/selene.exe`;
- removes the temporary build directory, including read-only files under `.git`.

## Dependencies

- `git` in `PATH`;
- `cargo` in `PATH`, or at `%USERPROFILE%\.cargo\bin\cargo.exe`.

## Selene rebuild patch

Selene 0.31.0's binary crate defaults to the Roblox/Luau build:

```toml
[features]
default = ["roblox"]
tracy-profiling = ["profiling/profile-with-tracy", "tracy-client"]
roblox = ["selene-lib/roblox", "full_moon/roblox", "ureq"]
```

For this project, Roblox support is unnecessary. LuaCs needs Lua 5.2 syntax
support instead. `install_selene.py` changes the manifest to:

```toml
[features]
default = ["lua52"]
tracy-profiling = ["profiling/profile-with-tracy", "tracy-client"]
roblox = ["selene-lib/roblox", "full_moon/roblox", "ureq"]
lua52 = ["selene-lib/lua52", "full_moon/lua52"]
```

This disables the default Roblox feature for the built binary and enables Lua
5.2 support through both Selene's lint library and the `full_moon` parser.
