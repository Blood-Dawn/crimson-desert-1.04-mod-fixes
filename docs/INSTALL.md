# Installation guide

Step-by-step for installing these mods. If you already use CDUMM regularly, just drag a `.json` into the Inspect Mod area, enable, apply, and you're done — the rest of this is for newcomers.

## 1. Verify your game version

In-game: Settings → Video → look at the corner. You want to see `GameVersion: 1.04.02`.

If you're on a different sub-version (1.04.00, 1.04.01, 1.04.03+), these files may or may not work. CDUMM will check at import time.

## 2. Install CDUMM if you don't have it

[CDUMM v3.x — Nexus Mods #207](https://www.nexusmods.com/crimsondesert/mods/207)

Download the latest release, run the .exe. It auto-detects your Crimson Desert install via Steam. First launch will scan all 208 game files and build a vanilla snapshot — takes a few minutes the first time.

> **Note:** The legacy CD JSON Mod Manager will probably also accept these mods (the JSON format is the same), but CDUMM is recommended because it does proper byte-verification before applying, which protects you from corrupting your game files if a mod is broken.

## 3. Download the mods you want

Browse the `mods/` folder in this repo. Pick:

- **One** stamina variant from `mods/stamina/`
- **One** spirit variant from `mods/spirit/` (if you want spirit cost reduction)
- **EasyMoney** and/or **Craft GoldBar** from `mods/economy/` if you want those

Don't enable multiple variants of the same mod — they conflict with themselves.

You can download individual `.json` files directly from GitHub by clicking the file, then **"Raw"** in the top right, then Ctrl+S to save.

Or just clone/download the whole repo:

```bash
git clone https://github.com/Bloodawn/crimson-desert-1.04-mod-fixes.git
```

## 4. Import into CDUMM

For each `.json` file you want to install:

1. Open CDUMM
2. **Drag and drop** the `.json` file directly onto the CDUMM window (the "Drop mods here to install" area)
3. Or use the **Inspect Mod** menu option to import individual files

CDUMM will verify the byte offsets against your live game data. You should see "Apply Completed with Warnings" or similar acceptance — if you see "X byte patches don't match," the mod isn't compatible with your game version.

## 5. Enable and apply

1. After import, the mod appears in the **PAZ Mods** list
2. Click the toggle to set it to **Activated** (green)
3. Click the **Apply** button at the top
4. Wait for "Apply Complete — All mod changes applied and verified successfully"

## 6. Launch the game

You can launch the game directly from CDUMM with the **Launch Game** button, or from Steam. Either works.

## Troubleshooting

### "X byte patches don't match"

The mod isn't compatible with your game version. Check:
- Your game version is exactly 1.04.02
- You're using the latest version of these mod files (re-clone the repo)
- Your game files aren't already modified by another mod manager

### "Apply hangs / watchdog kills it after 180 seconds"

Too many mods enabled simultaneously. CDUMM's semantic merge engine chokes when many mods all touch the same large file (`0008/0.paz`). Workaround: disable the heaviest 5MB+ delta mods first, apply, then re-enable smaller ones one at a time.

### "Apply Completed with Warnings"

This is fine. Warnings are about disabled mods CDUMM is checking, not about active ones. Active mods applied successfully.

### Game crashes when opening map / inventory / specific menu

Some other mod is conflicting. Disable mods one at a time until the crash stops to identify the culprit. The crash is rarely from these specific mods (stamina/spirit/economy don't touch UI files).

### My save is from before I installed mods — is it safe?

Yes. None of these mods modify save data. They modify the game's static data tables. Your save is unaffected by mod install/uninstall.

## Uninstalling

In CDUMM:
1. Toggle the mod to **Deactivated**
2. Click **Apply**
3. Or: right-click the mod → **Remove** to fully uninstall

CDUMM keeps vanilla backups of every patched file and restores them automatically when you disable mods. Your game files return to clean vanilla state.
