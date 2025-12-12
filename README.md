# WinDBG Cheat Sheet

# Setup

| Description       | Link |
|:------------------|:-----|
| **App Store:**    | https://apps.microsoft.com/detail/9pgjgd53tn86?hl=en-US&gl=US |
| **Command Line:** | `winget install Microsoft.WinDbg` |
| **Wiki:**         | https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/ |

`/!\` **NOTE:** Must set environment variable `_NT_SYMBOL_PATH` before starting:

* `setx _NT_SYMBOL_PATH SRV*D:\cache\windbg_symbol_server*http://msdl.microsoft.com/download/symbols`
* `for /f "tokens=3 delims=% " %i in ('reg query HKCU\Environment /v _NT_SYMBOL_PATH^| findstr /i /c:"_NT_SYMBOL_PATH"') do set _NT_SYMBOL_PATH=%i`

**Run x64 version** from the command line (**Admin Mode):**

```
explorer shell:appsFolder\Microsoft.WinDbg_8wekyb3d8bbwe!Microsoft.WinDbg
```

`windbg.exe` is installed to:

* x64: `C:\Program Files\WindowsApps\Microsoft.WinDbg_1.2511.21001.0_x64__8wekyb3d8bbwe`
* x86: `C:\Program Files (x86)\Windows Kits\10\Debuggers\x64`
* x86: `C:\Program Files (x86)\Windows Kits\10\Debuggers\x86`

## Setup Misc.

* `Get-AppxPackage Microsoft.WinDbg`
* `find "Application Id" "C:\Program Files\WindowsApps\Microsoft.WinDbg_1.2511.21001.0_x64__8wekyb3d8bbwe\AppxManifest.xml"`

```
    <Application Id="Microsoft.WinDbg.CdbX64" Executable="amd64\cdb.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.CdbX86" Executable="x86\cdb.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.CdbARM64" Executable="arm64\cdb.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.KdX64" Executable="amd64\kd.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.KdX86" Executable="x86\kd.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.KdARM64" Executable="arm64\kd.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.NtsdX64" Executable="amd64\ntsd.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.NtsdX86" Executable="x86\ntsd.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.NtsdARM64" Executable="arm64\ntsd.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.DbgSrvX64" Executable="amd64\dbgsrv.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.DbgSrvX86" Executable="x86\dbgsrv.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg.DbgSrvARM64" Executable="arm64\dbgsrv.exe" EntryPoint="Windows.FullTrustApplication">
    <Application Id="Microsoft.WinDbg" Executable="DbgX.Shell.exe" EntryPoint="Windows.FullTrustApplication">
```

**Open "appsFolder" in Explorer:**

```
explorer.exe shell:appsfolder
explorer.exe shell:::{4234d49b-0245-4df3-B780-3893943456e1}
```

# Help

```
start hh.exe "C:\Program Files (x86)\Windows Kits\10\Debuggers\x86\debugger.chm"
```

To open HTML Help at a specific page view that page, **Right-Click > Properties > Address** to get the URL, then copy and paste into a command line:

Debugger Commands:

```
start hh.exe "mk:@MSITStore:C:\Program%20Files%20(x86)\Windows%20Kits\10\Debuggers\x86\debugger.chm::/debugger/debugger_commands.htm"`
```

Meta Commands:

```
start hh.exe "mk:@MSITStore:C:\Program%20Files%20(x86)\Windows%20Kits\10\Debuggers\x86\debugger.chm::/debugger/meta_commands.htm"`
```

| Command         | Description |
|:----------------|:------------|
| `help`          | Show available command                 |
| `.hh <command>` | Show (HTML) help for specified command |
| `!help`         | List debugger extensions               |

# Breakpoints ::/debugger/bp__bu__bm__set_breakpoint_.htm

`[~Thread] ba[ID] Access Size [Options] [Address [Passes]] ["CommandString"]`

| Command            | Description |
|:-------------------|:------------|
| `ba e 1`           | Data breakpoint on Execute. Size MUST be 1 |
| `ba r <size>`      | Data breakpoint on Read 1, 2, 4      |
| `ba w <size>`      | Data breakpoint on Write 1, 2, 4     |
| `ba i <size>`      | Data breakpoint on Kernel IO 1, 2, 4 |
| `bc #`             | Clear Breakpoint                     |
| `bd #`             | Disable Breakpoint                   |
| `be`#`             | Enable Breakpoint                    |
| `bl`               | List Breakpoints                     |
| `bp <address>`     | Set breakpoint                       |
| `bp <file>!<func>` | Set breakpoint. ie. `bp notepad!main` `bp notepad!wWinMain` |
| `bu`               | Set unresolved breakpoint   |
| `bm`               | Set symbol breakpoint       |

