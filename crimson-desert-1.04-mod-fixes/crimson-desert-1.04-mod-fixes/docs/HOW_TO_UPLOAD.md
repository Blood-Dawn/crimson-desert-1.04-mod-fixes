# How to upload this repo to GitHub

> Internal doc — for the maintainer (Bloodawn). Delete this file before publishing the repo if you don't want it visible to users.

## Option 1: Web UI (no git required)

1. **Create the repo on github.com**
   - Go to https://github.com/new while logged into your Bloodawn account
   - Repository name: `crimson-desert-1.04-mod-fixes`
   - Description: *Hand-rebased JSON mods for Crimson Desert 1.04.02 — community fixes for popular mods that broke when Pearl Abyss shipped patch 1.04.*
   - Public
   - **Do NOT** check "Initialize this repository with a README" — we already have one
   - Click **Create repository**

2. **Upload files via web UI**
   - On the empty repo page, click **uploading an existing file** (the link appears in the empty-repo instructions)
   - Drag and drop the **entire contents** of this folder (`crimson-desert-1.04-mod-fixes/`) onto the page
   - Wait for all files to finish uploading (~13 files, mostly small)
   - Commit message: `Initial release — 1.04.02 fixes for stamina, spirit, economy mods`
   - Click **Commit changes**

3. **Verify**
   - Go to your repo's main page
   - You should see the README rendered with the table of mods
   - Click into `mods/stamina/` to verify the JSON files are there

## Option 2: Git CLI

If you prefer git from the terminal:

```bash
# 1. Create the empty repo on github.com first (don't initialize with README)

# 2. From this folder:
cd crimson-desert-1.04-mod-fixes/

git init
git add .
git commit -m "Initial release — 1.04.02 fixes for stamina, spirit, economy mods"
git branch -M main
git remote add origin https://github.com/Bloodawn/crimson-desert-1.04-mod-fixes.git
git push -u origin main
```

## Option 3: GitHub CLI (`gh`)

If you have `gh` installed and authenticated:

```bash
cd crimson-desert-1.04-mod-fixes/

gh repo create Bloodawn/crimson-desert-1.04-mod-fixes \
  --public \
  --source=. \
  --remote=origin \
  --description "Hand-rebased JSON mods for Crimson Desert 1.04.02 — community fixes for popular mods that broke when Pearl Abyss shipped patch 1.04." \
  --push
```

One command, done.

## After uploading

### Add topics to make it findable

On your repo page, click the gear icon next to "About" in the right sidebar. Add topics:
- `crimson-desert`
- `mods`
- `mod-manager`
- `cdumm`
- `nexus-mods`
- `game-modding`

### Pin a "where to download" comment on Nexus

Go to the relevant Nexus mod pages (especially [#107 Infinite Stamina](https://www.nexusmods.com/crimsondesert/mods/107) and [#135 Infinite Spirit](https://www.nexusmods.com/crimsondesert/mods/135)) and post a comment in their forums:

> 1.04.02 rebased version available at https://github.com/Bloodawn/crimson-desert-1.04-mod-fixes — community fix while waiting for the official update. Cleared with no warranty; full credit to the original author.

This is the part that helps players actually find your fix. The repo is invisible without these signposts.

### Watch for issues

Set up GitHub email notifications. The first issue you'll get is probably either "doesn't work for me" (which is usually a different game sub-version) or one of the original mod authors asking you to take it down. Both are worth responding to within a day if possible.

### When the next patch breaks everything

Crimson Desert ships frequent updates. When 1.05 (or whatever) drops:

1. Re-extract vanilla data from the new game version
2. Re-run the rebasing script against the new vanilla
3. Bump version, update CHANGELOG, push
4. The same install steps work for users — they just download fresh files
