# Changelog

All notable changes to the ESPHome configs are listed here.
Pure documentation and README updates are not included.

---

## 2026-05-31

### Added
- Home-like light color controls: long-press brightness overlays now show a color-ring button for color-capable lights and open a color/temperature detail view with preset colors, Kelvin presets, and a brightness slider for both CYD and external ILI9341 variants. The Color/Temp switch is hidden automatically when a light only supports one mode. Thanks @Dominik-1980 for the initial push on this feature.
- Home-like climate tile support: `TILE*_TYPE: climate` now provides a thermostat-style long-press overlay with target temperature ring, +/- controls, current temperature/humidity display, clickable HVAC mode selection, half-degree temperature steps, live HA sync while open, and per-tile `TILE*_CLIMATE_*` substitutions for range, step size, and HA attribute mapping.

---

## 2026-05-02

### Docs
- Added assembly guides for both hardware variants (`3d_print/ESP32-2432S028/ASSEMBLY.md` and `3d_print/ILI9341-only-display/ASSEMBLY.md`) covering print parts, mounting order, power wiring, and display installation

---

## 2026-04-26

### Fixed
- Duplicate MDI glyph error when the same icon is used on multiple tiles — font glyph list is now driven by a dedicated `MDI_GLYPH_1..6` substitution block instead of the per-tile `TILE*_ICON` variables, so each glyph appears exactly once regardless of how many tiles share it

---

## 2026-04-22

### Fixed
- Display platform migrated from `ili9xxx` to `mipi_spi` — fixes white screen on ESPHome 2026.4+
- Removed `miso_pin` from LCD SPI bus (not required for display, caused issues with newer ESPHome)
- Removed `color_palette: 8BIT` (not supported by `mipi_spi`)

---

## 2026-04-02

### Added
- `TILE*_LONGPRESS` mode selector: `none | slider | action` — replaces the previous implicit dual-mode behavior
- Long-press `action` mode: `TILE*_LONGPRESS_ACTION*` substitutions allow targeting a completely different entity on long press than the short tap
- `secrets.yaml.example` — template for all required secret keys; all YAMLs now use `!secret` references instead of hardcoded placeholders

### Fixed
- Long-press events not appearing in Home Assistant history — publish delay increased from 200ms to 500ms
- Double-tap registered when `DIM_BRIGHTNESS` equals `MAX_BRIGHTNESS` — touch-blocking overlay is now only shown when the display actually dims

### Changed
- `esphome/` folder reorganized: `home-like/` and `buttons/` are now the top-level UI variant folders, each containing `cyd-2432s028/` and `ili9341-external-esp32/` subfolders
- Credentials (`api.encryption.key`, `ota.password`, WiFi) moved to `secrets.yaml` across all variants

---

## 2026-04-01

### Added
- Orientation support for home-like UI: four presets (0° / 90° / 180° / 270°) with correct display transform, touch transform, grid layout, and background image selection

---

## 2026-03-31

### Added
- Cover tile support: `TILE*_TYPE: cover` with open/close tap action and position slider on long press

---

## 2026-03-25

### Added
- Auto-dim night mode: configurable time range during which the display dims to a lower brightness level (`NIGHT_DIM_START` / `NIGHT_DIM_END` / `NIGHT_DIM_BRIGHTNESS`)

---

## 2026-03-13

### Added
- Configurable 12/24h time format via `TIME_24H` substitution

---

## 2026-03-07

### Added
- Long-press control overlay for home-like UI: brightness slider (light), speed slider (fan), position slider (cover)
- Tile substitutions restructured to support per-tile type, tap action, value mode, and color configuration

---

## 2026-02-24

### Added
- Home-like UI variant (`home-like.yaml`): 2×3 tile grid with wallpaper background, icon, title, and value per tile
- Each tile publishes `tileN_press` / `tileN_long_press` action strings to Home Assistant

---

## 2026-02-21

### Changed
- Migrated from display lambda rendering to LVGL-based UI across all variants
- Real-time state mirroring from Home Assistant via `on_state` triggers and central `ui_refresh` script

---

## 2025-02-28

### Fixed
- Display rendering broken since ESPHome 2025.2 (thanks @3DJupp)
