# Lock Matrix: Known File Locking Daemons & Recovery

<!-- last-verified: 2026-09-05 -->

Patterns for identifying background processes locking developer tool directories, and procedures for releasing handles safely.

## Common Locking Daemons in `.vscode/extensions`

| Process Name | Associated Extension | Typical Spawned Command |
| :--- | :--- | :--- |
| `codex-code-mode-host.exe` | OpenAI ChatGPT / Codex | `<ext>\bin\windows-x86_64\codex-code-mode-host.exe` |
| `codex.exe` | OpenAI ChatGPT / Codex | `<ext>\bin\windows-x86_64\codex.exe app-server` |
| `kilo.exe` | Kilo Code | `<ext>\bin\kilo.exe serve --port 0` |
| `pylance` / `python` | MS Python / Pylance | Language server host worker |
| `intelephense` | Intelephense PHP | Node.js language server host |
| `gopls.exe` / `gopls` | Go Language Server / MCP | `<path>\gopls.exe` (locks `$env:LOCALAPPDATA\go-build`) |
| `tsserver.exe` / `vtsls` | TypeScript Language Server | Background IDE AST worker |
| `rust-analyzer` | Rust Language Server | Cargo workspace indexer |

---

## Lock Discovery Commands

### Windows (PowerShell)
To find processes locking a specific folder path:
```powershell
$targetPath = "C:\path\to\target"
Get-CimInstance Win32_Process | Where-Object { 
    $_.CommandLine -like "*$targetPath*" -or $_.ExecutablePath -like "*$targetPath*" 
} | Select-Object ProcessId, Name, ParentProcessId, CommandLine
```

### Linux / macOS (POSIX)
To find processes locking a file or directory:
```bash
lsof +D "/path/to/target"
# Or
fuser -v "/path/to/target"
```

---

## Safe Termination Procedure

1. Verify the process is a background worker/daemon and not a primary user document editor.
2. Terminate the child process by PID:
   - Windows: `Stop-Process -Id <PID> -Force`
   - Linux/macOS: `kill -9 <PID>`
3. Wait 1 to 2 seconds for the operating system file system handle table to release locks before issuing file deletion commands.

---

## Safe Deletion Execution Pattern (PowerShell & POSIX)

When executing bulk cache purges across directories where transient file locks may exist:

### Windows (PowerShell)
1. **Never use `-LiteralPath "$targetPath\*"` with wildcards**: PowerShell's `-LiteralPath` parameter treats `*` as a literal character name, causing silent deletion failures.
2. **Iterate child items with granular error handling**:
   ```powershell
   function Remove-FolderContentsSafe([string]$targetPath) {
       if (-not (Test-Path -LiteralPath $targetPath)) { return 0 }
       $items = Get-ChildItem -LiteralPath $targetPath -Force -ErrorAction SilentlyContinue
       if (-not $items) { return 0 }
       $freedBytes = 0
       foreach ($item in $items) {
           try {
               if (-not $item.PSIsContainer) {
                   $len = $item.Length
                   Remove-Item -LiteralPath $item.FullName -Force -ErrorAction Stop
                   $freedBytes += $len
               } else {
                   $subFiles = Get-ChildItem -LiteralPath $item.FullName -Recurse -File -Force -ErrorAction SilentlyContinue
                   $subBytes = ($subFiles | Measure-Object -Property Length -Sum).Sum
                   Remove-Item -LiteralPath $item.FullName -Recurse -Force -ErrorAction Stop
                   if ($subBytes) { $freedBytes += $subBytes }
               }
           } catch {
               # Locked by an active process - safely bypassed without halting
           }
       }
       return $freedBytes
   }
   ```
3. **Shell String Escaping Discipline**: When running via `pwsh -Command`, use single-quoted script blocks `'...'` or write to a temporary `.ps1` file to avoid premature outer shell expansion of `$_` and `$matches`.
4. **Verified Delta Metric**: Always record `(Get-PSDrive <Drive>).Free` before and after execution to report confirmed net disk gain.

### Linux / macOS (POSIX)
1. Delete directory contents while keeping top-level container:
   ```bash
   find "$target_path" -mindepth 1 -maxdepth 1 -exec rm -rf {} + 2>/dev/null
   ```
2. Measure volume delta via `df -k "$target_path"`.

---

## Language Server & Build Tool Lock Resolution Discipline

When language package cleaners (e.g. `go clean -cache`, `cargo clean`, `npm cache clean`) fail with `Access is denied` or POSIX `unlinkat: Permission denied`:

1. **Root Cause**: An active language server daemon (e.g. `gopls.exe`, `tsserver`, `intelephense`) or MCP background server has loaded a cached executable or dynamic library into active memory.
2. **Lock Discovery**:
   - Windows:
     ```powershell
     Get-Process gopls, tsserver, intelephense, rust-analyzer -ErrorAction SilentlyContinue | Select-Object Name, Id, Path
     ```
   - Linux / macOS:
     ```bash
     pgrep -fl "gopls|tsserver|intelephense|rust-analyzer"
     ```
3. **Resolution Policy**:
   - Never terminate active editor processes (`Code.exe`, `nvim`, `antigravity`) directly.
   - Gracefully offer to stop only the specific background worker daemon PID, wait 1-2 seconds for handle release, and re-run the clean command.
   - If user chooses not to terminate the daemon: bypass the single locked binary safely and purge all remaining unallocated cache objects without failing the entire cleanup task.
