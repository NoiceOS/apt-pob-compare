# Awakened PoE Trade + PoB comparison (Linux, unofficial)

🇫🇷 [Version française](README.fr.md)

An **unofficial** modified build of [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade) 3.29.108 for Path of Exile 1.
It adds a **"PoB Comparison"** block to the price check (Ctrl+D). The checked item is compared to the item equipped in the same slot of your Path of Building build, and **Path of Building recalculates** your DPS, EHP, life, max hits, resistances and other stats.

> ⚠️ This is not the official version. **Do not report bugs to SnosMe**, or on the official APT or Path of Building repositories.

## What it shows

- **Collapsed** header: a summary such as `DPS ±x% · EHP ±y%`.
- **Expanded**: every stat the item changes, as in the PoB tooltip (before → after, difference, %).
- A choice of the build's **item set** (leveling / endgame / maxed…) and **slot** (Ring 1 / Ring 2, weapons…).
- Without Path of Building configured: a simpler comparison based on item mods only (resistances vs cap, attribute requirements, life…).

## Installation (Linux x86_64)

### 1. Download

From the [**Releases**](../../releases) page:
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
git clone --depth 1 https://github.com/PathOfBuildingCommunity/PathOfBuilding.git ~/PathOfBuilding
cp lua-utf8.so ~/PathOfBuilding/runtime/
```

`lua-utf8.so` **must** be in PoB's `runtime/` folder.
Tested with PoB **2.67.2** (commit `16de4b8`). To update PoB later: `git -C ~/PathOfBuilding pull`.

### 4. Launch

```bash
./Awakened-PoE-Trade-3.29.108-pob.AppImage --no-updates
```

**`--no-updates` matters.** Without it, APT's auto-update replaces this build with the official one, and the comparison is gone.

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
On the first check, Path of Building starts in the background (about 4 s). After that, each comparison is instant.

## Limitations

- Linux only (no Windows or macOS build).
- APT must be set to **English** (like the game client).
- The build's PoB configuration (charges, conditions, main skill) is used as exported.
- If Path of Building can't parse an item, an error is shown instead of the calculation.

## Troubleshooting

| Message | Fix |
|---|---|
| `Cannot start LuaJIT` | Wrong LuaJIT path → check with `which luajit` |
| `Path of Building not found` | The folder must contain `src/HeadlessWrapper.lua` |
| `module 'lua-utf8' not found` | `lua-utf8.so` is missing from `PathOfBuilding/runtime/` |
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
```

`lua-utf8.so` is built from [starwing/luautf8](https://github.com/starwing/luautf8) (commit `a47b143`):

```bash
gcc -O2 -shared -fPIC -I/usr/include/luajit-2.1 lutf8lib.c -o lua-utf8.so
```

## Licenses

All MIT licensed, see `licenses/`:
- [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade): © Alexander Drozdov
- [Path of Building Community](https://github.com/PathOfBuildingCommunity/PathOfBuilding)
- [luautf8](https://github.com/starwing/luautf8): © Xavier Wang

Not affiliated with or endorsed by Grinding Gear Games.
