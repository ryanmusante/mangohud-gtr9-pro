# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.20.0-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.4-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Readout-only MangoHud config for the Beelink GTR9 Pro (Radeon 8060S / Strix Halo) on CachyOS Wayland with RADV. A single horizontal top bar, left to right: FPS with frametime and graph, GPU load with temperature, clock and power, CPU load with temperature, frequency and power, then the unified memory pool (`vram` + `ram`). Display-only — nothing changes frame pacing or rendering.

This is the layout deployed by [ry-install](https://github.com/ryanmusante/ry-install); the repo config and the installer's embedded config are kept in lockstep.

## Install

Needs MangoHud >= 0.8.4, kernel >= 6.14 (6.18.4+ preferred for gfx1151 stability), RADV (Mesa >= 24.1, where gfx1151 landed).

```fish
mkdir -p ~/.config/MangoHud
cp ./MangoHud.conf ~/.config/MangoHud/
```

## Usage

```
mangohud %command%
gamemoderun mangohud %command%
```

Toggle with `Shift_R + F12` (set explicitly via `toggle_hud`). Move the bar with `position=top-center` or `top-right`.

Under ry-install the config is deployed automatically and the overlay is enabled for all Vulkan apps via `MANGOHUD=1`, so the `mangohud %command%` prefix is optional.

## Example

Illustrative — element order only, not exact runtime formatting:

```
FPS 142 8.5 ms  GPU 96% 74°C 2901 MHz 102 W  CPU 38% 61°C 4102 MHz 42 W  VRAM 6.8 GiB  RAM 13.2 GiB
```

## Options

The config is grouped into labelled sections — Layout, Control, FPS, GPU, CPU, Memory, Style — and renders in that order. Every directive, in file order:

| Section | Directive | Effect |
|---|---|---|
| Layout | `horizontal` | single top-of-screen row |
| Layout | `legacy_layout=0` | render in config element order |
| Layout | `position=top-left` | bar location; `top-center` / `top-right` to move |
| Control | `toggle_hud=Shift_R+F12` | explicit show/hide bind (MangoHud's default key) |
| FPS | `fps` | current frame rate |
| FPS | `frametime` | frametime in ms, inline with FPS |
| FPS | `frame_timing` | frametime line graph |
| GPU | `gpu_stats` | GPU load % |
| GPU | `gpu_temp` | GPU edge temperature |
| GPU | `gpu_core_clock` | GPU core clock |
| GPU | `gpu_power` | GPU package power draw (W) |
| CPU | `cpu_stats` | CPU load % |
| CPU | `cpu_temp` | CPU package temperature (Tctl) |
| CPU | `cpu_custom_temp_sensor=k10temp,temp1_input` | pin CPU temp to the k10temp Tctl input |
| CPU | `cpu_mhz` | CPU frequency (highest active core) |
| CPU | `cpu_power` | CPU package power draw (W) |
| Memory | `vram` | GPU memory in use; on this APU, BIOS carveout plus GTT combined (true unified GPU usage) |
| Memory | `ram` | CPU-side usage of the shared pool |
| Style | `font_size=20` | HUD text size |
| Style | `text_outline` | outline glyphs for legibility over bright frames |
| Style | `background_alpha=0.4` | HUD backdrop opacity (0 transparent, 1 opaque) |

On this shared-memory APU `vram` reports true unified GPU memory usage: MangoHud detects the APU and adds the GTT pool to the BIOS carveout, so the figure tracks real GPU allocation rather than just the small fixed carveout. `ram` shows system memory separately. `legacy_layout=0` keeps the on-screen order identical to the config.

`cpu_temp` shows the CPU package temperature (the `k10temp` Tctl channel). On CachyOS, MangoHud can otherwise latch onto a motherboard/EC sensor and report the wrong value, so `cpu_custom_temp_sensor=k10temp,temp1_input` pins it to the correct hwmon input; adjust the `hwmon_name,input` pair if `sensors` shows a different layout on your unit. `gpu_temp` is the GPU edge reading — the thermal-limited part on Strix Halo — and `cpu_power` / `gpu_power` cover package draw on each side.

## License

MIT.
