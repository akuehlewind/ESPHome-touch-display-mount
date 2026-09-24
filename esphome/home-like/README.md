# Home-Like UI — Tile Grid Layout

This folder contains the main, actively maintained UI: a wallpaper-backed 2×3 tile grid inspired by modern smart home apps.

Requires **ESPHome 2026.9.0 or newer**. For existing password-based devices, follow the [two-step wireless OTA migration](../../README.md#encrypted-ota-and-migration), or install over USB/serial.

> **Looking for the simple version?**
> A minimal lockscreen with 4 round buttons is available in [`../buttons/`](../buttons/).

---

## What this UI does

- 2×3 grid of configurable tiles, each with icon, title, and value line
- Supports lights, fans, switches, covers, climate entities, scenes, and scripts per tile
- Real-time state synchronization with Home Assistant
- Long press opens a value overlay (brightness / fan speed / cover position / climate temperature)
- Color-capable lights can open a color/temperature picker from the brightness overlay
- Long press can alternatively fire a completely different HA entity than the short tap
- Orientation presets: 0°, 90°, 180°, 270° — uncomment to switch
- Auto-dim and night mode with configurable brightness levels
- Optional direct HA service calls or automation-only mode
- Optional short-tap OFF confirmation per tile; defaults to disabled and leaves long presses independent

---

## Configuration reference

All tile substitutions are documented in [TILE_CONFIGURATION.md](TILE_CONFIGURATION.md). It covers:

- Every substitution key with description and default value
- All tap actions, long press modes, and value modes explained
- Complete copy-paste examples for: light, switch, cover, fan, climate, scene, script
- Long press examples: slider overlay and independent entity action
- How to find and format Material Design Icons

Start there if you are setting up tiles for the first time.

---

## Picking your hardware variant

You need to pick **one** YAML file that matches your hardware:

### `cyd-2432s028/home-like.yaml` — Cheap Yellow Display (CYD)

The **ESP32-2432S028**, commonly known as the **Cheap Yellow Display (CYD)**, is an all-in-one board with the ESP32, ILI9341 display, XPT2046 touchscreen, and backlight all on a single PCB. It is the easiest and most common choice for this project — just one USB cable, no manual wiring.

Use this file if you have a standalone ESP32-2432S028 board.

In the **DISPLAY / ORIENTATION PRESETS** section, enable exactly one complete block for the controller and orientation you need. `ILI9341 / 0 degrees` is active by default; every ILI9342 block is fully commented out. There is no separate ILI9342 copy and no template logic to edit. Check the actual controller: USB connector type alone is not definitive, and ST7789 variants are not supported by these presets.

| Setting | ILI9341 (default) | ILI9342 |
|---------|-------------------|---------|
| Native dimensions | 240 x 320 | 320 x 240 |
| Display data rate / color order | 10 MHz / BGR, preserving existing behavior | 40 MHz / RGB |
| LVGL rotation relative to orientation | +90 degrees, modulo 360 | +180 degrees, modulo 360 |
| Touch swap X/Y | false | true |
| Touch mirror X / Y | true / false | true / true in landscape; false / false in portrait |
| Touch calibration X / Y minima | 280 / 340 | 340 / 280 |

The ILI9342 contributor reported testing all four orientations on hardware. Its touch mirror settings are empirical, particularly in portrait; a source audit cannot independently verify touch alignment. Hardware verification of the integrated profile by the maintainer is **pending**, so check touch targets and calibration on your own panel. ILI9341 remains the default with its existing settings and behavior.

### `ili9341-external-esp32/home-like.yaml` — Standalone ILI9341 + external ESP32

Use this file if you have a **separate ILI9341 display module** wired to a generic ESP32 board (e.g. ESP32 DevKit / Wroom 32D). This requires manual wiring according to the pin table in the main README.

---

## Credentials — secrets.yaml

All sensitive values (API key and WiFi credentials) are referenced via ESPHome's `!secret` system, so nothing sensitive is stored in the YAML itself. Encrypted OTA inherits the API key.

Copy [`../secrets.yaml.example`](../secrets.yaml.example) to `secrets.yaml` **next to the chosen YAML**, and fill in your values. For example, a config kept at `cyd-2432s028/home-like.yaml` needs `cyd-2432s028/secrets.yaml`. When pasted into Device Builder, use the directory containing that device's YAML.

```yaml
wifi_ssid: "Your WiFi Network Name"
wifi_password: "your_wifi_password"
smartdisplay_api_key: "your_32_byte_base64_api_key"
wifi_ap_password: "your_fallback_password"
```

`secrets.yaml` is gitignored and never committed.

Retain the existing `ota_password` secret only while performing step 1 of the [wireless migration](../../README.md#encrypted-ota-and-migration); the final encrypted config does not use it.

---

## Assets downloaded at build time

The default config downloads its icon font and wallpapers while compiling. No local asset folders are required for this setup; `secrets.yaml` is still required alongside the YAML.

| Substitution | Source | Required by |
|--------------|--------|-------------|
| `MDI_FONT_URL` | Templarian/MaterialDesign-Webfont release `v7.4.47` | Icon glyphs on every tile |
| `ASSET_BASE` | This repository's `esphome/home-like` at public commit `9ffa5dfddf695150ad0388f5f5e2bcf786772195` | Background wallpaper |

The pinned background URL is `https://raw.githubusercontent.com/akuehlewind/ESPHome-touch-display-mount/9ffa5dfddf695150ad0388f5f5e2bcf786772195/esphome/home-like`.

- `smartdisplay_background.png` — used for 0° and 180° (landscape)
- `smartdisplay_background_90.png` — used for 90° and 270° (portrait)

The active image is selected by `BG_IMAGE` in the chosen display/orientation preset inside the YAML.

To use the bundled assets locally, copy this folder's `fonts/` and `images/` next to your chosen YAML and override:

```yaml
ASSET_BASE: '.'
MDI_FONT_URL: 'fonts/materialdesignicons-webfont.ttf'
```

These overrides only localize the icon font and wallpapers. The existing Google Fonts entries still require network access during compilation.

## Short-tap confirmation

Set `TILEn_CONFIRM_OFF: "true"` for a tile that should ask before turning off or closing. It is independent of `TILEn_TAP_ACTION` and defaults to `"false"`. Cancellation, tapping outside, or display dimming emits no event and performs no action. Acceptance publishes `tileN_confirmed_off` and uses an explicit `turn_off`/`close_cover` action, so a state change while the dialog is open cannot turn the entity back on.

The alert question, actions, color controls, and climate labels are localizable through the `UI_*` substitutions. Confirmation questions contain a `{title}` placeholder, which may be moved to match the target language's word order. A complete German example is included in [the configuration reference](TILE_CONFIGURATION.md#interface-text-and-localization).

The guard covers built-in toggle behavior for lights, fans, switches and covers, `fan_toggle_preset`, matching toggle services, and explicit off/close services. Unknown states are treated as potentially on. Scenes, scripts, climate tiles and opaque custom services are not gated because the display cannot know whether they switch something off. Long presses remain independent and are **not protected**. In automation-only mode, handle `tileN_confirmed_off` with an idempotent off/close service. See [the full scope and example](TILE_CONFIGURATION.md#short-tap-off-confirmation).
