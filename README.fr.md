# Awakened PoE Trade + comparaison PoB (non officiel)

🇬🇧 [English version](README.md)

Version modifiée et **non officielle** d'[Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade) 3.29.108 pour Path of Exile 1, pour **Windows 10/11** et **Linux**.
Elle ajoute au price check (Ctrl+D) un bloc **« PoB Comparison »** : l'item vérifié est comparé à l'item équipé dans le même slot de ton build Path of Building, et **Path of Building recalcule** le DPS, l'EHP, la vie, les max hit, les résistances, etc.

> ⚠️ Ce n'est pas la version officielle. **Ne signale pas de bug à SnosMe**, ni sur le dépôt officiel d'APT, ni sur celui de Path of Building.

## Ce que ça affiche

- En-tête **replié** : résumé `DPS ±x% · EHP ±y%`.
- **Déplié** : toutes les stats modifiées par l'item, comme dans l'infobulle de PoB (avant → après, écart, %).
- Choix du **set d'items** du build (leveling / endgame / maxed…) et du **slot** (Ring 1 / Ring 2, armes…).
- Sans Path of Building configuré : comparaison simplifiée basée sur les mods (résistances vs cap, prérequis d'attributs, vie…).

Les mises à jour automatiques officielles sont **désactivées** dans cette version (une mise à jour officielle la remplacerait et supprimerait la comparaison).

## Installation : Windows 10/11

### 1. Télécharger et extraire

Depuis la page [**Releases**](../../releases/latest), télécharge `Awakened-PoE-Trade-3.29.108-pob-win-x64.zip` et extrais-le, par exemple dans `C:\Jeux\Awakened PoE Trade PoB\`.

Il contient l'application et un dossier `luajit-win64\` (le moteur qui fait tourner Path of Building en arrière-plan).

### 2. Récupérer Path of Building Community (version source)

Télécharge [**PathOfBuilding v2.67.2 (zip)**](https://github.com/PathOfBuildingCommunity/PathOfBuilding/archive/refs/tags/v2.67.2.zip) et extrais-le, par exemple dans `C:\Jeux\PathOfBuilding-2.67.2\`.

> La version installée de Path of Building (l'installeur `.exe`) **ne fonctionne pas** : il faut ce dossier source, qui contient `src\` et `runtime\`.

### 3. Lancer

Lance `Awakened PoE Trade.exe`. Si Windows SmartScreen affiche un avertissement (l'application n'est pas signée), clique sur **Informations complémentaires → Exécuter quand même**.

### 4. Configurer

Dans les réglages d'APT → **Price check** → bloc **« Path of Building comparison »** :

| Champ | Exemple |
|---|---|
| Build link | `https://pobb.in/xxxx` (pobb.in, pastebin ou code PoB brut) |
| Path of Building folder | `C:\Jeux\PathOfBuilding-2.67.2` |
| LuaJIT executable | `C:\Jeux\Awakened PoE Trade PoB\luajit-win64\luajit.exe` |

Mets bien les **chemins complets**. Astuce : Maj + clic droit sur un fichier → « Copier en tant que chemin d'accès » (enlève les guillemets).

## Installation : Linux x86_64

### 1. Télécharger

Depuis la page [**Releases**](../../releases/latest) :
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
git clone --depth 1 --branch v2.67.2 https://github.com/PathOfBuildingCommunity/PathOfBuilding.git ~/PathOfBuilding
cp lua-utf8.so ~/PathOfBuilding/runtime/
```

Le fichier `lua-utf8.so` doit **obligatoirement** être dans le dossier `runtime/` de PoB.

### 4. Lancer

```bash
./Awakened-PoE-Trade-3.29.108-pob.AppImage
```

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
Au premier check, Path of Building démarre en arrière-plan (~5 s), puis chaque comparaison est instantanée.

## Limites

- APT doit être réglé en **anglais** (comme le client du jeu).
- La config PoB du build (charges, conditions, skill principal) est utilisée telle qu'elle a été exportée.
- Si Path of Building ne sait pas lire un item, une erreur s'affiche à la place du calcul.
- Les textes du panneau de comparaison sont en anglais.
- Testé avec Path of Building 2.67.2 et sa version de développement. Une version plus récente de PoB pourrait nécessiter une mise à jour de ce build.
- La version Windows a été compilée sous Linux et **n'a pas encore été testée sur un vrai Windows**. Les retours sont bienvenus.

## Dépannage

| Message | Solution |
|---|---|
| `Cannot start LuaJIT` | Mauvais chemin LuaJIT (Windows : chemin vers `luajit.exe` ; Linux : vérifier avec `which luajit`) |
| `Set the full path to luajit.exe in settings` | Windows : le champ LuaJIT doit être un chemin complet |
| `Path of Building not found` | Le dossier doit contenir `src/HeadlessWrapper.lua` (prendre le zip source, pas l'installeur) |
| `module 'lua-utf8' not found` | Linux : `lua-utf8.so` absent de `PathOfBuilding/runtime/`. Windows : garder ensemble les 3 fichiers de `luajit-win64\` |
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
npx electron-builder build --win zip --x64 --publish never -c.win.signAndEditExecutable=false
```

Runtime Lua :
- Linux `lua-utf8.so`, depuis [starwing/luautf8](https://github.com/starwing/luautf8) (commit `a47b143`) :
  `gcc -O2 -shared -fPIC -I/usr/include/luajit-2.1 lutf8lib.c -o lua-utf8.so`
- Windows `luajit-win64\`, cross-compilé avec mingw-w64 depuis [LuaJIT](https://github.com/LuaJIT/LuaJIT) v2.1 (commit `a4c0645`) :
  `make HOST_CC=gcc CROSS=x86_64-w64-mingw32- TARGET_SYS=Windows BUILDMODE=dynamic`, puis
  `x86_64-w64-mingw32-gcc -O2 -shared -I<LuaJIT>/src lutf8lib.c -o lua-utf8.dll -L<LuaJIT>/src -llua51`

## Licences

Tous sous licence MIT, voir `licenses/` :
- [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade) : © Alexander Drozdov
- [Path of Building Community](https://github.com/PathOfBuildingCommunity/PathOfBuilding)
- [LuaJIT](https://luajit.org) : © Mike Pall
- [luautf8](https://github.com/starwing/luautf8) : © Xavier Wang

Projet non affilié à Grinding Gear Games, ni approuvé par eux.
