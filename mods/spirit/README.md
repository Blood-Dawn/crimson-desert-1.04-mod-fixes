# Spirit (MP) cost mods

Rebased from [Infinite Spirit and Spirit Regen (Nexus #135)](https://www.nexusmods.com/crimsondesert/mods/135) by **lemonheads / CyberGecko1** for game 1.04.02.

> **Only the cost-reduction variants are included.** The regen-multiplier variants from the original mod only had ~40% of patches verifying against 1.04.02 — the core `Active_Recovery_MP` Focus passive entry didn't rebase. Half a regen mod would feel broken, so those files are not shipped here. The cost mod alone effectively gives you "infinite spirit" because you can't spend what's free.

## Variants

Pick **ONE** — they conflict with each other:

| File | Spirit costs at |
|---|---|
| `spirit_cost_v104_25pct_1.04_fix.json` | 25% (75% off) |
| `spirit_cost_v104_50pct_1.04_fix.json` | 50% |
| `spirit_cost_v104_75pct_1.04_fix.json` | 75% (subtle) |
| **`spirit_cost_v104_infinite_1.04_fix.json`** | **~0% (effectively free)** |

## What's covered

102 of the original 149 patches verified. Most combat skills with spirit costs are zeroed:

- Most elemental attacks
- RotationBash, SkyKick, JiJeongTa, HaDongTa
- ChargeShot, MultiShot, DeadEye
- ElementalReinforce, BlackWolf, TriStar
- Most level-1 through level-9 versions of the above

## What's not covered

47 patches were dropped because the bytes don't exist at the expected offsets in 1.04.02. The biggest gap is **`Active_UseResource_Mp`** (30 missing patches) — a ramped resource entry that drives many MP-using skills. Pearl Abyss restructured this in 1.04. Effect: some skill levels will still consume some spirit.

In practice the result feels nearly infinite — even partially-zeroed costs combined with normal regen mean you rarely deplete.

## Install

Drag the `.json` you want into CDUMM, enable, apply. See the [main install guide](../../docs/INSTALL.md).