# Command-Line WinDBG ::/debugger/command_line_options.htm

| CMD.exe | Description |
|:------------|:------------|
| `-c <command>`    | Specify command to run at startup |
| `-srcpath <path>` | Path to source code               |

# Command-Line Process -- 

| Command     | Description |
|:------------|:------------|
| `vercommand`| Show command-line of debugger  |
| `vertarget` | Show Windows version of target |

# Disassembly ::/debugger/u__unassemble_.htm

| Command       | Description |
|:--------------|:------------|
| `u @rip`      | Unassemble next 8 instructions                 |
| `ub @rip`     | Unassemble prev 8 instructions                 |
| `ub @rip L##` | Unassemble # instructions                      |
| `uf @rip`     | Unassemble function                            |
| `uf /c @rip`  | Unassemble all function calls a function makes |

# Execution

* `g` continue execution

# Information

| Command     | Description |
|:------------|:------------|
| `!cpuid`    | Show CPU Family, Model, Stepping |

# Memory

| Command     | Description |
|:------------|:------------|
| `db`        | Display as 8-bit bytes                        |
| `dw`        | Display as 16-bit words                       |
| `dd`        | Display as 32-bit double-words                |
| `dq`        | Display as 64-bit ints                        |
| `dp @<reg>` | Display register as pointer. `rsp`, `rbp`     |
| `dp /c #`   | Display pointer with # columns (instead of 2) |
| `dp @rsp`   | Display pointer from stack pointer register   |
| `dps @<reg>`| Display memory as symbol functions. i.e. `dps @rsp` |

# Process, Threads, Modules

| Command     | Description |
|:------------|:------------|
| `|`           | Show PID                        |
| `~`           | Show Threads                    |
| `lm`          | List Modules                    |
| `lm m <file>` | Show start end address for game |
| `lm v <file>` | Show file version               |
| `? <file>`    | Show start address of the game  |

# Registers

| Command     | Description |
|:------------|:------------|
| `r`          | View all registers          |
| `r eax`      | View specified register     |
| `r eax=0x123`| Set register to hex literal |

# Stack

| Command     | Description |
|:------------|:------------|
| `k`         | Show Backtrace                         |
| `kn`        | show Backtrace with stack depth        |
| `kc`        | Show Backtrace clean (names only)      |
| `kv`        | Show Backtrace with function arguments |
| `ln <addr>` | List Nearest symbol functions          |

# Symbols

| Admin CMD.exe | Description |
|:------------|:------------|
| `md - D:\cache\windbg_symbol_server` | Create path for symbol cache directory |
| `setx _NT_SYMBOL_PATH SRV*D:\cache\windbg_symbol_server*http://msdl.microsoft.com/download/symbols` | Set symbol search path to custom path `D:\cache\windbg_symbol_server` |
| `reg query HKCU\Environment | findstr /i /c:"_NT_SYMBOL_PATH"` | Display symbol search path |
| `for /f "tokens=3 delims=% " %%i in ('reg query HKCU\Environment /v _NT_SYMBOL_PATH^| findstr /i /c:"_NT_SYMBOL_PATH"') do set FOO=%%i` | Set `Foo` with current `_NT_SYMBOL_PATH` value |

| Command     | Description |
|:------------|:------------|
| `.reload`         | Reload symbols |
| `x <game>!<func>` | List executable functions. i.e. `x notepad!*main` or `x Notepad!wWinMain` |
| `x /a`            | List in Ascending order of addresses |
| `x /d`            | List in ? |
| `.sympath`        | To view symbol path |
| `!lmi <exename>`  | To view path to .pdb |

# References

* [WinDBG quick start tutorial](https://codemachine.com/articles/windbg_quickstart.html)
* [Comprehensive Guide to Using WinDbg (Windows Debugger)](https://gist.github.com/MangaD/3bbeae1b326351b2728d856fb5cd651c)

---

Last updated Dec. 12, 2025 by Michaelangel007
