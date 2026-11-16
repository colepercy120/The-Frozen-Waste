# TFW Sunset Invasion: Porting and expansion guide

The portable unit is the **Sunset Invasion framework**, not the Santa faith or tenet. All package filenames use `tfw_sunset_invasion` (except the `zz_` load-order prefix on game rules). Existing `tfw_santa_` keys remain Santa-option-specific so more invasion options can coexist.

## Copy these 15 files into the corresponding mod paths

- `common/casus_belli_types/tfw_sunset_invasion_cb.txt`
- `common/coat_of_arms/coat_of_arms/tfw_sunset_invasion_title_coa.txt`
- `common/dna_data/tfw_sunset_invasion_dna.txt`
- `common/dynasties/tfw_sunset_invasion_dynasties.txt`
- `common/dynasty_houses/tfw_sunset_invasion_houses.txt`
- `common/game_rules/zz_tfw_sunset_invasion_game_rules.txt`
- `common/landed_titles/tfw_sunset_invasion_titles.txt`
- `common/modifiers/tfw_sunset_invasion_modifiers.txt`
- `common/nicknames/tfw_sunset_invasion_nicknames.txt`
- `common/scripted_effects/tfw_sunset_invasion_effects.txt`
- `events/tfw_sunset_invasion_events.txt`
- `gfx/portraits/portrait_modifiers/tfw_sunset_invasion_portrait_modifiers.txt`
- `history/characters/tfw_sunset_invasion_characters.txt`
- `localization/english/tfw_sunset_invasion_l_english.yml`
- `map_data/geographical_regions/tfw_sunset_invasion_regions.txt`

The coats of arms for both the Jolly Host title and Annahir's dynasty are in one file. The scripted effects file includes a shared custom-invader dispatcher, the Santa option, and individual/combined console invasion test effects. Add new invaders to these same category-specific files with their own unique scripted IDs.

## Shared `lotr_` integration (merge into destination overrides)

- `common/scripted_effects/lotr_effects/lotr_sunset_invasion_effects.txt`: the common `sunset_invasion_general_effect` must call `tfw_sunset_invasion_select_custom_invader_effect` when choosing custom invaders; the common spawn/landing/start-war parameter hooks remain in use.
- `events/lotr_events/lotr_war_events.txt`: intervention game-rule and random choices dispatch Santa to `tfw_santa_invasion_effect`, including the debug event.

The provided `common/game_rules/zz_tfw_sunset_invasion_game_rules.txt` declares the complete `sunset_invasions_rule` including the `tfw_santa_intervention` option. **Merge other projects' choices** into that definition instead of loading duplicate rule definitions.

To expand, add a unique option-specific effect to `tfw_sunset_invasion_effects.txt`, extend the shared invader selection hook where necessary, add its game rule choice, and wire its `lotr_war_events.txt` dispatch. Santa-specific character/CB/content IDs remain unchanged.

## Excluded: faith and tenet are separate content

**Do not port the following as part of the invasion package:**

- `tenet_giftlord`: stays in `common/religion/doctrine_types/tfw_tenets.txt`.
- `faith_santa`: defined in the base-elven-religion override `common/religion/religion_types/lotr_elven_religion.txt`.
- Faith and tenet localization `faith_santa*` and `tenet_giftlord_*`: stays in `localization/english/tfw_religion_l_english.yml`.
- Faith icon `gfx/interface/icons/faith/giftlord.dds`: stays with religious content.

The invasion **references** the faith, so a destination project must supply it independently. No Giftlord tenet/faith code is included in this package.

## External identifiers to verify in the destination

- Landing and capital: `c_aurora_isles_2`. Package defines `h_tfw_santa_host` and `tfw_santa_invasion_region`.
- Source regions: `middleearth` and `middleearth_forodwaith`.
- Cultures: `maiar` for Annahir, `lossidil` for the landing county.
- Reference character: `tfw_lineofnixni_001` (used to copy Annahir's chronological age; immortal age is 100).
- Faith: `faith_santa`, provided separately from the invasion.
- Invader troops: `lossidil_frostguard`, `lossidil_icebowmen`, `lossidil_snow_spears`, `lossidil_frost_riders`, `high_elven_heavy_infantry`, `high_elven_archers`, `song_smiths`.
- Optional WED: the dead historical template `tfw_santa_001` may receive `lifestyle_mage` via WED's startup setup; its spawned counterpart gets XP if that trait exists.

Other common base-mod traits, modifiers, script effects, and army definitions must exist or be adapted.

## Console tests

- Santa: `effect tfw_test_santa_invasion_effect = yes`
- Valinor: `effect tfw_test_valinor_invasion_effect = yes`
- Pharazôn: `effect tfw_test_pharazon_invasion_effect = yes`
- Amav: `effect tfw_test_amav_invasion_effect = yes`
- All four: `effect tfw_test_all_sunset_invasions_effect = yes`

Use a disposable save for the all-four test. In-game validation of the dynasty, Jolly Host heraldry, initial war, age and good-ruler vassalization is still required.
