# ESP32 Cheap Yellow Display (ESP32-2432S028) Home Assistant Touch Panel – LVGL UI + 3D Printed Desk / Under-Desk Tilt Mount
![ESPHome](https://img.shields.io/badge/ESPHome-Compatible-blue)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Integrated-orange)
![LVGL](https://img.shields.io/badge/LVGL-UI-green)
![Cheap Yellow Display](https://img.shields.io/badge/CYD-Supported-yellow)

ESPHome powered Home Assistant control panel using the popular Cheap Yellow Display (ESP32-2432S028).

A 3D-printable enclosure with adjustable tilt for an ESP32 2.8" ILI9341 touchscreen, powered by ESPHome + LVGL and integrated with Home Assistant.  
Includes ready-to-flash YAML configs for the ESP32-2432S028 (Cheap Yellow Display) and a standalone ILI9341 + external ESP32 wiring variant.

Three mounting options are supported:

• Desk Mount – for tabletop installation  
• Under-Desk Mount – for installation underneath a desk or shelf  
• Wall Mount – for wall installation (CYD version only)

The enclosure can therefore be used as a compact Home Assistant control panel for desks, walls, or mounted underneath desk or furniture.

------------------------------------------------------------------------

The ESP32-2432S028 Cheap Yellow Display is transformed into a tiltable Home Assistant control panel powered by ESPHome and LVGL.

The goal of this project is to create a clean, wall- or desk-mounted smart control panel that integrates seamlessly with Home Assistant.  
It features a modern LVGL-based UI, real-time state synchronization, and optional direct Home Assistant service calls.

The enclosure is designed for:

- Clean cable routing  
- Adjustable viewing angle (±35° tilt)  
- Minimal footprint  
- Professional, integrated appearance  

Two hardware variants are supported:

- ✅ **ESP32-2432S028 (Cheap Yellow Display / CYD)**
- ✅ **Standalone ILI9341 + External ESP32 wiring variant**

The ESP32-2432S028 integrates the ESP32, ILI9341 display, touchscreen controller, and backlight circuitry on a single board.  
It is commonly known as the **Cheap Yellow Display (CYD)** in the maker community and is the easiest option for this project.

<img src="images/display-home-like.png" width="100%">
<img src="images/display-home-like-overlay.png" width="100%">

<img src="images/display-buttons.png" width="50%">
<img src="images/desk-mount.jpeg" width="50%">

<img src="images/tilt.jpeg" width="50%">
<img src="images/side.jpeg" width="50%">

## Demo
https://github.com/user-attachments/assets/68709d8e-21ec-4851-bf73-70f583027390

Note: The video quality looks worse than in real life because the display refresh rate interferes with the camera frame rate when filming the screen. In reality the UI looks much smoother and cleaner.

---

# 🆕 Architecture Overview (LVGL-Based UI)

This project uses ESPHome LVGL instead of a classic display lambda approach.

### Core Concepts

- UI rendered fully using LVGL widgets
- 4 configurable touch buttons
- Real-time state mirroring from Home Assistant
- Central `ui_refresh` script keeps UI synchronized
- Optional direct Home Assistant service calls
- Action sensor for automation-based control

### Two Control Modes

#### 1️⃣ Direct Mode (Default – Plug & Play)

If `DIRECT_ACTIONS` is set to `"true"` (default):

- Touch publishes an action string
- AND directly calls a Home Assistant service  
  (default: `light.toggle`)

No Home Assistant automations required.

#### 2️⃣ Automation Mode

If `DIRECT_ACTIONS` is set to `"false"`:

- Touch ONLY publishes an action string
- You implement behavior in Home Assistant automations

Trigger example:
```
Entity: sensor.smartdisplay_action
To: btn1_press
```

### Overlay Control Layer

The UI contains a secondary overlay layer used for interactive controls.

When a tile is long-pressed:

1. The overlay opens
2. The control type is determined (brightness / percentage)
3. Slider interaction updates Home Assistant
4. Closing the overlay returns to the tile grid

This keeps the tile UI clean while still allowing detailed control.


# ⚙️ Requirements

- Home Assistant installed
- ESPHome Add-on installed
- ESPHome 2026.2+
- Home Assistant 2026.2+
- Basic ESPHome knowledge
- 3D printer access (optional)

---

# 🧪 Tested With

This project was tested using:

- **ESPHome 2026.2+**
- **Home Assistant 2026.2+**
- ESP32-2432S028 (Cheap Yellow Display / CYD)
- ILI9341 + XPT2046 standalone wiring variant

# ⚠️ IMPORTANT -- Enable "Actions" in Home Assistant

Because this project uses:

```
homeassistant.action → light.toggle
```

You must allow the ESPHome device to perform Home Assistant service calls.

### How to enable:

1. Open Home Assistant  
2. Go to **Settings**  
3. Click **Devices & Services**  
4. Open the **ESPHome integration**  
5. Select your device  
6. Enable:

> "Allow the device to perform Home Assistant actions"

Without this setting, direct control will not work.

---

# ✨ Features

- LVGL-based UI (lockscreen or tile layout depending on configuration)
- 2×3 configurable tile layout (home-like UI)
- Long-press control overlay for brightness / fan speed
- Real-time state synchronization with Home Assistant
- Optional direct Home Assistant service calls
- Automation-based mode supported
- Adjustable 3D-printed enclosure
- Desk mount or under-desk mount options
- Hidden cable routing
- Works with CYD or external ESP32 wiring

------------------------------------------------------------------------

# 📦 Hardware

## Option A -- ESP32-2432S028 (Cheap Yellow Display Board -- Recommended)

| Part                         | Price   | Comment                                         |
|------------------------------|---------|-------------------------------------------------|
| ESP32-2432S028			   | 15$     | 2.8" ILI9341 with integrated ESP                |
| 2x M4 3.5x16 screw (flathead)| 0.10$   | Screws to connect the mount case with the base  |

Advantages:

-   Single USB cable
-   Clean installation
-   No manual wiring
-   Widely available under the name "Cheap Yellow Display"

| ESP32-2432S028               | PIN          | Comment                                              |
|------------------------------|--------------|------------------------------------------------------|
| LCD                          |              |                                                      |
| clk_pin                      | GPIO14       | SPI LCD Clock                                        |
| mosi_pin                     | GPIO13       | SPI LCD MOSI (sometimes also labeled as SDI)         |
| miso_pin   	    		   | GPIO12       | SPI LCD MISO (sometimes also labeled as SDO	         |
| cs_pin                       | GPIO15       | Display CS                                           |
| dc_pin                       | GPIO2        | Display DC                                           |
| Touchscreen                  |              |                                                      |
| clk_pin                      | GPIO25       | SPI Touchscreen Clock                                |
| mosi_pin                     | GPIO32       | SPI Touchscreen MOSI (sometimes also labeled as DIN) |
| miso_pin                     | GPIO39       | SPI Touchscreen MISO (sometimes also labeled as DO)  |
| cs_pin                       | GPIO33       | Touchscreen CS                                       |
| interrupt_pin                | GPIO36       | Touchscreen Interrupt                                |
| LED                          |              |                                                      |
| ledc                         | GPIO21       | Backlight LED display                                |
| ledc                         | GPIO4        | Onboard LED (not used for this project)              |

------------------------------------------------------------------------

## Option B -- Standalone Display + ESP32

| Part                         | Price   | Comment                                         |
|------------------------------|---------|-------------------------------------------------|
| 2.8" ILI9341    			   | 8$      | 2.8" ILI9341                                    |
| ESP32 Wroom 32D   		   | 8$      | or some ESP32                                   |
| 2x M4 3.5x16 screw (flathead)| 0.10$   | Screws to connect the mount case with the base  |

Requires manual wiring according to the YAML pin configuration.

Use the wiring table below, which shows how to put everything together.

| ILI9341                      | PIN  ESP32   | Comment                                              |
|------------------------------|--------------|------------------------------------------------------|
| GND                          | GND          | Ground                                               |
| VCC                          | 3.3V         | Power                                                |
| LCD                          |              |                                                      |
| SCK                          | GPIO18       | SPI LCD Clock                                        |
| SDI (MOSI)                   | GPIO23       | SPI LCD MOSI (sometimes also labeled as SDI)         |
| SDO (MISO)   	    	   | GPIO19       | SPI LCD MISO (sometimes also labeled as SDO	         |
| CS                           | GPIO27       | Display CS                                           |
| D/C                          | GPIO26       | Display DC                                           |
| RESET                        | GPIO16        | Display Reset                                        |
| Touchscreen                  |              |                                                      |
| T_CLK                        | GPIO25       | SPI Touchscreen Clock                                |
| T_DO                         | GPIO35       | SPI Touchscreen MISO (sometimes also labeled as DO) |
| T_DIN                        | GPIO32       | SPI Touchscreen MOSI (sometimes also labeled as DIN)  |
| T_CS                         | GPIO33       | Touchscreen CS                                       |
| T_IRQ                        | GPIO34       | Touchscreen Interrupt                                |
| LED                          |              |                                                      |
| ledc                         | GPIO4        | Backlight LED display                                |

------------------------------------------------------------------------

# 🚀 Installation

1.  Open ESPHome in Home Assistant
2.  Create a new device
3.  Copy the appropriate YAML file
4.  Edit the **USER CONFIG / substitutions** section at the top of the YAML (entities, labels, icons)
5.  Adjust WiFi credentials
6.  Flash the device

Available YAML variants (pick **one UI** for your **hardware**):

### Cheap Yellow Display (ESP32-2432S028 / CYD)
- `esphome/cyd-2432s028/buttons.yaml` – lockscreen with 4 round buttons (good for quick toggles)
- `esphome/cyd-2432s028/home-like.yml` – 2x3 “tiles” UI (wallpaper + tiles)

### External display wiring (any ESP32 + ILI9341 + XPT2046)
- `esphome/ili9341-external-esp32/buttons.yaml` – lockscreen with 4 round buttons
- `esphome/ili9341-external-esp32/home-like.yml` – 2x3 “tiles” UI (wallpaper + tiles)

> Tip: Both `home-like.yml` files share a single background image located at `esphome/images/smartdisplay_background.png`.
> Because the YAML files live in subfolders, reference it as `../images/smartdisplay_background.png` in the `image:` section.


------------------------------------------------------------------------

### Assets (required for the UI)
- **Material Design Icons font**: place `materialdesignicons-webfont.ttf` at `esphome/fonts/materialdesignicons-webfont.ttf`.
  - You can get it from the MaterialDesignIcons project (TTF release) or your local install.
  - If you change icons in the YAML, ensure the glyph list contains them.
- **Background image (home-like UI)**:
  - Default file: `esphome/images/smartdisplay_background.png` (included in this repo).
  - In `home-like.yml`, reference it as `/images/smartdisplay_background.png` (relative to the YAML file).
# ⚙️ UI mapping (USER CONFIG)

Your config choice defines the UI style:

## `buttons.yaml` (lockscreen like buttons)
- Buttons: BTN1..BTN4
- Action strings: `btn1_press` .. `btn4_press`
- Default direct action (if `DIRECT_ACTIONS: "true"`): `light.toggle` on `BTN*_ENTITY`

<img src="images/display-buttons.png" width="50%">


## `home-like.yml` (tiles)
- Tiles: TILE1..TILE6
- Action strings: `tile1_press` .. `tile6_press`
- Each tile can optionally call a Home Assistant service directly (toggle/scene/script/fan preset/light brightness).
- Per-tile OFF label is configurable via `TILE*_LABEL_OFF` (e.g. "Off" / "Aus").

### Long-Press Overlay

Tiles can open a control overlay when long-pressed.

Depending on the tile configuration, the overlay provides:

| Tile Type | Overlay Control |
|-----------|----------------|
| light     | Brightness slider |
| fan       | Speed / percentage slider |
| custom    | Optional custom behavior |

The overlay allows quick adjustments without leaving the tile UI.

<img src="images/display-home-like.png" width="50%">
<img src="images/display-home-like-overlay.png" width="50%">

------------------------------------------------------------------------

# 🧠 System Flow

### Direct Mode

1. Button pressed  
2. Action string published  
3. Home Assistant service called  
4. Entity state updated  
5. Mirrored back into ESPHome  
6. UI refreshed  

### Automation Mode

1. Button pressed  
2. Action string published  
3. Home Assistant automation triggers  
4. Automation controls device  
5. State mirrored back  


------------------------------------------------------------------------

# 🔧 Customization

For most setups, you only need to edit the **USER CONFIG / substitutions** section in the YAML.

You can also:

-   Add more buttons / pages
-   Change service calls (the default is `light.toggle`)
-   Modify fonts and icon glyphs
-   Adjust colors / theme
-   Date localization can be adjusted in the ui_refresh script (days[] / months[] arrays).
-   Adjust transforms for your panel

If touch alignment is wrong, ensure `display.transform` and `touchscreen.transform`
are **identical** (they must always match).

------------------------------------------------------------------------
# Troubleshooting

### Corrupted / garbled display output

If the display shows **corrupted graphics, horizontal lines, or random pixels**, the most common cause is a **different display controller on your Cheap Yellow Display**.

Most CYD boards use **ILI9341**, but some variants ship with **ST7789** or **ILI9342**.  
If the driver in ESPHome does not match the controller, the display output may look broken.

Try changing the display model in the YAML:

```yaml
display:
  - platform: ili9xxx
    model: ILI9341
```

Alternative models that may work depending on your board:

```yaml
model: ST7789V
```

or

```yaml
model: ILI9342
```

Flashing with a different model is usually the fastest way to identify the correct controller.

---

### Display rotated / mirrored

If the UI appears **rotated, mirrored, or upside down**, adjust the `transform`
settings.

Both the **display** and the **touchscreen** configuration must match,
otherwise touch input will be misaligned.

Common parameters:

```yaml
transform:
  swap_xy: true
  mirror_x: true
  mirror_y: true
```
Depending on the board variant, you may need to experiment with these values until the orientation matches your screen.
Make sure the display.transform and touchscreen.transform settings use the same orientation.

------------------------------------------------------------------------

# 🖨 3D Printing

See the `/3d_print` folder for STL files and Fusion 360 source files.

Three mount options are available:

• **Desk Mount** – for tabletop installation  
• **Under-Desk Mount** – for installation underneath a desk or shelf  
• **Wall Mount** – for wall installation (CYD version)

Desk mount and Under-Desk mount use the same enclosure parts and adjustable tilt mechanism.  
Print the main enclosure parts and choose one mount type depending on your setup.

Designed for:

- Desk mounting
- Under-desk mounting
- Wall mounting
- Adjustable viewing angle
- Hidden cable routing

------------------------------------------------------------------------

# ⚠️ Disclaimer

This is a hobby project and considered work-in-progress.

Feel free to fork, remix and improve it.

# Other
<img src="images/display.jpeg" width="50%">
<img src="images/standalone-display.jpeg" width="50%">
<img src="images/fusion2.jpg" width="50%">
<img src="images/integrated-display-back.jpeg" width="50%">



# More projects
Looking for more cool projects using this display? Check out the [LoctekMotion Touch Display GitHub repository](https://github.com/3DJupp/LoctekMotion-TouchDisplay)! This awesome project takes the same 2.8" ILI9341 touchscreen setup and repurposes it—not for lights, but for controlling a height-adjustable Flexispot desk with ease. If you're into smart home automation and custom builds, it's definitely worth a look!

