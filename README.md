# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.8.0-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Readout-only MangoHud config for the Beelink GTR9 Pro (Radeon 8060S / Strix Halo) on CachyOS Wayland with RADV. A single horizontal top bar: FPS with frametime and a frametime graph, GPU and CPU load with clocks and temperature, and the unified `vram` / `ram` / `swap` pool. Display-only — nothing changes frame pacing or rendering.

`legacy_layout=0` keeps the on-screen order identical to the config.

## Install

Needs MangoHud ≥ 0.8.3, kernel ≥ 6.14, RADV (Mesa 24+). Copy the config:

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

## Example

Illustrative — element order only, not exact runtime formatting:

```
FPS 8.5 ms  GPU 96% 74°C 2901 MHz  CPU 38% 4102 MHz  VRAM 6.5 GiB 2400 MHz  RAM 13.2 GiB 0.0 GiB
```

Column spacing, separators, and the engine label before FPS are set by MangoHud at runtime; only the element order is fixed by the config.

## Metrics

| Directives | Shows |
|---|---|
| `fps` `frametime` `frame_timing` | FPS, frametime, frametime graph |
| `gpu_stats` `gpu_temp` `gpu_core_clock` | GPU load, edge temperature, core clock |
| `cpu_stats` `cpu_mhz` | CPU load, frequency |
| `vram` `gpu_mem_clock` `ram` `swap` | unified pool: VRAM + memory clock, RAM + swap |

`gpu_mem_clock` renders inside the VRAM group and `swap` as a bare value after RAM, since the GTR9 Pro shares one LPDDR5X pool between CPU and GPU; the memory clock readout requires `vram`.

## Files

```
mangohud-gtr9-pro/
├── CHANGELOG.md
├── MangoHud.conf
└── README.md
```

## License

MIT.
