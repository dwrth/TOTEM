<picture align="center">
  <source media="(prefers-color-scheme: dark)" srcset="/docs/images/TOTEM_logo_dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="/docs/images/TOTEM_logo_bright.svg">
  <img alt="TOTEM logo" src="/docs/images/TOTEM_logo_dark.svg">
</picture>

<h1 align="center">T O T E M <sub>(fork)</sub></h1>

Personal fork of [GEIGEIGEIST/TOTEM](https://github.com/GEIGEIGEIST/TOTEM) — a 38-key column-staggered choc split for the Seeed XIAO (RP2040 / nRF52840).

***

## Goal

Adapt the TOTEM so it fits *my* hands and workflow better:

1. **Add a top “num” row** — not primarily for digits, but for one-key shortcuts and quick binds (e.g. jumping Aerospace spaces with a single press, and similar muscle-memory actions).
2. **Ergonomic tweaks for personal fit** — starting with raising the outer pinky stretch keys, plus further layout/hand-fit adjustments as I try the mock and iterate.

Upstream TOTEM stays the reference. This repo is the working fork for that redesign (layout → PCB → case → firmware).

***

## Status

| Piece | State |
| --- | --- |
| Layout (Ergogen) | Stock reconstruction + num-row variant in [`ergogen/`](ergogen/) |
| Plan / how-to | [`docs/adding-number-row.md`](docs/adding-number-row.md) |
| PCB / case / firmware | Still upstream until the new layout is frozen |

- [`ergogen/totem.yaml`](ergogen/totem.yaml) — PCB-measured reconstruction of the stock 38-key layout  
- [`ergogen/totem-num.yaml`](ergogen/totem-num.yaml) — same, plus a `num` row on each finger column and stretch keys moved up 1u  

Import either file into the [Ergogen](https://docs.ergogen.xyz/) web UI to preview.

***

## Upstream TOTEM

Original board by Geist — [Hackster write-up](https://www.hackster.io/geist/totem-a-tiny-splitkeyboard-with-splay-cb2e43).

![TOTEM layout](/docs/images/TOTEM_layout.svg)

- [PCB](/PCB/) · [Case](/case/) · [Build guide](/docs/buildguide.md)
- Firmware: [QMK/Vial](/firmware/QMK/) (wired) · [ZMK](/firmware/ZMK/) (wireless)
- Kits: [keeb.supply](https://keeb.supply/products/geist-totem)

***

## Credits

Upstream design, PCB, case, and firmware: [GEIGEIGEIST/TOTEM](https://github.com/GEIGEIGEIST/TOTEM) and everyone listed there.
