# Changelog

## 2026-04-25 — Initial release

First release. Tested against Crimson Desert 1.04.02 (game exe FileVersion 1.0.0.978).

### Added
- **Stamina** (5 variants — 10/25/50/75/infinite drain percentages)
  - Rebased from lemonheads' Infinite Stamina v1.02.00 (Nexus #107)
  - 123 of 140 patches verified against vanilla 1.04.02
  - Covers sprint, flight, climbing, swimming, horse, glider, aerial roll, mount, vehicle, bow, most combat
- **Spirit cost** (4 variants — 25/50/75/infinite cost percentages)
  - Rebased from lemonheads' Infinite Spirit and Spirit Regen v1.02.00 (Nexus #135)
  - 102 of 149 patches verified
  - Spirit regen variants intentionally not included (only 13/31 patches verified — too sparse to ship)
- **EasyMoney** (1 file)
  - Rebased from Pldada's v3.0 (Nexus #917)
  - 4 of 4 patches verified
- **Craft GoldBar With 1 GoldOre** (1 file)
  - Rebased from Pldada's v1.0 (Nexus #1100)
  - 1 of 1 patches verified, entry-anchored

### Methodology notes
- All rebases used per-entry-region byte-pattern search anchored on entry name strings in the relevant `.pabgb` data tables
- Vanilla game data extracted from `0008/0.paz` using the documented PAZ archive index (LZ4-block compressed, with offsets pulled from CDUMM's `.pamt_index.cache`)
- Every shipped patch was post-verified — the `original` byte sequence in each JSON exists at the new `offset` in the unmodified game file
- Patches that couldn't be unambiguously located within their entry's data region were dropped rather than guessed at — this is why some skills are missing from the stamina/spirit fixes
