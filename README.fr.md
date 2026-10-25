# Awakened PoE Trade + comparaison PoB (Linux, non officiel)

🇬🇧 [English version](README.md)

Version modifiée et **non officielle** d'[Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade) 3.29.108 pour Path of Exile 1.
Elle ajoute au price check (Ctrl+D) un bloc **« PoB Comparison »** : l'item vérifié est comparé à l'item équipé dans le même slot de ton build Path of Building, et **Path of Building recalcule** le DPS, l'EHP, la vie, les max hit, les résistances, etc.

> ⚠️ Ce n'est pas la version officielle. **Ne signale pas de bug à SnosMe**, ni sur le dépôt officiel d'APT, ni sur celui de Path of Building.

## Ce que ça affiche

- En-tête **replié** : résumé `DPS ±x% · EHP ±y%`.
- **Déplié** : toutes les stats modifiées par l'item, comme dans l'infobulle de PoB (avant → après, écart, %).
- Choix du **set d'items** du build (leveling / endgame / maxed…) et du **slot** (Ring 1 / Ring 2, armes…).
- Sans Path of Building configuré : comparaison simplifiée basée sur les mods (résistances vs cap, prérequis d'attributs, vie…).

## Installation (Linux x86_64)

### 1. Télécharger

Depuis la page [**Releases**](../../releases) :
- `Awakened-PoE-Trade-3.29.108-pob.AppImage`
- `lua-utf8.so`

```bash
chmod +x Awakened-PoE-Trade-3.29.108-pob.AppImage
```

### 2. Installer LuaJIT

```bash
sudo dnf install luajit      # Fedora
sudo apt install luajit      # Debian / Ubuntu
sudo pacman -S luajit        # Arch
```

Sur un système immuable (Bazzite, Silverblue…), tu peux passer par Homebrew : `brew install luajit`.

### 3. Récupérer Path of Building Community

```bash
git clone --depth 1 https://github.com/PathOfBuildingCommunity/PathOfBuilding.git ~/PathOfBuilding
cp lua-utf8.so ~/PathOfBuilding/runtime/
```

Le fichier `lua-utf8.so` doit **obligatoirement** être dans le dossier `runtime/` de PoB.
Version testée : PoB **2.67.2** (commit `16de4b8`). Pour mettre PoB à jour plus tard : `git -C ~/PathOfBuilding pull`.

### 4. Lancer

```bash
./Awakened-PoE-Trade-3.29.108-pob.AppImage --no-updates
```

**`--no-updates` est important** : sans ça, la mise à jour automatique d'APT remplace cette version par l'officielle, et la comparaison disparaît.

### 5. Configurer

Dans les réglages d'APT → **Price check** → bloc **« Path of Building comparison »** :

| Champ | Exemple |
|---|---|
| Build link | `https://pobb.in/xxxx` (pobb.in, pastebin ou code PoB brut) |
| Path of Building folder | `/home/<toi>/PathOfBuilding` |
| LuaJIT executable | `/usr/bin/luajit` (ou `/home/linuxbrew/.linuxbrew/bin/luajit`) |

Le chemin complet s'obtient avec `which luajit`. Mets bien le **chemin complet** : quand l'app est lancée depuis un menu, elle ne voit pas toujours le même `PATH` que le terminal.

## Utilisation

Ctrl+D sur un item en jeu → le bloc « PoB Comparison » apparaît au-dessus des filtres.
Au premier check, Path of Building démarre en arrière-plan (~4 s), puis chaque comparaison est instantanée.

## Limites

- Linux uniquement (pas de version Windows / macOS).
- APT doit être réglé en **anglais** (comme le client du jeu).
- La config PoB du build (charges, conditions, skill principal) est utilisée telle qu'elle a été exportée.
- Si Path of Building ne sait pas lire un item, une erreur s'affiche à la place du calcul.
- Les textes du panneau de comparaison sont en anglais.

## Dépannage

| Message | Solution |
|---|---|
| `Cannot start LuaJIT` | Mauvais chemin LuaJIT → vérifier avec `which luajit` |
| `Path of Building not found` | Le dossier doit contenir `src/HeadlessWrapper.lua` |
| `module 'lua-utf8' not found` | `lua-utf8.so` absent de `PathOfBuilding/runtime/` |
| `Could not load the build` | Vérifier le lien, ou ré-exporter depuis PoB |

## Contenu du dépôt

- `patches/awakened-poe-trade-3.29.108-pob-compare.patch` : les modifications du code source, à appliquer sur le tag `v3.29.108` d'APT (`git apply`).
- `licenses/` : licences des projets utilisés.

### Recompiler soi-même

```bash
git clone https://github.com/SnosMe/awakened-poe-trade.git && cd awakened-poe-trade
git checkout v3.29.108
git apply ../patches/awakened-poe-trade-3.29.108-pob-compare.patch
cd renderer && npm ci && npm run build
cd ../main && npm ci && npm run build
npx electron-builder build --linux AppImage --publish never
```

`lua-utf8.so` est compilé depuis [starwing/luautf8](https://github.com/starwing/luautf8) (commit `a47b143`) :

```bash
gcc -O2 -shared -fPIC -I/usr/include/luajit-2.1 lutf8lib.c -o lua-utf8.so
```

## Licences

Tous sous licence MIT, voir `licenses/` :
- [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade) : © Alexander Drozdov
- [Path of Building Community](https://github.com/PathOfBuildingCommunity/PathOfBuilding)
- [luautf8](https://github.com/starwing/luautf8) : © Xavier Wang

Projet non affilié à Grinding Gear Games, ni approuvé par eux.
