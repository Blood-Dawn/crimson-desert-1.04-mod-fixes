# Stamina mods

Rebased from [Infinite Stamina (Nexus #107)](https://www.nexusmods.com/crimsondesert/mods/107) by **lemonheads / CyberGecko1** for game 1.04.02.

## Variants

Pick **ONE** — they conflict with each other:

| File | Stamina drains at |
|---|---|
| `stamina_v104_10pct_1.04_fix.json` | 10% normal speed (almost gone fast still) |
| `stamina_v104_25pct_1.04_fix.json` | 25% |
| `stamina_v104_50pct_1.04_fix.json` | 50% |
| `stamina_v104_75pct_1.04_fix.json` | 75% (subtle effect) |
| **`stamina_v104_infinite_1.04_fix.json`** | **~0% (effectively infinite)** |

## What's covered

123 of the original 140 patches verified against vanilla 1.04.02. Covers:

- Sprint / running stamina
- All flight (CrowWing variants — hover, move, dash)
- Glider / Aerial Roll / Focused Aerial Roll
- Climbing
- Swimming
- Horse stamina (most horse skills)
- Mount stamina (vehicles, dragons, etc.)
- Bow draw stamina
- Most combat stamina (attacks, guards, dodges)
- Wrestle stamina

## What's not covered

17 specific combat moves were not rebased because Pearl Abyss changed their drain values in 1.04 to amounts the original mod didn't anticipate. They'll still drain stamina at vanilla rates:

```
Skill_HorsePawStamp                 (one specific horse animation)
Skill_ShieldDash                    (shield charge)
Skill_Damian_ShieldDash             (Damian's shield charge)
Skill_ShieldParryingDash            (parry-dash combo)
Skill_TurnBash_I                    (one weapon attack variant)
Skill_MoveCutting_I/II/III          (3 attack variants)
Skill_DeepThrust                    (one attack)
Skill_QuickDraw                     (one attack)
Skill_Wrestle_GiantSwing            (one wrestling move)
Skill_Demian_RapierHardAttack       (Damian rapier hard-attack)
Skill_NoLimit_Cutting_I             (one limit-break attack)
Skill_BossWeapon_HamondBindel
Skill_BossWeapon_Common
Skill_BossWeapon_FlameKnight
Skill_BossWeapon_SplitHorn
```

In practice your traversal/exploration stamina is fully fixed and most combat is fixed. A handful of niche attacks still cost normal stamina.

## Install

Drag the `.json` you want into CDUMM, enable, apply. See the [main install guide](../../docs/INSTALL.md).
