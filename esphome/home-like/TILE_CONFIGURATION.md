# Tile Configuration Reference

This document explains every substitution in `home-like.yaml` and provides copy-paste examples for all supported entity types.

---

## How substitutions work

ESPHome substitutions are compile-time text replacements. There is no runtime logic in the substitution block — every `${TILE1_...}` reference in the YAML is replaced with the value you set before the firmware is compiled and flashed. This means:

- You cannot use `if/else` in substitutions
- Changes require a reflash
- The firmware is fully self-contained after flashing

---

## Global settings

| Key | Default | Description |
|-----|---------|-------------|
| `DIRECT_ACTIONS` | `"true"` | `true` = device calls HA services directly on tap. `false` = only publishes the action event, you handle it in HA automations. |
| `ROOM_NAME` | `"Office"` | Text shown in the top-left header. |
| `TIME_24H` | `"true"` | `true` = 24h clock, `false` = 12h with AM/PM. |
| `AUTO_DIM` | `"true"` | Enable backlight auto-dimming after inactivity. |
| `AUTO_DIM_TIMEOUT` | `"10"` | Seconds of no touch before dimming. |
| `MAX_BRIGHTNESS` | `"100"` | Backlight brightness (%) when active. |
| `DIM_BRIGHTNESS` | `"40"` | Backlight brightness (%) when dimmed. |
| `NIGHT_MODE` | `"true"` | Use a lower brightness level during night hours. |
| `NIGHT_START_HOUR` | `"22"` | Hour (0–23) when night mode starts. |
| `NIGHT_END_HOUR` | `"7"` | Hour (0–23) when night mode ends. |
| `NIGHT_DIM_BRIGHTNESS` | `"18"` | Backlight brightness (%) during night hours. |

---

## Per-tile substitutions

Each of the 6 tiles has the same set of keys, just with a different number (TILE1 … TILE6).

### Basic settings

