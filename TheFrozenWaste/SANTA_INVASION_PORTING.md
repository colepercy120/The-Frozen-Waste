# Annahir / Father Christmas Sunset Invasion: porting manifest

This subsystem is isolated into Santa-specific files, apart from necessary `lotr_` overrides to shared base-mod definitions. Copy the paths below, **relative to `TheFrozenWaste/`**, into the matching folders of the destination mod.

## Self-contained file set (17 files)

- `common/casus_belli_types/tfw_santa_invasion_cb.txt`
- `common/coat_of_arms/coat_of_arms/tfw_sunset_invasion_title_coa.txt`
- `common/dna_data/tfw_santa_dna.txt`
- `common/dynasties/tfw_santa_dynasty.txt`
- `common/dynasty_houses/tfw_santa_houses.txt`
- `common/game_rules/zz_tfw_santa_invasion_game_rules.txt`
- `common/landed_titles/tfw_santa_titles.txt`
- `common/modifiers/tfw_santa_modifiers.txt`
- `common/nicknames/tfw_santa_nicknames.txt`
- `common/religion/doctrine_types/tfw_santa_tenets.txt`
- `common/scripted_effects/tfw_santa_invasion_effects.txt`
- `events/tfw_santa_invasion_events.txt`
- `gfx/interface/icons/faith/giftlord.dds`
- `gfx/portraits/portrait_modifiers/tfw_santa_portrait_modifiers.txt`
- `history/characters/tfw_santa_characters.txt`
- `localization/english/tfw_santa_invasion_l_english.yml`
- `map_data/geographical_regions/tfw_santa_invasion_regions.txt`

The dynasty and title coats of arms intentionally coexist in `tfw_sunset_invasion_title_coa.txt`. The invasion and console test effects intentionally coexist in `tfw_santa_invasion_effects.txt`. There is only one active definition for each key.

## Required edits to upstream `lotr_` overrides (not isolated)

1. `common/scripted_effects/lotr_effects/lotr_sunset_invasion_effects.txt`: The shared `sunset_invasion_general_effect` needs the fallback call to `tfw_sunset_invasion_select_custom_invader_effect`, after its Eonwe and Pharazon branches, so it recognizes the Santa region; the existing `$REVIVE_DEAD_CHARACTER_EFFECT$` hook then creates the live Annahir.
2. `events/lotr_events/lotr_war_events.txt`: The normal and random intervention rule handling must route `tfw_santa_intervention` and the Santa random choice to `tfw_santa_invasion_effect`. The debug event also has a Santa branch.
3. `common/religion/religion_types/lotr_elven_religion.txt`: `faith_santa` is nested under the base elven religion and cannot be defined on its own without replacing the parent religion. Copy its faith block into the target mod's corresponding `lotr_` override.

The game rule also overrides the complete base `sunset_invasions_rule`; its entire replacement is already isolated as `common/game_rules/zz_tfw_santa_invasion_game_rules.txt`. Keep the `zz_` filename ordering, and merge any other project's added intervention choices instead of allowing two competing definitions of `sunset_invasions_rule`.

## Other definitions the destination must provide or adapt

- `c_aurora_isles_2` is the landing county and capital of `h_tfw_santa_host`.
- `middleearth` and `middleearth_forodwaith` are combined into the provided `tfw_santa_invasion_region`. The destination must define the source regions.
- `maiar` and `lossidil` cultures are used for Annahir and the landing county, respectively.
- `tfw_lineofnixni_001` is the existing ancient Maiar reference for the spawned Annahir's chronological age; adjust or provide this character in a project without TFW.
- `faith_santa` is provided by the `lotr_elven_religion.txt` patch, and `tenet_giftlord` by the isolated `tfw_santa_tenets.txt`. Other doctrines, holy sites, and religious identifiers referenced by the faith must exist in the target.
- The invasion armies use `lossidil_frostguard`, `lossidil_icebowmen`, `lossidil_snow_spears`, `lossidil_frost_riders`, `high_elven_heavy_infantry`, `high_elven_archers`, and `song_smiths`. They must exist in the target mod's men-at-arms data.
- The WED startup assignment of `lifestyle_mage` to `tfw_santa_001` is an optional **separate WED integration**, not part of the files here. The spawn effect transfers the rank if the template has the trait.
- The base mod's `sunset_invasion_general_effect` and `sunset_invasion_general_landing_effect` machinery, war scopes, and special `h_` title conventions must be present.

The `gfx/interface/icons/faith/giftlord.dds` asset is a binary image; copy it unchanged. Do not duplicate the faith or tenet definition from the old shared files. This package stores the tenet and related faith/tenet localization in Santa-specific files.

## Gameplay and test entry points

- Rule: `tfw_santa_intervention` in `sunset_invasions_rule`.
- Spawn and war: `tfw_santa_invasion_effect = yes`, run in an appropriate character scope with `largest_realm_ruler` selected.
- Console: `effect tfw_test_santa_invasion_effect = yes`.
- Console (all four available invasions): `effect tfw_test_all_sunset_invasions_effect = yes`. This starts simultaneous wars; use a disposable save.
- Historical dummy: `tfw_santa_001`, deceased at game start, with named DNA and immortal appearance age 100. The spawned character copies inherited appearance, has its own founding house and dynasty, and is assigned Nixni's chronological age.

## Porting verification

Check that each top-level script key is defined only once, `faith_santa` appears only within the chosen elven-religion override, `tenet_giftlord` appears only in the isolated tenet file, all seven Santa faith/tenet localization keys are in `tfw_santa_invasion_l_english.yml`, and the target project has all required county, culture, unit, and faith IDs. Run the console invasion on a disposable save, checking the Jolly Host's title, heraldry, portrait, liege transitions and war.
