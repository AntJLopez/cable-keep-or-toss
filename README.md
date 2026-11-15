# Cable Keep or Toss

A single-page tool for Treedix LED cable tester boards. Plug a cable in, pick
the plugs, tap every LED that lights, and the page tells you whether that's the
best result the cable can show: **Keep** or **Toss**, with what's missing and
what it breaks.

**Live:** https://antjlopez.github.io/cable-keep-or-toss/

It supports two boards:

- **USB tester** (Treedix TRX5-0476): Type-A 3.0, Type-A 2.0 and Type-C on the
  left edge; Type-B 3.0, Type-C, Micro-B 3.0, Mini-B, Lightning and Micro-B on
  the others. Open with `#usb`.
- **HDMI tester** (Treedix TRX5-0748): Standard, Mini and Micro HDMI on both
  edges. Open with `#hdmi`.

Each board is drawn at its real proportions (measured from product photos),
showing only the ports and LEDs.

Everything lives in `index.html`: no build step and no dependencies beyond
Google Fonts, which are optional (it falls back to system fonts).

## How it decides

### USB

On this board every pin of the left-edge ports is tied to the battery, and every
pin of the far-end port drives its own LED, so a lit LED means that pin reaches
the other end of the cable. A cable gets **Keep** when every pin its two plugs
can carry lights, nothing else lights, and the shield is connected.

| Cable                    | Best possible                                                                     | LEDs |
| ------------------------ | --------------------------------------------------------------------------------- | ---- |
| USB-C ↔ USB-C            | All except one D+/D− pair: 4 GND, 4 VBUS, both CC, 8 SuperSpeed, both SBU, Shield | 23   |
| USB-A 3.x → USB-C        | 4 GND, 4 VBUS, one D+/D− pair, one TX and one RX pair, Shield                     | 15   |
| USB-A 2.0 → USB-C        | 4 GND, 4 VBUS, one D+/D− pair, Shield                                             | 11   |
| → Micro-B 3.0, USB-B 3.0 | GND, VBUS, D+, D−, 4 SuperSpeed, Shield (ID dark)                                 | 9    |
| → Micro-B, Mini-B, USB-B | GND, VBUS, D+, D−, Shield (ID dark)                                               | 5    |
| → Lightning              | GND, VBUS, D+, D−                                                                 | 4    |

Treedix doesn't document which LEDs the Micro-B 3.0 and USB-B 3.0 SuperSpeed
pins use; the page expects TX2±/RX1± and accepts any four lit SuperSpeed LEDs if
your board differs.

### HDMI

Every HDMI connector size carries the same 19 signals plus the shell, and the
board gives each one an LED, so the best result is always **all 20 LEDs**. A
dark LED is explained by what it breaks: video pairs, pair shields, DDC (EDID
and HDCP), +5V and hot plug, CEC, and pin 14 (RES), which ARC, eARC and HDMI
Ethernet need.

### Limits

LEDs only show continuity. They can't show wire gauge (USB charging speed),
swapped wires, or signal quality, and they can't tell HDMI versions apart. Chips
in Lightning plugs, e-marked USB-C cables and fiber HDMI cables can change the
pattern.

## Sources

- [Treedix USB test board manual](https://manuals.plus/treedix/usb-cable-data-line-test-board-manual)
- [Community LED guide for the USB board](https://github.com/patrickgardella/usbcabletester)
- [Treedix HDMI cable tester board](https://treedix.com/products/cable-tester-board-for-hdmi-cable-tester-checker-20-led-light-status-indictor-to-check-disorder-short-open-and-cross-connection-for-hdmi-a-type-hdmi-c-type-hdmi-d-type)
