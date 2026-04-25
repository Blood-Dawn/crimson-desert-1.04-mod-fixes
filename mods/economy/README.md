# Economy mods

Rebased from Pldada's mods on Nexus for game 1.04.02.

> **Author note:** Pldada's original `readme.txt` for these mods includes a no-redistribution clause ("禁止上传到别的地方"). They're included here as a community courtesy to keep them working through 1.04 — if Pldada (PLMOST) wants them removed, [open an issue](../../../issues) and they'll be pulled.

## Files

| File | Original | What it does |
|---|---|---|
| `EasyMoney_1.04_fix.json` | [Nexus #917](https://www.nexusmods.com/crimsondesert/mods/917) | Buy GoldBar at Hernand tailor for 3 copper, sell to bank for 500 silver. Effectively unlimited money. |
| `Craft_GoldBar_With_1_GoldOre_1.04_fix.json` | [Nexus #1100](https://www.nexusmods.com/crimsondesert/mods/1100) | The recipe `GoldBar_2` requires 1 GoldOre instead of 100. |

Both of these can be installed simultaneously — they don't conflict with each other.

## Verification

- **EasyMoney**: 4 of 4 patches verified against vanilla 1.04.02 (3 in `storeinfo.pabgb` for the tailor shop entry, 1 in `iteminfo.pabgb` for the GoldBar sell price)
- **Craft GoldBar**: 1 of 1 patches verified, entry-anchored to `GoldBar_2` in `multichangeinfo.pabgb`

## Install

Drag the `.json` you want into CDUMM, enable, apply. See the [main install guide](../../docs/INSTALL.md).

## Heads up

These two mods together let you generate effectively infinite silver coins. If that ruins your fun, install neither and play vanilla economy. If you want a small economy boost without breaking it, install just the GoldOre crafting fix and skip EasyMoney.
