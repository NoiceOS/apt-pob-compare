# Awakened PoE Trade + PoB comparison (unofficial)

🇫🇷 [Version française](README.fr.md)

An **unofficial** modified build of [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade) 3.29.108 for Path of Exile 1, for **Windows 10/11** and **Linux**.
It adds a **"PoB Comparison"** block to the price check (Ctrl+D). The checked item is compared to the item equipped in the same slot of your Path of Building build, and **Path of Building recalculates** your DPS, EHP, life, max hits, resistances and other stats.

> ⚠️ This is not the official version. **Do not report bugs to SnosMe**, or on the official APT or Path of Building repositories.

## What it shows

- **Collapsed** header: a summary such as `DPS ±x% · EHP ±y%`.
- **Expanded**: every stat the item changes, as in the PoB tooltip (before → after, difference, %).
- A choice of the build's **item set** (leveling / endgame / maxed…) and **slot** (Ring 1 / Ring 2, weapons…).
- Without Path of Building configured: a simpler comparison based on item mods only (resistances vs cap, attribute requirements, life…).

Official auto-updates are **disabled** in this build (an official update would replace it and remove the comparison).

## Installation: Windows 10/11

### 1. Download and extract

From the [**Releases**](../../releases/latest) page, download `Awakened-PoE-Trade-3.29.108-pob-win-x64.zip` and extract it, for example to `C:\Games\Awakened PoE Trade PoB\`.

It contains the app and a `luajit-win64\` folder (the engine used to run Path of Building in the background).

### 2. Get Path of Building Community (source version)

Download [**PathOfBuilding v2.67.2 (zip)**](https://github.com/PathOfBuildingCommunity/PathOfBuilding/archive/refs/tags/v2.67.2.zip) and extract it, for example to `C:\Games\PathOfBuilding-2.67.2\`.

> The installed version of Path of Building (the `.exe` installer) **does not work**: you need this source folder, which contains `src\` and `runtime\`.

### 3. Launch

Run `Awakened PoE Trade.exe`. If Windows SmartScreen shows a warning (the app is not signed), click **More info → Run anyway**.

### 4. Configure

In APT settings → **Price check** → **"Path of Building comparison"** section:

| Field | Example |
|---|---|
| Build link | `https://pobb.in/xxxx` (pobb.in, pastebin or raw PoB code) |
| Path of Building folder | `C:\Games\PathOfBuilding-2.67.2` |
| LuaJIT executable | `C:\Games\Awakened PoE Trade PoB\luajit-win64\luajit.exe` |

Use **full paths**. Tip: Shift + right-click on a file → "Copy as path" (remove the quotes).

## Installation: Linux x86_64

### 1. Download

From the [**Releases**](../../releases/latest) page:
- `Awakened-PoE-Trade-3.29.108-pob.AppImage`
- `lua-utf8.so`

```bash
chmod +x Awakened-PoE-Trade-3.29.108-pob.AppImage
```

### 2. Install LuaJIT

```bash
sudo dnf install luajit      # Fedora
sudo apt install luajit      # Debian / Ubuntu
sudo pacman -S luajit        # Arch
```

On an immutable system (Bazzite, Silverblue…), you can use Homebrew: `brew install luajit`.

### 3. Get Path of Building Community

```bash
git clone --depth 1 --branch v2.67.2 https://github.com/PathOfBuildingCommunity/PathOfBuilding.git ~/PathOfBuilding
cp lua-utf8.so ~/PathOfBuilding/runtime/
```

`lua-utf8.so` **must** be in PoB's `runtime/` folder.

### 4. Launch

```bash
./Awakened-PoE-Trade-3.29.108-pob.AppImage
```

### 5. Configure

In APT settings → **Price check** → **"Path of Building comparison"** section:

| Field | Example |
|---|---|
| Build link | `https://pobb.in/xxxx` (pobb.in, pastebin or raw PoB code) |
| Path of Building folder | `/home/<you>/PathOfBuilding` |
| LuaJIT executable | `/usr/bin/luajit` (or `/home/linuxbrew/.linuxbrew/bin/luajit`) |

Run `which luajit` to get the full path. Use the **full path**: when the app is launched from a menu, it may not get the same `PATH` as your terminal.

## Usage

Press Ctrl+D on an item in game. The "PoB Comparison" block appears above the filters.
On the first check, Path of Building starts in the background (about 5 s). After that, each comparison is instant.

## Limitations

- APT must be set to **English** (like the game client).
- The build's PoB configuration (charges, conditions, main skill) is used as exported.
- If Path of Building can't parse an item, an error is shown instead of the calculation.
- Tested with Path of Building 2.67.2 and its development version. Later PoB versions may need an update of this build.
- The Windows build was compiled on Linux and **has not been tested on a real Windows machine yet**. Feedback welcome.

## Troubleshooting

| Message | Fix |
|---|---|
| `Cannot start LuaJIT` | Wrong LuaJIT path (Windows: path to `luajit.exe`; Linux: check with `which luajit`) |
| `Set the full path to luajit.exe in settings` | Windows: the LuaJIT field must be a full path |
| `Path of Building not found` | The folder must contain `src/HeadlessWrapper.lua` (use the source zip, not the installer) |
| `module 'lua-utf8' not found` | Linux: `lua-utf8.so` missing from `PathOfBuilding/runtime/`. Windows: keep the 3 files of `luajit-win64\` together |
| `Could not load the build` | Check the link, or re-export the build from PoB |

## Repository contents

- `patches/awakened-poe-trade-3.29.108-pob-compare.patch`: the source code changes, to apply on APT's `v3.29.108` tag (`git apply`).
- `licenses/`: licenses of the projects used.

### Build it yourself

```bash
git clone https://github.com/SnosMe/awakened-poe-trade.git && cd awakened-poe-trade
git checkout v3.29.108
git apply ../patches/awakened-poe-trade-3.29.108-pob-compare.patch
cd renderer && npm ci && npm run build
cd ../main && npm ci && npm run build
npx electron-builder build --linux AppImage --publish never
npx electron-builder build --win zip --x64 --publish never -c.win.signAndEditExecutable=false
```

Lua runtime:
- Linux `lua-utf8.so`, from [starwing/luautf8](https://github.com/starwing/luautf8) (commit `a47b143`):
  `gcc -O2 -shared -fPIC -I/usr/include/luajit-2.1 lutf8lib.c -o lua-utf8.so`
- Windows `luajit-win64\`, cross-compiled with mingw-w64 from [LuaJIT](https://github.com/LuaJIT/LuaJIT) v2.1 (commit `a4c0645`):
  `make HOST_CC=gcc CROSS=x86_64-w64-mingw32- TARGET_SYS=Windows BUILDMODE=dynamic`, then
  `x86_64-w64-mingw32-gcc -O2 -shared -I<LuaJIT>/src lutf8lib.c -o lua-utf8.dll -L<LuaJIT>/src -llua51`

## Licenses

All MIT licensed, see `licenses/`:
- [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade): © Alexander Drozdov
- [Path of Building Community](https://github.com/PathOfBuildingCommunity/PathOfBuilding)
- [LuaJIT](https://luajit.org): © Mike Pall
- [luautf8](https://github.com/starwing/luautf8): © Xavier Wang

Not affiliated with or endorsed by Grinding Gear Games.
