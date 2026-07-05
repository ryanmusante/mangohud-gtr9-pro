# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.17.0-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.4-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Readout-only MangoHud config for the Beelink GTR9 Pro (Radeon 8060S / Strix Halo) on CachyOS Wayland with RADV. A single horizontal top bar, left to right: FPS with frametime and graph, GPU load with temperature, clock and power, CPU load with frequency and power, then the unified memory pool (`vram` + `ram`). Display-only — nothing changes frame pacing or rendering.

This is the layout deployed by [ry-install](https://github.com/ryanmusante/ry-install); the repo config and the installer's embedded config are kept in lockstep.

## Install

Needs MangoHud >= 0.8.4, kernel >= 6.14, RADV (Mesa 24+).

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
FPS 142 8.5 ms  GPU 96% 74°C 2901 MHz 102 W  CPU 38% 4102 MHz 42 W  VRAM 0.5 GiB  RAM 13.2 GiB
```

## Options

Every directive in the config, in file order:

| Directive | Effect |
|---|---|
| `horizontal` | single top-of-screen row |
| `legacy_layout=0` | render in config element order |
| `position=top-left` | bar location; `top-center` / `top-right` to move |
| `toggle_hud=Shift_R+F12` | explicit show/hide bind (MangoHud's default key) |
| `fps` | current frame rate |
| `frametime` | frametime in ms, inline with FPS |
| `frame_timing` | frametime line graph |
| `gpu_stats` | GPU load % |
| `gpu_temp` | GPU edge temperature |
| `gpu_core_clock` | GPU core clock |
| `gpu_power` | GPU package power draw (W) |
| `cpu_stats` | CPU load % |
| `cpu_mhz` | CPU frequency (highest active core) |
| `cpu_power` | CPU package power draw (W) |
| `vram` | GPU VRAM (BIOS UMA carveout only, not the full pool) |
| `ram` | CPU-side usage of the shared pool |
| `font_size=20` | HUD text size |
| `text_outline` | outline glyphs for legibility over bright frames |
| `background_alpha=0.4` | HUD backdrop opacity (0 transparent, 1 opaque) |

On this shared-memory APU `vram` shows only the small BIOS carveout, so `ram` is the figure to watch. `legacy_layout=0` keeps the on-screen order identical to the config. Only `gpu_temp` is shown: the GPU is the thermal-limited part on Strix Halo, and its edge reading is the temperature that matters. `cpu_temp` is left commented because MangoHud's CPU-temperature readout didn't report reliably on this unit (the `k10temp` Tctl sensor is present but MangoHud may not pick it up depending on kernel and hwmon layout); `cpu_power` covers the CPU side instead. Uncomment `cpu_temp` and check whether it renders on your kernel.

## License

MIT.
