# TFW Sunset Invasion: minimal base-system integration

This package adds Annahir (Father Christmas) as a new invasion option. It **uses the source mod's existing Sunset Invasion scripted effects** rather than copying or modifying them.

## Required files

Keep these Annahir-specific files in their respective mod directories:

- `common/casus_belli_types/tfw_sunset_invasion_cb.txt`
- `common/coat_of_arms/coat_of_arms/tfw_sunset_invasion_title_coa.txt`
- `common/dna_data/tfw_sunset_invasion_dna.txt`
- `common/dynasties/tfw_sunset_invasion_dynasties.txt`
- `common/dynasty_houses/tfw_sunset_invasion_houses.txt`
- `common/landed_titles/tfw_sunset_invasion_titles.txt`
- `common/modifiers/tfw_sunset_invasion_modifiers.txt`
- `common/nicknames/tfw_sunset_invasion_nicknames.txt`
- `common/scripted_effects/tfw_sunset_invasion_effects.txt`
- `events/tfw_sunset_invasion_events.txt`
- `gfx/portraits/portrait_modifiers/tfw_sunset_invasion_portrait_modifiers.txt`
- `history/characters/tfw_sunset_invasion_characters.txt`
- `localization/english/tfw_sunset_invasion_l_english.yml`
- `map_data/geographical_regions/tfw_sunset_invasion_regions.txt`

**Do not copy** `common/scripted_effects/lotr_effects/lotr_sunset_invasion_effects.txt` into TFW or other companion mods. The source mod supplies `sunset_invasion_general_effect`, `sunset_invasion_general_landing_effect`, `sunset_invasion_general_variables_effect`, and the war-participant effects. Annahir's spawn callback saves the new character as `invader_char` for those effects.

## Two necessary integrations with source definitions

These cannot be extended by adding a separate, same-named definition. Keep them synchronized with the corresponding source-mod file when upgrading:

1. `common/game_rules/zz_tfw_sunset_invasion_game_rules.txt` extends the existing `sunset_invasions_rule` with `tfw_santa_intervention`. It must preserve all original options.
2. `events/lotr_events/lotr_war_events.txt` adds `tfw_santa_invasion_effect = yes` to the `tfw_santa_intervention` case and adds Annahir to `random_intervention`. All existing events and invaders remain unchanged. The source mod's debug event needs no Santa-specific branch.

Do not add new on-actions or a parallel invasion scheduler. The source mod's existing invasion trigger event continues to decide when to run.

## Design and dependencies

The Santa implementation uses the same generic callbacks as the base invasions:

- `tfw_santa_invasion_grab_landing_county`: save `c_muilfalas_1` as `landing_county`.
- `tfw_santa_invasion_spawn_character`: create the new Annahir and save him as `invader_char`.
- `tfw_santa_invasion_landing_effect`: apply the generic landing setup to `h_tfw_santa_host` and spawn his army.
- `tfw_santa_invasion_start_war`: declare `tfw_santa_invasion_cb` against `largest_realm_ruler`, using the generic participant effect.

The extra custom prewar vassalization and replacement-target search are intentionally absent. Standard Sunset Invasion CB victory and subject-transfer effects apply.

The invasion **references** but does not define `faith_santa` (in `lotr_elven_religion.txt`), the separate Giftlord tenet, `c_muilfalas_1`, `maiar`, `lossidil`, `tfw_lineofnixni_001`, and the named men-at-arms. Preserve these dependencies or adapt them when porting.

No console-only test wrappers or additional diagnostic logging are included. Test the standard game-rule and random-intervention paths in a disposable save.