| Key | Description |
|-----|-------------|
| `TILE*_ENTITY` | The Home Assistant entity this tile controls (e.g. `light.living_room`). |
| `TILE*_STATE_ENTITY` | Entity used to determine active/inactive visual state. Usually the same as `TILE*_ENTITY`. Set to a different entity if you want the tile to reflect a group or a related sensor. |
| `TILE*_TITLE` | Text label shown on the tile. |
| `TILE*_ICON` | Material Design Icons Unicode glyph. Find icons at [pictogrammers.com/library/mdi](https://pictogrammers.com/library/mdi/) — click an icon and copy the Unicode value (e.g. `U+F0769` → `"\U000F0769"`). |
| `TILE*_TYPE` | Entity type: `light` \| `fan` \| `switch` \| `scene` \| `script` \| `cover`. Controls tap behavior, value display, and slider type in auto mode. |
| `TILE*_TAP_ACTION` | What happens on short tap — see [Tap actions](#tap-actions) below. |
| `TILE*_LONGPRESS` | What happens on long press — see [Long press modes](#long-press-modes) below. |
| `TILE*_VALUE_MODE` | How the value line below the title is rendered — see [Value modes](#value-modes) below. |
| `TILE*_LABEL_OFF` | Text shown in the value line when the entity is off / inactive. |
| `TILE*_LABEL_ON` | Text shown in the value line when `VALUE_MODE` is `text` and entity is on. |

### Colors

All colors are 24-bit hex in the format `"0xRRGGBB"`.

| Key | Description |
|-----|-------------|
| `TILE*_CIRCLE_ACTIVE_COLOR` | Icon circle background when active. |
| `TILE*_CIRCLE_DISABLED_COLOR` | Icon circle background when inactive. |
| `TILE*_ICON_ACTIVE_COLOR` | Icon glyph color when active. |
| `TILE*_ICON_DISABLED_COLOR` | Icon glyph color when inactive. |
| `TILE*_BG_ACTIVE_COLOR` | Tile background when active. |
| `TILE*_BG_DISABLED_COLOR` | Tile background when inactive. |
| `TILE*_TITLE_ACTIVE_COLOR` | Title text color when active. |
| `TILE*_TITLE_DISABLED_COLOR` | Title text color when inactive. |
| `TILE*_VALUE_ACTIVE_COLOR` | Value text color when active. |
| `TILE*_VALUE_DISABLED_COLOR` | Value text color when inactive. |

### Advanced settings (short tap)

| Key | Default | Description |
|-----|---------|-------------|
| `TILE*_TAP_SERVICE` | `""` | Override the HA service called on tap (e.g. `"light.toggle"`). Leave empty to use the default for the tile type. |
| `TILE*_TAP_PARAM_KEY` | `""` | Extra parameter key for the service call (e.g. `"preset_mode"`). |
| `TILE*_TAP_PARAM_VAL` | `""` | Extra parameter value (e.g. `"Auto"`). |

### Advanced settings (long press — slider mode)

Only relevant when `TILE*_LONGPRESS` is set to `"slider"`.

| Key | Default | Description |
|-----|---------|-------------|
| `TILE*_LONGPRESS_SLIDER` | `"auto"` | Slider type: `auto` \| `brightness` \| `percentage` \| `cover_position`. `auto` picks the right type based on `TILE*_TYPE`. |
| `TILE*_LONGPRESS_OFF_VALUE` | `"0"` | Initial slider position (0–100) shown when the entity is currently off. |

### Advanced settings (long press — action mode)

Only relevant when `TILE*_LONGPRESS` is set to `"action"`.

| Key | Default | Description |
|-----|---------|-------------|
| `TILE*_LONGPRESS_ACTION` | `""` | HA entity to call on long press. Can be completely different from `TILE*_ENTITY`. |
| `TILE*_LONGPRESS_ACTION_TYPE` | `""` | Entity type of the long press target: `light` \| `fan` \| `switch` \| `scene` \| `script` \| `cover`. Leave empty to inherit from `TILE*_TYPE`. |
| `TILE*_LONGPRESS_ACTION_SERVICE` | `""` | Service to call (default is `activate`/`turn_on`). Set e.g. `"light.toggle"` to toggle instead. |

---

## Tap actions

| Value | Behavior |
|-------|----------|
| `auto` | Picks the best action for the tile type: `toggle` for light/fan/switch/cover, `activate` for scene/script. |
| `toggle` | Toggles the entity (light.toggle / fan.toggle / switch.toggle / cover open↔close). |
| `activate` | Turns on / activates the entity (scene.turn_on / script.turn_on / light.turn_on / cover.open_cover). |
| `fan_toggle_preset` | If fan is ON: turns it off. If fan is OFF: turns it on and sets a preset mode (configure with `TAP_PARAM_KEY` / `TAP_PARAM_VAL`). |
| `custom` | Use `TAP_SERVICE` + `TAP_PARAM_KEY` / `TAP_PARAM_VAL` for a fully custom service call. |

---

## Long press modes

| Value | Behavior |
|-------|----------|
| `slider` | Opens a value overlay with a slider. Configure with `TILE*_LONGPRESS_SLIDER` and `TILE*_LONGPRESS_OFF_VALUE`. |
| `action` | Fires a different HA entity than the short tap. Configure with `TILE*_LONGPRESS_ACTION*`. |
| `none` | No direct action — only publishes `tileN_long_press` to HA so you can react in automations. |

In all modes, the event `tileN_long_press` is always published to `sensor.smartdisplay_action` in Home Assistant.

---

## Value modes

| Value | Behavior |
|-------|----------|
| `auto` | Picks automatically: `brightness` for lights, `percentage` or `preset` for fans, `text` for everything else. |
| `brightness` | Shows brightness as a percentage (e.g. `80 %`). When off, shows `LABEL_OFF`. |
| `percentage` | Shows fan speed as a percentage. When off, shows `LABEL_OFF`. |
| `preset` | Shows the fan preset_mode attribute. When off, shows `LABEL_OFF`. |
| `text` | Shows `LABEL_ON` when on, `LABEL_OFF` when off. For scenes/scripts, always shows `LABEL_ON`. |

---

## Home Assistant events

Every tap publishes a short-lived state to `sensor.smartdisplay_action`:

| Event | Trigger |
|-------|---------|
| `tile1_press` … `tile6_press` | Short tap on tile 1–6 |
| `tile1_long_press` … `tile6_long_press` | Long press on tile 1–6 |

The state resets to `""` after 500ms. In HA automations, trigger on `state` → `to: "tile1_press"` etc.

---

## Complete examples

### Light — toggle + brightness slider

```yaml
TILE1_ENTITY: "light.living_room_ceiling"
TILE1_STATE_ENTITY: "light.living_room_ceiling"
TILE1_TITLE: "Ceiling"
TILE1_ICON: "\U000F0769"          # mdi:ceiling-light
TILE1_TYPE: "light"
TILE1_TAP_ACTION: "auto"          # toggles the light
TILE1_LONGPRESS: "slider"         # long press opens brightness slider
TILE1_VALUE_MODE: "auto"          # shows brightness % when on, "Off" when off
TILE1_LABEL_OFF: "Off"
TILE1_LABEL_ON: "On"
# colors
TILE1_CIRCLE_ACTIVE_COLOR: "0xFEC600"
TILE1_CIRCLE_DISABLED_COLOR: "0x7B7B6F"
TILE1_ICON_ACTIVE_COLOR: "0xFFFFFF"
TILE1_ICON_DISABLED_COLOR: "0xFEC600"
TILE1_BG_ACTIVE_COLOR: "0xFFFFFF"
TILE1_BG_DISABLED_COLOR: "0x939391"
TILE1_TITLE_ACTIVE_COLOR: "0x000000"
TILE1_TITLE_DISABLED_COLOR: "0xFFFFFF"
TILE1_VALUE_ACTIVE_COLOR: "0x7A7A7C"
TILE1_VALUE_DISABLED_COLOR: "0xD9D9D9"
# advanced
TILE1_TAP_SERVICE: ""
TILE1_TAP_PARAM_KEY: ""
TILE1_TAP_PARAM_VAL: ""
TILE1_LONGPRESS_SLIDER: "auto"    # auto = brightness for lights
TILE1_LONGPRESS_OFF_VALUE: "0"
TILE1_LONGPRESS_ACTION: ""
TILE1_LONGPRESS_ACTION_TYPE: ""
TILE1_LONGPRESS_ACTION_SERVICE: ""
```

---

### Switch — simple toggle, no slider

```yaml
TILE2_ENTITY: "switch.office_power_strip"
TILE2_STATE_ENTITY: "switch.office_power_strip"
TILE2_TITLE: "Power Strip"
TILE2_ICON: "\U000F06FF"          # mdi:power-socket-eu
TILE2_TYPE: "switch"
TILE2_TAP_ACTION: "auto"          # toggles the switch
TILE2_LONGPRESS: "none"           # long press only sends event, no slider
TILE2_VALUE_MODE: "text"          # shows "On" / "Off"
TILE2_LABEL_OFF: "Off"
TILE2_LABEL_ON: "On"
# colors
TILE2_CIRCLE_ACTIVE_COLOR: "0xFEC600"
TILE2_CIRCLE_DISABLED_COLOR: "0x7B7B6F"
TILE2_ICON_ACTIVE_COLOR: "0xFFFFFF"
TILE2_ICON_DISABLED_COLOR: "0xFEC600"
TILE2_BG_ACTIVE_COLOR: "0xFFFFFF"
TILE2_BG_DISABLED_COLOR: "0x939391"
TILE2_TITLE_ACTIVE_COLOR: "0x000000"
TILE2_TITLE_DISABLED_COLOR: "0xFFFFFF"
TILE2_VALUE_ACTIVE_COLOR: "0x7A7A7C"
TILE2_VALUE_DISABLED_COLOR: "0xD9D9D9"
# advanced
TILE2_TAP_SERVICE: ""
TILE2_TAP_PARAM_KEY: ""
TILE2_TAP_PARAM_VAL: ""
TILE2_LONGPRESS_SLIDER: "auto"
TILE2_LONGPRESS_OFF_VALUE: "0"
TILE2_LONGPRESS_ACTION: ""
TILE2_LONGPRESS_ACTION_TYPE: ""
TILE2_LONGPRESS_ACTION_SERVICE: ""
```

---

### Cover — open/close toggle + position slider

```yaml
TILE3_ENTITY: "cover.living_room_blinds"
TILE3_STATE_ENTITY: "cover.living_room_blinds"
TILE3_TITLE: "Blinds"
TILE3_ICON: "\U000F111C"          # mdi:blinds
TILE3_TYPE: "cover"
TILE3_TAP_ACTION: "auto"          # opens if closed, closes if open
TILE3_LONGPRESS: "slider"         # long press opens position slider
TILE3_VALUE_MODE: "auto"          # shows "Open" / "Closed" (text mode for covers)
TILE3_LABEL_OFF: "Closed"
TILE3_LABEL_ON: "Open"
# colors — blue theme for covers
TILE3_CIRCLE_ACTIVE_COLOR: "0x00C5EC"
TILE3_CIRCLE_DISABLED_COLOR: "0x7B7B6F"
TILE3_ICON_ACTIVE_COLOR: "0xFFFFFF"
TILE3_ICON_DISABLED_COLOR: "0x00C5EC"
TILE3_BG_ACTIVE_COLOR: "0xFFFFFF"
TILE3_BG_DISABLED_COLOR: "0x939391"
TILE3_TITLE_ACTIVE_COLOR: "0x000000"
TILE3_TITLE_DISABLED_COLOR: "0xFFFFFF"
TILE3_VALUE_ACTIVE_COLOR: "0x7A7A7C"
TILE3_VALUE_DISABLED_COLOR: "0xD9D9D9"
# advanced
TILE3_TAP_SERVICE: ""
TILE3_TAP_PARAM_KEY: ""
TILE3_TAP_PARAM_VAL: ""
TILE3_LONGPRESS_SLIDER: "auto"    # auto = cover_position for covers
TILE3_LONGPRESS_OFF_VALUE: "0"
TILE3_LONGPRESS_ACTION: ""
TILE3_LONGPRESS_ACTION_TYPE: ""
TILE3_LONGPRESS_ACTION_SERVICE: ""
```

---

### Fan — toggle preset on tap + speed slider on long press

```yaml
TILE4_ENTITY: "fan.air_purifier"
TILE4_STATE_ENTITY: "fan.air_purifier"
TILE4_TITLE: "Purifier"
TILE4_ICON: "\U000F0D43"          # mdi:air-purifier
TILE4_TYPE: "fan"
TILE4_TAP_ACTION: "fan_toggle_preset"  # off→on with preset, on→off
TILE4_LONGPRESS: "slider"              # long press opens speed slider
TILE4_VALUE_MODE: "auto"               # shows preset name when on, "Off" when off
TILE4_LABEL_OFF: "Off"
TILE4_LABEL_ON: "On"
# colors — blue theme for fan
TILE4_CIRCLE_ACTIVE_COLOR: "0x00C5EC"
TILE4_CIRCLE_DISABLED_COLOR: "0x7B7B6F"
TILE4_ICON_ACTIVE_COLOR: "0xFFFFFF"
TILE4_ICON_DISABLED_COLOR: "0x00C5EC"
TILE4_BG_ACTIVE_COLOR: "0xFFFFFF"
TILE4_BG_DISABLED_COLOR: "0x939391"
TILE4_TITLE_ACTIVE_COLOR: "0x000000"
TILE4_TITLE_DISABLED_COLOR: "0xFFFFFF"
TILE4_VALUE_ACTIVE_COLOR: "0x7A7A7C"
TILE4_VALUE_DISABLED_COLOR: "0xD9D9D9"
# advanced — fan_toggle_preset requires SERVICE + PARAM
TILE4_TAP_SERVICE: "fan.toggle"
TILE4_TAP_PARAM_KEY: "preset_mode"
TILE4_TAP_PARAM_VAL: "Auto"       # preset to activate when turning on
TILE4_LONGPRESS_SLIDER: "auto"    # auto = percentage for fans
TILE4_LONGPRESS_OFF_VALUE: "0"
TILE4_LONGPRESS_ACTION: ""
TILE4_LONGPRESS_ACTION_TYPE: ""
TILE4_LONGPRESS_ACTION_SERVICE: ""
```

---

### Scene — activate on tap

```yaml
TILE5_ENTITY: "scene.movie_night"
TILE5_STATE_ENTITY: "scene.movie_night"  # scenes are always "on" so the tile stays active-styled
TILE5_TITLE: "Movie"
TILE5_ICON: "\U000F0E8B"          # mdi:movie-open
TILE5_TYPE: "scene"
TILE5_TAP_ACTION: "auto"          # activates the scene
TILE5_LONGPRESS: "none"
TILE5_VALUE_MODE: "text"          # always shows LABEL_ON for scenes
TILE5_LABEL_OFF: "Scene"
TILE5_LABEL_ON: "Scene"
# colors — green theme
TILE5_CIRCLE_ACTIVE_COLOR: "0x4CAF50"
TILE5_CIRCLE_DISABLED_COLOR: "0x7B7B6F"
TILE5_ICON_ACTIVE_COLOR: "0xFFFFFF"
TILE5_ICON_DISABLED_COLOR: "0x4CAF50"
TILE5_BG_ACTIVE_COLOR: "0xFFFFFF"
TILE5_BG_DISABLED_COLOR: "0x939391"
TILE5_TITLE_ACTIVE_COLOR: "0x000000"
TILE5_TITLE_DISABLED_COLOR: "0xFFFFFF"
TILE5_VALUE_ACTIVE_COLOR: "0x7A7A7C"
TILE5_VALUE_DISABLED_COLOR: "0xD9D9D9"
# advanced
TILE5_TAP_SERVICE: ""
TILE5_TAP_PARAM_KEY: ""
TILE5_TAP_PARAM_VAL: ""
TILE5_LONGPRESS_SLIDER: "auto"
TILE5_LONGPRESS_OFF_VALUE: "0"
TILE5_LONGPRESS_ACTION: ""
TILE5_LONGPRESS_ACTION_TYPE: ""
TILE5_LONGPRESS_ACTION_SERVICE: ""
```

---

### Script — run on tap

```yaml
TILE6_ENTITY: "script.good_morning"
TILE6_STATE_ENTITY: "script.good_morning"
TILE6_TITLE: "Morning"
TILE6_ICON: "\U000F0A72"          # mdi:weather-sunny
TILE6_TYPE: "script"
TILE6_TAP_ACTION: "auto"          # runs the script
TILE6_LONGPRESS: "none"
TILE6_VALUE_MODE: "text"
TILE6_LABEL_OFF: "Run"
TILE6_LABEL_ON: "Run"
# colors — orange theme
TILE6_CIRCLE_ACTIVE_COLOR: "0xFF6F00"
TILE6_CIRCLE_DISABLED_COLOR: "0x7B7B6F"
TILE6_ICON_ACTIVE_COLOR: "0xFFFFFF"
TILE6_ICON_DISABLED_COLOR: "0xFF6F00"
TILE6_BG_ACTIVE_COLOR: "0xFFFFFF"
TILE6_BG_DISABLED_COLOR: "0x939391"
TILE6_TITLE_ACTIVE_COLOR: "0x000000"
TILE6_TITLE_DISABLED_COLOR: "0xFFFFFF"
TILE6_VALUE_ACTIVE_COLOR: "0x7A7A7C"
TILE6_VALUE_DISABLED_COLOR: "0xD9D9D9"
# advanced
TILE6_TAP_SERVICE: ""
TILE6_TAP_PARAM_KEY: ""
TILE6_TAP_PARAM_VAL: ""
TILE6_LONGPRESS_SLIDER: "auto"
TILE6_LONGPRESS_OFF_VALUE: "0"
TILE6_LONGPRESS_ACTION: ""
TILE6_LONGPRESS_ACTION_TYPE: ""
TILE6_LONGPRESS_ACTION_SERVICE: ""
```

---

### Long press — call a different entity

Short tap toggles a light; long press activates a scene.

```yaml
TILE1_ENTITY: "light.living_room"
TILE1_TYPE: "light"
TILE1_TAP_ACTION: "auto"           # short tap: toggle the light
TILE1_LONGPRESS: "action"          # long press: fire a different entity
# ...
TILE1_LONGPRESS_ACTION: "scene.movie_night"
TILE1_LONGPRESS_ACTION_TYPE: "scene"
TILE1_LONGPRESS_ACTION_SERVICE: "" # empty = default activate for scene
```

Short tap toggles a light; long press also toggles a different light.

```yaml
TILE1_LONGPRESS: "action"
TILE1_LONGPRESS_ACTION: "light.bedroom"
TILE1_LONGPRESS_ACTION_TYPE: ""    # empty = inherits "light" from TILE1_TYPE
TILE1_LONGPRESS_ACTION_SERVICE: "light.toggle"  # must set toggle explicitly, default is turn_on
```

---

## Finding Material Design Icons

1. Go to [pictogrammers.com/library/mdi](https://pictogrammers.com/library/mdi/)
2. Search for an icon name
3. Click the icon → copy the Unicode codepoint (e.g. `F0769`)
4. Format it as `"\U000F0769"` (always 8 hex digits, padded with leading zeros)

Make sure the glyph is listed in the `glyphs:` section of the MDI font definition in the YAML (search for `materialdesign_icons`). If you use an icon that is not listed there, it will show as a blank space.
