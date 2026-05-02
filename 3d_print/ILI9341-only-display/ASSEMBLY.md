# Assembly Guide – Standalone ILI9341 + External ESP32

## Parts to print

| File | Qty | Notes |
|------|-----|-------|
| `inner-body.stl` | 1 | Holds the display; has a cutout on the back for pin access |
| `outer-body.stl` | 1 | Outer shell |
| `locking-body.stl` | 1 | Connects inner body to the mount and locks the viewing angle |
| Mount variant (see below) | 1 | Pick one from `variants/` |

**Mount variants** (print one):

| Variant | File |
|---------|------|
| Desk mount | `variants/desk-mount/desk-mount.stl` |
| Under-desk mount | `variants/under-desk-mount/under-desk-mount.stl` |

---

## Assembly steps

### 1. Attach the locking body to the mount

Insert the **locking body** into the round hole of the desk or under-desk mount base.

### 2. Place the inner body

Set the **inner body** on top of the locking body.

### 3. Screw inner body to locking body

Insert the **2× M4 3.5×16 flathead screws** through the openings in the inner body and tighten them into the locking body.

Tighten until the display holds its position firmly but can still be tilted by hand — then adjust to your preferred viewing angle and tighten fully so it stays in place.

### 4. Wire up the display

Connect the ILI9341 display and ESP32 according to the wiring table in the [main README](../../README.md#option-b----standalone-display--esp32).

The inner body has a **cutout on the back** so all pins remain accessible from behind once the display is installed — no need to route cables through the body.

### 5. Insert the display

Slide the ILI9341 board into the inner body with the screen facing forward. Route any wiring out through the back cutout.

### 6. Close the enclosure

Press the **outer body** onto the inner body until it clicks into place.

---

## Tips

- Print all parts without supports.
- Keep wire connections as short as practical so they sit neatly behind the display.
