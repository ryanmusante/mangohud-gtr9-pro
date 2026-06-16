# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.11.1-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.4-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Readout-only MangoHud config for the Beelink GTR9 Pro (Radeon 8060S / Strix Halo) on CachyOS Wayland with RADV. A single horizontal top bar, left to right: GPU load with clock, edge temperature and power; CPU load with frequency and temperature; the unified memory pool (`vram` + `ram`); then FPS with frametime, a frametime graph and 1%/0.1% lows. Display-only — nothing changes frame pacing or rendering.

`legacy_layout=0` keeps the on-screen order identical to the config.

## Install

Needs MangoHud ≥ 0.8.4, kernel ≥ 6.14, RADV (Mesa 24+). Copy the config:

```fish
mkdir -p ~/.config/MangoHud
cp ./MangoHud.conf ~/.config/MangoHud/
```

## Usage

```
mangohud %command%
gamemoderun mangohud %command%
```

Toggle with `Shift_R + F12`. Move the bar with `position=top-center` or `top-right`.

Under [ry-install](https://github.com/ryanmusante/ry-install) this config is deployed automatically and the overlay is enabled for all Vulkan apps via `MANGOHUD=1`, so the `mangohud %command%` prefix is optional.

## Example

Illustrative — element order only, not exact runtime formatting:

```
GPU 96% 74°C 2901 MHz 119 W  CPU 38% 4102 MHz 61°C  VRAM 0.5 GiB  RAM 13.2 GiB  FPS 142 8.5 ms (1% 98 · 0.1% 71)
```

Column spacing, separators, and the engine label before FPS are set by MangoHud at runtime; only the element order is fixed by the config.

## Metrics

| Directives | Shows |
|---|---|
| `gpu_stats` `gpu_temp` `gpu_core_clock` `gpu_power` | GPU load, edge temperature, core clock, package power |
| `cpu_stats` `cpu_mhz` `cpu_temp` | CPU load, frequency, temperature |
| `vram` `ram` | unified LPDDR5X pool: VRAM carveout + RAM |
| `fps` `frametime` `frame_timing` `fps_metrics` | FPS, frametime, frametime graph, 1%/0.1% lows |

On this shared-memory APU `vram` shows only the small BIOS carveout, so `ram` is the memory figure to watch. `gpu_mem_clock` and `swap` are left out as they aren't useful here. `throttling_status` and `core_load` are included but commented out — uncomment to enable.

## Files

```
mangohud-gtr9-pro/
├── CHANGELOG.md
├── MangoHud.conf
└── README.md
```

## License

MIT.
