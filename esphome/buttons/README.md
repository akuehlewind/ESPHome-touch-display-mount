# Buttons UI — Simple Lockscreen Layout

This folder contains the original "buttons" UI: a minimal lockscreen with a clock and 4 round toggle buttons.

> **Looking for the full-featured UI?**
> The actively maintained version is in [`../home-like/`](../home-like/).
> It offers a 2×3 tile grid, per-tile entity types, brightness/fan/cover sliders, long press actions, orientation presets, and much more.

---

## What this UI does

- Displays a clock (time + date)
- 4 configurable round buttons, each mapped to a Home Assistant entity
- Each tap publishes `btn1_press` … `btn4_press` to `sensor.smartdisplay_action`
- Optional direct HA service calls (light.toggle by default)

## What it does NOT have

- No tile grid or wallpaper background
- No per-button entity types (lights only by default)
- No value/state display per button
- No brightness, fan speed, or cover position sliders
- No long press actions
- No orientation presets
- No auto-dim or night mode

## When to use it

If you want the absolute simplest possible setup — just 4 buttons that toggle lights — this is the quickest way to get started. Configuration is minimal: set 4 entity IDs and flash.

For anything beyond basic toggles, use the [home-like UI](../home-like/) instead.

## Picking your hardware variant

You need to pick **one** YAML file that matches your hardware:

### `cyd-2432s028/buttons.yaml` — Cheap Yellow Display (CYD)

The **ESP32-2432S028**, commonly known as the **Cheap Yellow Display (CYD)**, is an all-in-one board with the ESP32, ILI9341 display, XPT2046 touchscreen, and backlight all on a single PCB. Just one USB cable, no manual wiring.

### `ili9341-external-esp32/buttons.yaml` — Standalone ILI9341 + external ESP32

Use this file if you have a **separate ILI9341 display module** wired to a generic ESP32 board. Requires manual wiring according to the pin table in the main README.

---

## Required assets — copy these alongside your YAML

When you copy the YAML into ESPHome, you also need to copy the following folders **next to the YAML file** (or adjust the paths inside the YAML):

| Folder | Contents | Required by |
|--------|----------|-------------|
| `fonts/` | `materialdesignicons-webfont.ttf` | Button icons |
| `images/` | `smartdisplay_background.png` | Background wallpaper |

Both folders are included here and work with both hardware variants.
