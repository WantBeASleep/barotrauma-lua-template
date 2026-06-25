# Formatter binary dependency

This directory contains the bootstrap script for the Lua formatter binary used
by the project.

## `install_stylua.py`

Downloads and installs the pinned StyLua release:

```powershell
python bin-deps\formatter\install_stylua.py
```

The script:

- downloads StyLua `2.5.2` from the official GitHub release archive;
- creates `bin/` if it does not already exist;
- extracts `stylua.exe` from the downloaded archive;
- copies the executable to `bin/stylua.exe`;
- removes its temporary `_stylua_download` directory.

No external project files are required beyond the StyLua release archive. The
script resolves all paths from its own location and the project root.

## Dependencies

none
