# W.R.E.C.K.

# Warband Refined & Enhanced Compiler Kit

WRECK is a compiler that allows you to use data from any module during compilation. A vanilla compiler runs each module one at a time, requiring you to import modules with the required data each time. WRECK loads these modules only once, saving time.

## Optional modules

In non-integrated WRECK, you can add *module_item_modifiers.py*, *module_ui_strings.py* and *module_user_hints.py* for compilation.

**module_item_modifiers.py**

Compiled to *Data/item_modifiers.txt*. This module allows you to change names, prices, and spawn rates (rarity coefficients). Game effects cannot be changed, but their descriptions are provided in the comments section of the file.

**module_ui_strings.py**

Compiles to *languages/en/ui.csv*. Allows editing of interface text.

**module_user_hints.py**

Compiled to *languages/en/hints.csv*. Contains hints shown to the player in various situations. Note: The compiler does not change the value in *module.ini* `num_hints =` . This value must be changed manually.

The remaining options are only available with full WRECK integration.

## Wrecking module system

- Copy content of the archive to your module system.

- Open *module_strings.py* in the text editor. Save with encoding "Utf-8" (not "Utf-8 with BOM"); otherwise the compiler fails with an encoding error.

- Wreck files with *wrecker.bat*.

- Delete files *build_module.bat*, *wrecker.bat*, *wrecker.py* and all files starting with *"process_"*.

- Open *module_info_example.py* and compare to your *module_info.py*. All information should be there. After that you can delete example file.

- Check that your python version is 3. Open command line (shell) and type `python -V`. If your version is not 3 than you need to edit system PATH variables or add full path for python in the *compile.bat*.

- You need to add parentheses to all print statements. This is difference between Python2 and 3. Alternatively you can use header files from already integrated module system. Actually there is only one file that needs to be edited '*header_troops.py*.

- Add this line to header_troops.py: `bignum = 0x40000000000000000000000000000000`

- Add this to header_operations.py right after the `neg = 0x80000000`:

`negate = neg`

- Add this to module_constants.py:

########################################################

## POSITIONS RESERVED BY CODE

########################################################

triggers_paranoia_pos = pos100
position_move_xyz_fp_pos = pos101

