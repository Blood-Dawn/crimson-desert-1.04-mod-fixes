# How rebasing JSON mods works

This is a brief explanation of the technique used to update these mods for game version 1.04.02. Sharing it so other modders can do the same when Pearl Abyss ships the next patch.

## The problem

JSON Mod Manager mods (the format used by CD JSON Mod Manager and CDUMM) work by writing specific byte values at specific byte offsets within Crimson Desert's data files. A typical patch looks like this:

```json
{
  "offset": 1262348,
  "label": "GoldBar -> 3c",
  "original": "50c30000",
  "patched": "03000000"
}
```

This says "at byte offset 1,262,348 in the game's `iteminfo.pabgb` file, the bytes are currently `50 c3 00 00` (= the integer 50,000), and we want to change them to `03 00 00 00` (= 3)."

When Pearl Abyss ships a game patch, they often add new items, change existing items, or restructure tables — which **shifts all the byte offsets**. The mod's `offset: 1262348` no longer points to the GoldBar entry; it might point to something completely different. CDUMM checks the `original` bytes against what's actually at that offset, sees a mismatch, and refuses to apply the patch.

## The fix

The key insight is that most game data tables are **structured by entry name**. The `iteminfo.pabgb` file contains thousands of items, each with a name string (`"GoldBar"`, `"Skill_CrowWing"`, etc.) followed by that item's data.

Even when offsets shift, **the entry names themselves don't change** unless Pearl Abyss removes the item entirely. So the rebasing approach is:

1. Find the entry name string in the new vanilla file
2. Search nearby (typically next 200–400 bytes) for the original byte pattern
3. Compute the new absolute offset
4. Write a new patch with the same `original` and `patched` values but the corrected `offset`

For mods that already use CDUMM's "entry-anchored" format (where each patch has `entry` and `rel_offset` fields alongside the absolute `offset`), this is even simpler — the rebaser just needs to find the entry name and add `rel_offset`.

## Step-by-step

### 1. Extract vanilla game data

Crimson Desert stores game data in PAZ archives at `<game_dir>/0000/0.paz`, `0008/0.paz`, etc. The `.pamt` files in the same folders are indexes telling you which file is at which offset within the archive.

CDUMM ships a Python pickle of the full index at `CDMods/.pamt_index.cache`. Loading it gives you a dict of `path -> PazEntry(paz_file, offset, comp_size, orig_size, flags)`.

The data tables we care about (`iteminfo.pabgb`, `storeinfo.pabgb`, `skill.pabgb`, `multichangeinfo.pabgb`, `factionnode.pabgb`) all live in `0008/0.paz`. They're LZ4-block compressed when `flags = 0x20000`.

```python
import lz4.block, pickle

# Load CDUMM's index
with open(".pamt_index.cache", "rb") as f:
    index = pickle.load(f)

# Find the file
entry = next(e for k, e in index.items() if "iteminfo.pabgb" in k)

# Read and decompress
with open(entry.paz_file, "rb") as f:
    f.seek(entry.offset)
    raw = f.read(entry.comp_size)
decompressed = lz4.block.decompress(raw, uncompressed_size=entry.orig_size)
```

You now have the vanilla bytes for the new game version.

### 2. For each patch in the broken mod:

```python
def rebase(mod_patch, vanilla_bytes):
    entry_name = mod_patch["entry"]                       # e.g. "GoldBar_2"
    rel_offset = mod_patch.get("rel_offset", 0)           # within-entry byte offset
    expected = bytes.fromhex(mod_patch["original"])

    # Find entry name (may appear multiple times — filter to exact matches)
    name_pos = vanilla_bytes.find(entry_name.encode("ascii"))

    # Search the entry's data region for the expected byte pattern
    region = vanilla_bytes[name_pos : name_pos + 400]
    rel_pos_in_region = region.find(expected)

    if rel_pos_in_region == -1:
        return None  # pattern not found — skip this patch

    new_offset = name_pos + rel_pos_in_region
    return {**mod_patch, "offset": new_offset}
```

### 3. Verify

After computing the new offset, double-check that the bytes at that offset match the `original` field. If they don't, drop the patch entirely rather than ship a wrong one.

### 4. Watch out for

- **Short byte patterns** (1–2 bytes) are too generic. They'll match coincidentally many places. Either expand the pattern (read 4–8 bytes of context from the original entry) or skip patches that have only short patterns.
- **Multiple name occurrences** — `Skill_CrowWing` matches as a substring of `Skill_CrowWing_Move`, `Skill_CrowWing_MoveFast`, etc. Always check the byte after the name match — it should be `\x00` (string terminator) or non-alphanumeric.
- **Pearl Abyss changes values, not just structure** — sometimes a skill that used to drain 10 stamina now drains 30. The mod's `original: "f0d8"` (= -10) won't appear because the new value is `0d8a` (= -30). These can't be safely rebased; the gameplay change must be re-derived manually.
- **Variable-length entries** — Pearl Abyss adds new fields, which pushes later fields further. The `rel_offset` from an old version is no longer reliable; you must search for the pattern, not jump to a fixed offset.

## CDUMM's "Inspect Mod" trick

When CDUMM rejects a `.zip` import with "byte patches don't match," extracting the `.json` and dragging just the `.json` file into the **Inspect Mod** area will sometimes accept it. CDUMM's `.zip` importer is stricter than its `.json` importer; the latter does verification but allows you to enable mods even with mismatches (which then no-op rather than apply).

This is useful for testing partially-rebased mods or for installing mods you've manually rebased even if a few patches still fail.
