# Assembly Guide – ESP32-2432S028 (CYD)

## Parts to print

| File | Qty | Notes |
|------|-----|-------|
| `inner-body.stl` | 1 | Holds the display |
| `outer-body.stl` | 1 | Outer shell |
| `locking-body.stl` | 1 | Connects inner body to the mount and locks the viewing angle |
| Mount variant (see below) | 1 | Pick one from `variants/` |

**Mount variants** (print one):

| Variant | File |
|---------|------|
| Desk mount | `variants/desk-mount/desk-mount.stl` |
| Under-desk mount | `variants/under-desk-mount/under-desk-mount.stl` |
| Wall mount | `variants/wall-mount/wall-mount-touch-display.stl` |
| Flush mount (EU box) | `variants/flush-mount/flush-mount-body.stl` |

---

## Assembly steps

### 1. Attach the locking body to the mount

Insert the **locking body** into the round hole of the desk or under-desk mount base.

### 2. Place the inner body

Set the **inner body** on top of the locking body.

### 3. Screw inner body to locking body

Insert the **2× M4 3.5×16 flathead screws** through the openings in the inner body and tighten them into the locking body.

Tighten until the display holds its position firmly but can still be tilted by hand — then adjust to your preferred viewing angle and tighten fully so it stays in place.

### 4. Connect and thread the power cable

The CYD ships with a small JST power cable. Plug the JST connector into the **VIN / 5V port** on the CYD board.

⚠️ The CYD has multiple identical-looking JST connectors with different voltages. Always check the label — connecting 5V to a 3.3V port will damage the board.

Feed the **red and black wires through the cable hole** in the inner body before inserting the display. Once the display is in place this is no longer easy to do.

On the loose end, connect the **red wire to 5V** and the **black wire to GND** of an old USB-A cable. Solder or use small wire connectors.

> The side USB connector on the CYD is not used — routing power via the JST cable keeps the enclosure compact with no protruding connector.

### 5. Insert the display

Slide the CYD board into the inner body with the screen facing forward.

### 6. Close the enclosure

Press the **outer body** onto the inner body until it clicks into place.

---

## Tips

- Print all parts without supports.
- The desk mount and under-desk mount use the same enclosure parts — only the base differs.
