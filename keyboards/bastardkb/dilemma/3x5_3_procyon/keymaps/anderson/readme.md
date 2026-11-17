# Dilemma 3x5+3 Procyon `anderson` keymap

QWERTY with [home row mods](https://precondition.github.io/home-row-mods) and
[Miryoku-inspired layers](https://github.com/manna-harbour/miryoku), plus a
128x128 SH1107 OLED that shows a guide for the active layer.

Builds against [`bastardkb/bastardkb-qmk`](https://github.com/bastardkb/bastardkb-qmk)
`main` with the `bastardkb/bk_pointing_device` and `bastardkb/argos` modules
(see `keymap.json`). VIA is enabled.

## Layers

| Layer      | Reached by                                      | Contents                                     |
| ---------- | ----------------------------------------------- | -------------------------------------------- |
| Base       | —                                               | QWERTY, GUI/Alt/Ctrl/Shift on the home row   |
| Function   | hold `Tab`                                      | F1–F12, Print Screen, Scroll Lock, Pause     |
| Navigation | hold `Space`                                    | Arrows on HJKL, Home/PgDn/PgUp/End, Insert, Caps Lock, Print Screen |
| Media      | hold `Esc`                                      | Volume, track, play/stop, RGB effect/toggle  |
| Pointer    | hold `Z` or `/`, or move the trackpad           | Mouse buttons, scroll, pointing modes, DPI   |
| Numeral    | hold `Backspace`                                | Numpad on the left hand                      |
| Symbols    | hold `Enter`                                    | Shifted symbols on the left hand             |
| Macros     | hold `Delete`                                   | Reserved for user-defined macros             |

The pointer layer is also the auto mouse layer (`AUTO_MOUSE_DEFAULT_LAYER 4`):
touching the trackpad activates it.

## Pointer layer

```
 BOOT  CLR   TABS  DPI   SDPI    SDPI  WHL↓  WHL↑  CLR   BOOT
 GUI   ALT   CTL   SFT   HIST    ←     ↓     ↑     →     ZOOM⇄
 ___   DRAG  SNIP  BTN3  BRIT    CUR⇄  BTN3  SNIP  DRAG  ___
             BTN3  BTN2  BTN1    BTN1  BTN2  BTN3
```

- `MS_WHLD` / `MS_WHLU` on U / I scroll down / up.
- `DPI_MOD` cycles the DPI of the active mode, `S_D_MOD` the sniping DPI. Hold
  Shift to step backward.
- Hold-to-activate modes, active while held: `SNIPING` (lower DPI),
  `DRGSCRL` (trackpad scrolls), `PTABS` (Ctrl+Tab / Ctrl+Shift+Tab),
  `PHIST` (undo / redo: Ctrl+Z / Ctrl+Shift+Z), `PBRIGHT` (keyboard RGB
  brightness).
- Toggle modes, tap to turn on and tap again to turn off: `CUR_TOG`
  (trackpad sends arrow keys), `PZOOM_TOG` (vertical swipes send Ctrl+`+` /
  Ctrl+`-`).
- `EE_CLR` resets the persisted keymap and pointing settings; `QK_BOOT`
  enters the bootloader.

Sniping can be enabled automatically whenever the pointer layer is active.
This is a runtime setting stored in EEPROM (`auto_precision_on_mouse_layer_enabled`),
changed through the Argos protocol, not a compile-time define.

Other module keycodes not mapped here: `DPI_RMOD`, `S_D_RMOD`, `SNP_TOG`,
`DRG_TOG`, `CURSOR`, `PZOOM`, `PVOLUME`/`PVOLUME_TOG`, `PTABS_TOG`,
`PHIST_TOG`, `PBRIGHT_TOG`, `INVX`, `INVY`, `PCUSTOM1`–`PCUSTOM5` (and their
`_TOG` variants). See `modules/bastardkb/bk_pointing_device/introspection.h`.

## OLED

- Rows 0–7: layer artwork.
- Rows 7–10: key guide for the active layer.
- Row 13: current DPI and active pointing mode (`SNIP`, `DRAG`, `CURS`,
  `ZOOM`, `TABS`, `HIST`, `BRIT`, …; `+SNIP` when sniping stacks on another
  mode). Toggled modes stay shown until turned off.
- Row 14: active modifiers.
- Row 15: Num/Caps/Scroll Lock and WPM.

## Building

```sh
qmk compile -kb bastardkb/dilemma/3x5_3_procyon -km anderson
```

with `QMK_HOME` pointing at a `bastardkb-qmk` checkout and this repository as
the userspace (`git submodule update --init` for `modules/bastardkb`).
