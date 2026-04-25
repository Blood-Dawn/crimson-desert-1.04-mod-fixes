# Crimson Desert 1.04 Mod Fixes

Hand-rebased JSON mods for Crimson Desert game version **1.04.02** (April 23, 2026 patch).

These are community fixes for popular mods that broke when Pearl Abyss shipped patch 1.04. Original mod authors will likely update their own releases eventually — these are stopgaps for the impatient.

> **Status:** Working. All offsets byte-verified against vanilla 1.04.02 game files.

---

## What's in here

| Mod | Original | Verified patches | Notes |
|---|---|---|---|
| **Infinite Stamina** (5 variants) | [Nexus #107](https://www.nexusmods.com/crimsondesert/mods/107) by lemonheads | 123 / 140 | Sprint, flight, climbing, swimming, horse, glider all work. 17 specific combat skills not rebased — see [Known Limitations](#known-limitations). |
| **Infinite Spirit Cost** (4 variants) | [Nexus #135](https://www.nexusmods.com/crimsondesert/mods/135) by lemonheads | 102 / 149 | Most combat skills cost reduced. The `Active_UseResource_Mp` ramp didn't rebase cleanly — partial coverage. |
| **EasyMoney** | [Nexus #917](https://www.nexusmods.com/crimsondesert/mods/917) by Pldada | 4 / 4 | GoldBar costs 3c at Hernand tailor, sells for 500s at bank. |
| **Craft GoldBar With 1 GoldOre** | [Nexus #1100](https://www.nexusmods.com/crimsondesert/mods/1100) by Pldada | 1 / 1 | Self-explanatory. |

> **Note on Pldada mods (EasyMoney, GoldBar):** The original `readme.txt` for these mods includes a no-redistribution clause from the author. They're included here in the spirit of "rebased to keep working" rather than redistribution. If you're the original author and want them removed, [open an issue](../../issues) and I'll pull them.

---

## Quick install (CDUMM)

> **Required:** [CDUMM v3.x or later](https://www.nexusmods.com/crimsondesert/mods/207). The legacy CD JSON Mod Manager will probably also work for these files but I haven't tested.

1. Download the `.json` file you want from `mods/` (e.g. `mods/stamina/stamina_v104_infinite_1.04_fix.json`).
2. In CDUMM, click **+ Inspect Mod** (or drag-and-drop the .json file onto the CDUMM window).
3. Toggle the mod **Activated**.
4. Click **Apply**.
5. Launch the game.

If CDUMM rejects the import with byte-mismatch errors, your game version is probably not 1.04.02 — check `Settings → Video → GameVersion` in the corner of the in-game options.

### Pick one variant per category

These mods conflict with themselves — install only ONE stamina variant and ONE spirit variant at a time. Don't enable both `infinite` and `50pct` simultaneously.

---

## Mod categories

### `mods/stamina/` — Stamina drain reduction

5 variants for different play styles:

| File | Effect |
|---|---|
| `stamina_v104_10pct_1.04_fix.json` | 10% drain (very fast depletion still) |
| `stamina_v104_25pct_1.04_fix.json` | 25% drain |
| `stamina_v104_50pct_1.04_fix.json` | 50% drain |
| `stamina_v104_75pct_1.04_fix.json` | 75% drain (subtle) |
| **`stamina_v104_infinite_1.04_fix.json`** | **Effectively infinite (drain = -0.001)** |

Covers: sprint, all flight (CrowWing/Glider), aerial roll, focused aerial roll, climbing, swimming, horse stamina, mount stamina, vehicle stamina, bow draw stamina, most combat stamina, guard.

### `mods/spirit/` — Spirit (MP) cost reduction

4 variants:

| File | Effect |
|---|---|
| `spirit_cost_v104_25pct_1.04_fix.json` | 25% cost (75% off) |
| `spirit_cost_v104_50pct_1.04_fix.json` | 50% cost |
| `spirit_cost_v104_75pct_1.04_fix.json` | 75% cost |
| **`spirit_cost_v104_infinite_1.04_fix.json`** | **Zero cost on most skills** |

Partial coverage — see [Known Limitations](#known-limitations) below.

### `mods/economy/` — Money & crafting

| File | Effect |
|---|---|
| `EasyMoney_1.04_fix.json` | Buy GoldBar for 3 copper at Hernand tailor, sell at bank for 500 silver. |
| `Craft_GoldBar_With_1_GoldOre_1.04_fix.json` | Crafting recipe `GoldBar_2` requires 1 GoldOre instead of 100. |

---

## Known limitations

### Stamina (17 of 140 patches not rebased)

These specific combat moves will still drain stamina at vanilla rates because Pearl Abyss changed their drain values in 1.04 — the byte patterns the mod expects don't exist anymore at any predictable location:

```
Skill_HorsePawStamp                 (one horse animation)
Skill_ShieldDash                    (shield charge)
Skill_Damian_ShieldDash             (Damian's shield charge)
Skill_ShieldParryingDash            (parry-dash combo)
Skill_TurnBash_I                    (one weapon attack)
Skill_MoveCutting_I/II/III          (3 attack variants)
Skill_DeepThrust                    (one attack)
Skill_QuickDraw                     (one attack)
Skill_Wrestle_GiantSwing            (one wrestling move)
Skill_Demian_RapierHardAttack       (Damian rapier hard-attack)
Skill_NoLimit_Cutting_I             (one limit-break attack)
Skill_BossWeapon_*                  (4 boss weapon attacks)
```

In practice: **all your traversal and exploration stamina is fully fixed.** A handful of specific niche combat moves will still cost their normal stamina. Most players won't notice unless they spam shield-dash or specific named attacks.

### Spirit cost (47 of 149 patches not rebased)

The big one is `Active_UseResource_Mp` (30 missing patches) — that's a ramped resource entry driving many MP-using skills. Pearl Abyss restructured it in 1.04. Effect: spirit-using skills will be partially zeroed but you may still see some cost on certain skill levels. Functionally it feels "nearly infinite" because regen will outpace the partial costs.

### Spirit regen (not included)

The spirit regen variants from the original mod only had 13 of 31 patches verifying against 1.04.02 — most importantly the core `Active_Recovery_MP` Focus passive didn't rebase. Half a regen mod is worse than no regen mod, so I didn't include those files. The spirit-cost mod alone should be enough for most use cases.

---

## Verification

Every patch in every JSON file in this repo has been byte-verified against the vanilla game data extracted from a 1.04.02 install. If a mod claims to patch offset X with bytes Y, those exact bytes Y were observed at offset X in the game's `gamedata/iteminfo.pabgb`, `gamedata/storeinfo.pabgb`, `gamedata/skill.pabgb`, or `gamedata/multichangeinfo.pabgb` at the time of release.

This means:
- ✅ CDUMM will accept these mods without "byte patches don't match" errors
- ✅ The patches will actually apply to your game files
- ⚠️ Whether the patches produce the *intended gameplay effect* is a separate question — most do, but Pearl Abyss may have restructured some game systems (durability/cooldown most notably) so some changes may be no-ops in-game

---

## Game version tested against

```
Crimson Desert 1.04.02 (game exe FileVersion 1.0.0.978)
Released 2026-04-23 by Pearl Abyss
Steam app id 3321460
```

If your installed game is a different sub-version (1.04.00, 1.04.01, 1.04.03+) these may or may not work. CDUMM will tell you immediately at import time if the offsets don't match — no risk of corrupting anything.

---

## Reporting issues

Found a mod that doesn't behave as expected in-game? Notice a specific patch that should have applied but didn't? [Open an issue](../../issues) with:

1. The mod file you used
2. Your game version (Settings → Video → GameVersion)
3. What you expected vs. what happened
4. CDUMM diagnostic output if available

---

## Credits & licensing

- **lemonheads / CyberGecko1** — original Infinite Stamina (#107) and Infinite Spirit (#135) mods. All gameplay design is theirs. This repo only adjusts the byte offsets to match game version 1.04.02.
- **Pldada (PLMOST)** — original EasyMoney (#917) and Craft GoldBar With 1 GoldOre (#1100) mods. Same — gameplay logic is theirs, offsets rebased.
- **Bloodawn** — rebasing work, repo maintenance.
- **CDUMM author (faisalkindi)** — the mod manager and PAZ archive parser this work depends on. Without their reverse-engineering of the PAZ format and their semantic merge engine, none of this would be feasible.

The rebased JSON files in this repo are derivative works of the originals. They're shared in good faith to keep the modding community working through game updates. If you're an original mod author and want yours removed, please [open an issue](../../issues) and it'll be pulled.

---

## Why this exists

Crimson Desert 1.04 (April 23, 2026) reworked crafting, added difficulty modes, added new pets, added new skills, and shifted data table layouts. This broke nearly every JSON byte-patch mod in the ecosystem. Mod authors update on their own schedules — sometimes within days, sometimes over weeks.

This repo exists to bridge that gap: keep popular mods working through update cycles, and serve as a reference for anyone learning how to rebase JSON mods themselves.

If you're a mod author and want help rebasing your work, [open an issue](../../issues) — happy to share the tooling.
