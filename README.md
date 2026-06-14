# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.5.0-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Expanded MangoHud config for the Beelink GTR9 Pro (Ryzen AI Max+ 395 / Radeon 8060S, RDNA 3.5 / Strix Halo) on CachyOS Wayland with RADV. A horizontal top-of-screen bar built on MangoHud's Horizontal preset (`preset=2`) with a diagnostics-oriented metric set: FPS with 1% / 0.1% lows, frametime (+ graph), GPU load with clocks and edge/memory temperatures, render resolution, CPU load with frequency and temperature, and the full UMA memory triad (`vram` + `ram` + `swap`). Every directive is readout-only — nothing here changes frame pacing, vsync, or rendering. The metric labels render uppercase; only MangoHud's fixed unit suffixes (`ms`, `MHz`, `GiB`) carry mixed case, which no config option can change.

## Table of contents

- [Compatibility](#compatibility)
- [Install](#install)
- [Usage](#usage)
- [Metrics](#metrics)
- [Files](#files)
- [License](#license)

## Compatibility

| Component | Required |
|---|---|
| MangoHud | `≥ 0.8.3` |
| Kernel   | `≥ 6.14` |
| Vulkan   | RADV (Mesa 24.x+) |

## Install

```fish
mkdir -p ~/.config/MangoHud
cp ./MangoHud.conf ~/.config/MangoHud/MangoHud.conf
```

## Usage

Steam launch options:

```
mangohud %command%
gamemoderun mangohud %command%
```

Toggle in-game: `Shift_R + F12`.

The bar is anchored top-left by default; set `position=top-center` (or `top-right`) in `MangoHud.conf` to move it along the top edge.

## Metrics

| Directive | Shows |
|---|---|
| `fps` + `fps_metrics=avg,1,0.1` | current FPS plus average, 1% low, and 0.1% low |
| `frametime` | frametime in milliseconds |
| `frame_timing` | inline frametime line graph (stutter spikes) |
| `gpu_stats` + `gpu_core_clock` + `gpu_mem_clock` | GPU load and core / memory clocks |
| `gpu_temp` + `gpu_mem_temp` | GPU edge and memory temperatures |
| `resolution` | current render resolution |
| `cpu_stats` + `cpu_mhz` + `cpu_temp` | CPU load, frequency, and temperature |
| `vram` + `ram` + `swap` | unified memory pool usage and swap |

Notes:
- `gpu_junction_temp` is omitted: on this APU (gfx1151) the junction/hotspot sensor mirrors the edge value that `gpu_temp` already reports, so it would only duplicate the GPU temperature.
- `gpu_mem_clock` and `gpu_mem_temp` require `vram` (enabled).
- The GTR9 Pro shares a single LPDDR5X pool between CPU and GPU, so `vram` and `ram` draw from the same unified memory.
- Driver, GPU-name, and engine readouts (`vulkan_driver`, `gpu_name`, `engine_version`) are intentionally omitted: they are the only free-form strings MangoHud renders in lowercase, and there is no option to force them uppercase.

## Files

```
mangohud-gtr9-pro/
├── CHANGELOG.md
├── MangoHud.conf
└── README.md
```

## License

MIT
