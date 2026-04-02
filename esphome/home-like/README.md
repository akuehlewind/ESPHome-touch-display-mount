# Home-Like UI — Tile Grid Layout

This folder contains the main, actively maintained UI: a wallpaper-backed 2×3 tile grid inspired by modern smart home apps.

> **Looking for the simple version?**
> A minimal lockscreen with 4 round buttons is available in [`../buttons/`](../buttons/).

---

## What this UI does

- 2×3 grid of configurable tiles, each with icon, title, and value line
- Supports lights, fans, switches, covers, scenes, and scripts per tile
- Real-time state synchronization with Home Assistant
- Long press opens a value overlay (brightness / fan speed / cover position slider)
- Long press can alternatively fire a completely different HA entity than the short tap
- Orientation presets: 0°, 90°, 180°, 270° — uncomment to switch
- Auto-dim and night mode with configurable brightness levels
- Optional direct HA service calls or automation-only mode

---

## Configuration reference

All tile substitutions are documented in [TILE_CONFIGURATION.md](TILE_CONFIGURATION.md). It covers:

- Every substitution key with description and default value
- All tap actions, long press modes, and value modes explained
- Complete copy-paste examples for: light, switch, cover, fan, scene, script
- Long press examples: slider overlay and independent entity action
- How to find and format Material Design Icons

Start there if you are setting up tiles for the first time.

---

## Picking your hardware variant

You need to pick **one** YAML file that matches your hardware:

### `cyd-2432s028/home-like.yaml` — Cheap Yellow Display (CYD)

The **ESP32-2432S028**, commonly known as the **Cheap Yellow Display (CYD)**, is an all-in-one board with the ESP32, ILI9341 display, XPT2046 touchscreen, and backlight all on a single PCB. It is the easiest and most common choice for this project — just one USB cable, no manual wiring.

Use this file if you have a standalone ESP32-2432S028 board.

### `ili9341-external-esp32/home-like.yaml` — Standalone ILI9341 + external ESP32

Use this file if you have a **separate ILI9341 display module** wired to a generic ESP32 board (e.g. ESP32 DevKit / Wroom 32D). This requires manual wiring according to the pin table in the main README.

---

## Credentials — secrets.yaml

All sensitive values (API key, OTA password, WiFi credentials) are referenced via ESPHome's `!secret` system, so nothing sensitive is stored in the YAML itself.

Copy `../secrets.yaml.example` to `secrets.yaml` (one level up, next to your ESPHome config root) and fill in your values:

```yaml
wifi_ssid: "Your WiFi Network Name"
wifi_password: "your_wifi_password"
smartdisplay_api_key: "your_32_byte_base64_api_key"
ota_password: "your_ota_password"
wifi_ap_password: "your_fallback_password"
```

`secrets.yaml` is gitignored and never committed.

---

## Required assets — copy these alongside your YAML

When you copy the YAML into ESPHome, you also need to copy the following folders **next to the YAML file** (or adjust the paths inside the YAML):

| Folder | Contents | Required by |
|--------|----------|-------------|
| `fonts/` | `materialdesignicons-webfont.ttf` | Icon glyphs on every tile |
| `images/` | `smartdisplay_background.png`, `smartdisplay_background_90.png` | Background wallpaper |

Both folders are included here and work with both hardware variants.

- `smartdisplay_background.png` — used for 0° and 180° (landscape)
- `smartdisplay_background_90.png` — used for 90° and 270° (portrait)

The active image is selected automatically via the `BG_IMAGE` substitution in the ORIENTATION preset block inside the YAML.
