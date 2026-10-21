# Safety Matrix: Browser Data Protection & Invariants

<!-- last-verified: 2026-09-05 -->

Classification of browser internal directories into safe regenerable caches versus strictly protected user databases.

## Chromium / Chrome / Edge Profile Data Map

### 1. Invariant: Strictly Protected (NEVER DELETE)

| File / Directory | Purpose | Risk if Deleted |
| :--- | :--- | :--- |
| `Login Data` / `Login Data-journal` | Saved passwords and credentials | **CRITICAL**: User loses all saved logins |
| `Cookies` / `Cookies-journal` | Active website authentication sessions | **HIGH**: Logs user out of all active web sessions |
| `Bookmarks` / `Bookmarks.bak` | Saved user bookmarks and hierarchy | **CRITICAL**: Complete loss of user bookmarks |
| `Preferences` / `Secure Preferences` | Profile configuration and UI state | **HIGH**: Resets browser profile settings to factory |
| `History` / `History-journal` | Browsing history and URL autocomplete | **MEDIUM**: Destroys browsing search history |
| `Web Data` | Autofill forms and credit cards | **HIGH**: Clears saved form autofill information |
| `Extensions/` | Installed Chrome extension binaries | **HIGH**: Uninstalls user extensions |

---

### 2. Disposable: Pure Regenerable Cache (SAFE TO PURGE)

| Directory Name | Contents | Safe to Purge? |
| :--- | :--- | :--- |
| `Cache` / `Cache_Data` | Downloaded web assets, images, CSS, JS | **YES** |
| `Code Cache\js` | V8 compiled bytecode of visited sites | **YES** |
| `Code Cache\wasm` | Compiled WebAssembly modules | **YES** |
| `GPUCache` | Compiled GPU hardware acceleration assets | **YES** |
| `DawnWebGPUCache` / `DawnCache` | WebGPU pipeline state caches | **YES** |
| `ShaderCache` / `GrShaderCache` | Skia and OpenGL graphics shader blobs | **YES** |
| `Service Worker\CacheStorage` | Progressive Web App offline cached files | **YES** |
| `Service Worker\ScriptCache` | Service worker script bytecode | **YES** |
| `component_crx_cache` | Temporary downloaded CRX update packages | **YES** |
| `Crashpad\reports` | Unsent application crash dump files | **YES** |

---

## Process State Invariant
Never mutate SQLite databases or directory trees while `chrome.exe` or `msedge.exe` are active. Terminate background processes cleanly prior to deletion.