If you missed the last two steps you will see these errors: [negate](https://static.wikia.nocookie.net/mount26blade20mooders20reference/images/9/9c/Negate.png/revision/latest?cb=20260129074325), [position](https://static.wikia.nocookie.net/mount26blade20mooders20reference/images/b/b8/Position.png/revision/latest?cb=20260129074357).

## Integrated WRECK abilities

The vanilla compiler uses two types of identifiers: static identifiers found in *ID_*.py* files (`icon_player`) and dynamic identifiers in the form of strings (`"icon_player"`), which are generated during compilation. This creates some problems. For example, you can't simultaneously add an icon and a party that uses it. You must first add the icon, then run *process_icons.py* to generate the icon identifier *ID_map_icons.py*. Only then can you use this icon in the party. This requires two compilations. WRECK uses only dynamic identifiers (`icon.player`, `"icon_player"`), which can be used anywhere and in any way. WRECK uses standard tag notations (**trp, itm, scn**), except for strings. Instead of **str** (`str.string_name`), which is reserved in Python, strings are denoted as `s.string_name`.

Global and local variables can also use this syntax. `":variable"` can be converted to `l.variable`, and `"$variable"` can be converted to `g.variable`. Both the old and new syntaxes can be used.

New dynamic identifiers can be used in mathematical expressions that involve run-time variables: local, global, and registers. For these expressions, the compiler will generate code and incorporate it into the module system. Mathematical expressions cannot be used in operations that involve `this_or_next` in the current or previous operand. This will result in errors.

Identifier properties can be used at compile time `(assign, reg11, trp.swadian_footman.strength)`.

| Items                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Scenes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Item modifiers                                            | Factions                                               | Parties                                                                                                                                                                                                                                                                                                                                                  | Party templates                                                                                                       | Troops                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - flags<br>- capabilities<br>- price<br>- weight<br>- head_armor<br>- body_armor<br>- leg_armor<br>- difficulty<br>- hp<br>- speed<br>- missile_speed<br>- size<br>- max_amount<br>- swing *(combined binary-packed value)*<br>- swing_damage<br>- swing_damage_type<br>- thrust *(combined binary-packed value)*<br>- thrust_damage<br>- thrust_damage_type<br>- abundance<br>- modifiers *(as imodbit mask)*<br>- food_quality<br>- accuracy<br>- horse_maneuver<br>- horse_speed<br>- weapon_length<br>- horse_charge | - flags<br>- min_x<br>- min_y<br>- max_x<br>- max_y<br>- water_level<br>- water_level_cm<br>- terrain_seed<br>- river_seed<br>- flora_seed<br>- size_x *(0..1023)*<br>- size_y *(0..1023)*<br>- valley *(0..127)*<br>- hill_height *(0..127)*<br>- ruggedness *(0..127)*<br>- vegetation *(0..127)*<br>- terrain *(see header_terrain_types.py)*<br>- polygon_size *(2..5)*<br>- disable_grass *(0..1)*<br>- shade_occlude *(0..1)*<br>- place_river *(0..1)*<br>- deep_water *(0..1)* | - price_coeff *(per cent)*<br>- rarity_coeff *(per cent)* | - flags<br>- coherence *(per cent)*<br>- default_color | - flags_field *(icon+flags binary-packed value)*<br>- icon<br>- flags<br>- default_menu<br>- template<br>- faction<br>- personality<br>- ai<br>- ai_target<br>- start_x *(rounded)*<br>- start_x_cm *(in 1/100ths, rounded)*<br>- start_y *(rounded)*<br>- start_y_cm *(in 1/100ths, rounded)*<br>- angle *(degrees)*<br>- angle_100 *(1/100th degrees)* | - flags_field *(icon+flags binary-packed value)*<br>- icon<br>- flags<br>- default_menu<br>- faction<br>- personality | - flags<br>- scene<br>- faction<br>- level<br>- strength<br>- agility<br>- intelligence<br>- charisma<br>- wp_1h<br>- wp_2h<br>- wp_polearms<br>- wp_archery<br>- wp_crossbows<br>- wp_thrown<br>- wp_firearms<br>- skills *(combined bitmask)*<br>- facecode_1<br>- facecode_2<br>- upgrade_path_1<br>- upgrade_path_2 |

Examples:

(assign, ":sword_avg_damage", (itm.sword.swing_damage + itm.sword.thrust_damage) / 2),
(assign, ":water_level", scn.bulugha_castle.water_level_cm),

# Village will be automatically positioned 5 units to the east from Castle, meaning we can change both parties coordinates simultaneously.

("castle", "Castle", icon.castle_c|pf_castle, 0, pt.none, fac.commoners, 0, ai_bhvr_hold, 0, (72.3, -31.8), [], 311),
("village", "Village", icon.village_a|pf_village, 0, pt.none, fac.commoners, 0, ai_bhvr_hold, 0, (p.castle.start_x + 5.0, p.castle.start_y), [], 43),

# All castles automatically inherit properties from "castle_tpl" party template at compile time.

("castle_1", "Culmarr Castle", icon.castle_a|pt.castle_tpl.flags, pt.castle_tpl.default_menu, pt.castle_tpl, pt.castle_tpl.faction, pt.castle_tpl.personality, pt.castle_tpl.ai, pt.castle_tpl.ai_target, (-101.3, -21), [], 50),
("castle_2", "Malayurg Castle", icon.castle_b|pt.castle_tpl.flags, pt.castle_tpl.default_menu, pt.castle_tpl, pt.castle_tpl.faction, pt.castle_tpl.personality, pt.castle_tpl.ai, pt.castle_tpl.ai_target, (97.5, -2.2), [], 75),

## Using plugin systems

WRECK provides its own plugin system. The mandatory plugins are in the root module system folder. They are *plugin_ms_extension.py* and *plugin_presentations.py*. Optional plugin *plugin_item_factions_limit_remover.py* is commented out. It can be used to bypass items 16 faction limit. Other plugins are in the *system* folder and this plugins are loaded automatically. So if you don't need them than just move to other folder.

Creating a plugin is easy. Just create a "*.py" file in the "system" folder and add at the top:

from compiler import *
register_plugin()

This will register plugin with file name by default. Avoid using short common verbs because they can be overriden by other modules like *module_constants.py*.

Inside this plugin we can add new entities. Just create a list with enitity type for example `troops = []` or `scripts = []`. They will be added at the end of those lists in the order in wich plugins are loaded. If you need to place code in the special place use injecting.

Create a list for example `tavern_test_trigger = []` with your triggers in the list. At the end of the plugin add this lines:

`injection = { 'town_default': tavern_test_trigger, }`

And in the file *module_mission_templates.py* add this at the end of the mission template "town_default":

  ]
    + bodyguard_triggers
    + essentials
    + helpful_presentations
    + [
    inject('town_default'),
    ]
  ),

  ("conversation_encounter", 0, -1,
  "Conversation encounter",

The other powerfull tool plugins provide is ability to change entity properties. In the plugin add this code:

def preprocess_entities(glob):

# generate dummy props to avoid item spawn issues on dedicated servers

  if len(glob['scene_props']) < len(glob['items']): #more items than props
    for i in range(len(glob['scene_props']), len(glob['items'])):
        glob['scene_props'].append(("zyx_dummy_prop_"+str(i), 0, 0, 0, []))

Here we created dummy entities. However thay cannot be used (referenced) in the module system files. You need to create a place holder first outside this function. For example:

scripts = [
    #place holders
    ("animation_duration_init", []),
    ("init_troop_flags", []),
    ("init_party_flags", []),
    ("init_party_template_flags", []),
]

def preprocess_entities(glob):
    for cur_script in glob['scripts']:
        if cur_script[0] == 'animation_duration_init':
            export = cur_script[1]
            break
    for animation in glob['animations']:
        duration = 1000
        command = ()
        for sequence in animation[3]:
            cur_duration = sequence[0]
            if cur_duration < duration: duration = cur_duration
        duration *= 1000
        command += (troop_set_slot, trp.animation_duration, slot, duration)
        export.append(command)
        slot += 1

    for cur_script in glob['scripts']:
        if cur_script[0] == 'init_troop_flags':
            export = cur_script[1]
            break
    index = 0
    for troop in glob['troops']:
        export.append((troop_set_slot, trp.troop_flags, index, troop[3]))
        index += 1
    
    for cur_script in glob['scripts']:
        if cur_script[0] == 'init_party_flags':
            export = cur_script[1]
            break
    index = 0
    for party in glob['parties']:
        flags = (party[2] & 0xFFFFFF00)
        export.append((troop_set_slot, trp.party_flags, index, flags))
        index += 1
    
    for cur_script in glob['scripts']:
        if cur_script[0] == 'init_party_template_flags':
            export = cur_script[1]
            break
    index = 0

All code in the examples above are from wreck built in plugins. So explore their code to learn more.

Also worth noticing that you can add any constant directly to plugin but this constants will not be available outside the plugin.

## Syntax extension

Another powerful feature of the WRECK plugin system is coding extensions using macro operands.

For example, a modder could add the following code to a plugin:

def troop_set_attribute(troop, attribute, value):
  return [
    (store_attribute_level, l._cached_, troop, attribute),
    (store_sub, l._cached_, value, l._cached_),
    (troop_raise_attribute, troop, attribute, l._cached_),
  ]
extend_syntax(troop_set_attribute)

This will add a new macro operation `(troop_set_attrigute, <troop_id>, <attrigute_id>, <new_value>)` that can be used anywhere. Of course, this isn't a real operand and will be replaced at compile time by the plugin's code.

The new operation can be used in other plugins, but only in those loaded after the plugin containing this macro operand.

While you can create can-fail macro operators, they shouldn't be used with the `this_or_next` or `neg` prefixes. They also can't be the last operand in a `this_or_next` conditional block. That is, the preceding operand must not contain `this_or_next`.

## Command line parameters

Command line parameters can be added to *compile.bat*.

1. `test` - will not export to text files.
2. `time` - will show compilition time.
3. `tag` - remove this parameter to compare with Napoleonic Wars compiler. It doesn't use tags. I don't recommend to remove this parameter. Tags are very big numbers in the left most bits of the x32 integers. They show type of the variable or entity to the game engine. Some operands can behave differently without those tags.
4. `triggers_paranoia` - will check triggers parameters for rewriting. Remove whis parameter on clean release. Because this plugin adds new triggers to each mission template.
5. `bw` - black and white mode.
6. `nodupe` - no messages about duplication.
7. `silent` - no error and warning messages.
8. `errors` - no warning messages.
9. `warnings` - ?.
10. `notices` - default option all messages.
11. `wait` - wait for pressing enter. Can be used when launched from comman line.
12. `cap` - no capitalisation messages.
13. `fac` - no faction messages.
14. `prsnt_helper` - inject presenation helper panel to all presentations.
15. `nested_loops` - no performance heavy nested loops messages.

## WRECK limitations

For plugin users: It is not possible to create new enitites (scripts, items, etc) in preprocess function. You need to create a place holders first. Actually you can create them but there are no allocated identifiers for them. I.e. you can not access directly in module code like "str_new_text". But you save the id of that string to any slot and reference them with local variables. Look into example in macros "str_store_item_id_repr".

Checking performance heavy loops is not full. It doesn't check inside scripts.

If local variable declared inside try-else chaining block it can be easily undeclared. WRECK will not show warning.

See the details at <a target="_blank" href="https://forums.taleworlds.com/index.php/topic,325102.0.html">project's official forum thread</a>.
