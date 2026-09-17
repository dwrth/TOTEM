# Adding a Number Row to TOTEM

A practical plan for forking TOTEM: add a full top row on both halves for numbers/shortcuts, and move the outer pinky (stretch) keys upward.

This is **not** a firmware-only change. New switch positions mean a new PCB outline, new routing, a new case, and then firmware. Treat it as a small custom-keyboard project that starts from this repo.

---

## What you are changing

| Today (stock TOTEM) | Your fork |
| --- | --- |
| 38 keys, 3 alpha rows + thumbs + outer pinkies | Same, plus a 4th finger row above the alphas |
| Outer pinkies sit low/outward (Balbuzard/Osprette style) | Those pinkies move **up** (same column family, higher reach) |
| Matrix: 4 rows × 5 cols per half | Need at least **one more matrix line** (usually a 5th row) |
| Existing case and plate cutouts | Must be redesigned to match the new outline |

Rough key count if you add 5 keys per half: **~48 switches** (and diodes/sockets to match).

---

## Tools / software you need

### Layout (start here)

| Tool | Role |
| --- | --- |
| [Ergogen](https://github.com/ergogen/ergogen) | Parametric split layout: columns, stagger, splay, pinky offset. This is how TOTEM’s layout was born. |
| [Keyboard Layout Editor](http://www.keyboard-layout-editor.com/) or [keymap-editor](https://nickcoutsos.github.io/keymap-editor/) | Sketch labels and layers before you commit to positions. |
| Paper + cardboard | Print 1:1 outlines, poke holes, test reach before PCB money. |

### PCB

| Tool | Role |
| --- | --- |
| [KiCad](https://www.kicad.org/) (7+) | Open `PCB/totem_0-3/`, edit schematic + board. Required. |
| [kleeb](https://github.com/crides/kleeb) footprints | XIAO footprints (including bottom pads) — recommended in the original design notes. |
| Existing project assets | Choc footprints and 3D models already live under `PCB/totem_0-3/TOTEMlib.pretty/` and `TOTEM3D/`. |

### Case

| Tool | Role |
| --- | --- |
| Fusion 360, FreeCAD, or Onshape | Rebuild top/bottom from a KiCad STEP export. Stock case files will not fit. |
| 3D print service | JLCPCB / PCBWay resin (same path as stock TOTEM), or local FDM for cheap fit checks. |

### Firmware

| Stack | When |
| --- | --- |
| [QMK](https://docs.qmk.fm/) + [Vial](https://get.vial.rocks/) | Wired XIAO RP2040 — start from `firmware/QMK/VIAL_source/`. |
| [ZMK](https://zmk.dev/) | Wireless XIAO BLE — start from [zmk-config-totem](https://github.com/GEIGEIGEIST/zmk-config-totem) (note: marked outdated; expect updates). |

### Fabrication

- PCB: JLCPCB, PCBWay, or Seeed Fusion (see `PCB/README.MD`)
- Extra parts: Choc switches, SOD-123 diodes, optional hotswap sockets — same BOM as the [build guide](buildguide.md), just more of each

---

## Hard constraint: MCU pins

Each half uses a Seeed XIAO. Stock QMK wiring is **4 row pins + 5 column pins** (`keyboard.json`). That matrix is already tight for 19 keys/half.

Adding a full top row almost always means **one more GPIO for a new matrix row** (or a column, then re-map). Before you place switches in KiCad:

1. List free XIAO pins after rows, cols, serial/TRRS, and anything else you keep.
2. Confirm the same pin plan works on **both** RP2040 (QMK) and nRF52840 (ZMK) if you want both variants.
3. Only then freeze “+5 keys per half.”

If you run out of pins, options shrink fast: drop a key, drop a feature, or leave the XIAO platform.

---

## Recommended workflow

### 1. Decide the layout on paper first

- How many new keys per half? (Often 5: numbers/symbols aligned to the five finger columns.)
- Move pinky stretch keys **up** relative to the current outer pinkies — keep or rethink splay.
- Keep thumb cluster and column stagger unless you have a reason to change them.
- Print a 1:1 mock and type on it for a few days.

**Guidance:** Ergogen docs + the original process write-up: [TOTEM on Hackster](https://www.hackster.io/geist/totem-a-tiny-splitkeyboard-with-splay-cb2e43) (layout → PCB → case → firmware).

### 2. Rebuild the outline in Ergogen (or carefully in KiCad)

Do not stretch the stock board “by eye” only. Regenerate positions so column stagger, splay, and the raised pinkies stay consistent. Export outlines / switch points you can import or redraw in KiCad.

### 3. Fork the PCB in KiCad

Open `PCB/totem_0-3/totem_0_3.kicad_pro`.

Typical edit order:

1. **Schematic** — add switches + diodes; extend the matrix (new `ROWn`); assign the free MCU pin.
2. **Board** — grow the outline upward; place new Choc footprints; move pinky footprints up; re-route.
3. **Ground pour / zones** — re-check after outline changes (the original design already hit a ground-fill gotcha once).
4. **Export Gerbers** — same manufacturing notes as `PCB/README.MD`.
5. **Export STEP** — for the case.

Useful references in-repo: pinout and traces under `docs/images/`, PDFs linked from `PCB/README.MD`.

### 4. Redesign the case

Stock `case/` parts assume the old outline and pinky recess. After the PCB STEP is solid:

- Rebuild top (integrated plate) and bottom.
- Recut TRRS / reset / power / USB openings.
- Reprint a cheap draft before resin.

### 5. Update firmware last

Only after the matrix is final:

- Change matrix size and pin list.
- Update `LAYOUT` / keymap so the new top row and raised pinkies exist.
- Flash and verify every switch in a matrix tester (Vial’s matrix tester is enough for wired).

QMK compile entry point for the stock board: see `firmware/QMK/VIAL_source/totem/readme.md`.

### 6. Build one prototype

Follow [buildguide.md](buildguide.md) habits (diode orientation, break-apart halves, etc.), then revise stagger/pinky height if the mock lied.

---

## Where to find guidance

| Topic | Source |
| --- | --- |
| How TOTEM itself was designed | [Hackster build story](https://www.hackster.io/geist/totem-a-tiny-splitkeyboard-with-splay-cb2e43) |
| Stock files to fork | This repo: `PCB/`, `case/`, `firmware/` |
| Pinky-outward idea | README credits: [Balbuzard](https://github.com/brow/balbuzard), [Osprette](https://github.com/smores56/osprette) — useful for pinky placement, not for a number row |
| Split PCB + KiCad workflow | [KiCad docs](https://docs.kicad.org/); community boards in [r/olkb](https://www.reddit.com/r/olkb/), Discord “Absolem Club” / split-keeb channels |
| Ergogen | [Ergogen repo](https://github.com/ergogen/ergogen) and example configs from other choc splits |
| QMK matrix / split | [QMK split keyboard docs](https://docs.qmk.fm/#/feature_split_keyboard) |
| ZMK matrix / shields | [ZMK new shield guide](https://zmk.dev/docs/development/new-shield) |
| Ordering PCBs / cases | `PCB/README.MD`, `case/README.MD` |

---

## Sensible order (short)

1. Paper mock with raised pinkies + top row  
2. Confirm free MCU pin for the new matrix line  
3. Ergogen layout — start from the reconstructed config in [`ergogen/totem.yaml`](../ergogen/totem.yaml)  
4. KiCad schematic → PCB → Gerbers + STEP  
5. Case from STEP  
6. QMK/ZMK matrix + keymap  
7. Prototype → tweak → final fab  

---

## What not to do

- Do not only edit the keymap and expect new physical keys.
- Do not move pinkies on the PCB without updating the case and plate.
- Do not order a production panel before a pin audit and a paper (or acrylic) mock.
- Do not assume ZMK config can be copied unchanged from the outdated totem config — re-check pins against your schematic.

---

## Success looks like

You can rest both hands on a 1:1 mock and reach the new top row and raised pinkies without strain; the schematic has a documented extra matrix line on a free XIAO pin; one prototype half scans every new switch in firmware.
