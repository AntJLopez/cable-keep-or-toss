# USB Cable Keep-or-Toss

A single-page tool for the Treedix USB cable tester board (the LED board with
Type-A 3.0, Type-A 2.0 and Type-C ports on one edge, and Type-B 3.0, Type-C,
Micro-B 3.0, Mini-B, Lightning and Micro-B ports on the others).

Plug a cable into the board, pick the two plug types, tap every LED that lights,
and the page tells you whether that's the best result the cable type can show:
**Keep** or **Toss**, with what's missing or shouldn't be lit.

Everything lives in `index.html`: no build step, no dependencies beyond Google
Fonts (optional; it falls back to system fonts).

## How it decides

On this board every pin of the left-edge ports is tied to the battery, and every
pin of the far-end port drives its own LED, so a lit LED means that pin reaches
the other end of the cable. A cable gets **Keep** when every pin its two plugs
can carry lights, nothing else lights, and the shield is connected.

| Cable                     | Best possible                                                             | LEDs |
| ------------------------- | ------------------------------------------------------------------------- | ---- |
| USB-C ↔ USB-C             | All except one D+/D− pair: 4 GND, 4 VBUS, both CC, 8 SuperSpeed, both SBU, Shield | 23   |
| USB-A 3.x → USB-C         | 4 GND, 4 VBUS, one D+/D− pair, one TX and one RX pair, Shield              | 15   |
| USB-A 2.0 → USB-C         | 4 GND, 4 VBUS, one D+/D− pair, Shield                                      | 11   |
| → Micro-B 3.0, USB-B 3.0  | GND, VBUS, D+, D−, 4 SuperSpeed, Shield (ID dark)                          | 9    |
| → Micro-B, Mini-B, USB-B  | GND, VBUS, D+, D−, Shield (ID dark)                                        | 5    |
| → Lightning               | GND, VBUS, D+, D−                                                          | 4    |

Limits: LEDs can't show wire gauge (charging speed), swapped wires, or signal
quality. Treedix doesn't document which LEDs the Micro-B 3.0 and USB-B 3.0
SuperSpeed pins use; the page expects TX2±/RX1± and accepts any four lit
SuperSpeed LEDs if your board differs.

## Hosting

It's a static file, so any static host works.

- **GitHub Pages**: Settings → Pages → Deploy from a branch → `main` / root.
  Free for public repositories.
- **Cloudflare**: create a Worker with static assets (or a Pages project)
  pointing at this repository, with no build command and `/` as the output
  directory.

## Sources

- [Treedix test board manual](https://manuals.plus/treedix/usb-cable-data-line-test-board-manual)
- [Community LED guide for this board](https://github.com/patrickgardella/usbcabletester)
