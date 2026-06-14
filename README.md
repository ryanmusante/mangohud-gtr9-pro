# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.1.0-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Minimal MangoHud config for the Beelink GTR9 Pro (Ryzen AI Max+ 395 / Radeon 8060S, RDNA 3.5 / Strix Halo) on CachyOS Wayland with RADV. A horizontal top-of-screen bar in the style of MangoHud's built-in Horizontal preset (`preset=2`), trimmed to a popular metric set with sensor detail: FPS, frametime (+ graph), GPU/CPU load with clocks and temperature, UMA-aware memory (`vram` + `ram`), and an active-throttling indicator (+ graph).

## Table of contents

- [Compatibility](#compatibility)
- [Install](#install)
- [Usage](#usage)
- [Example output](#example-output)
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

## Example output

A single horizontal row across the top of the screen (layout and values illustrative; clocks in MHz, temperatures in Celsius). The throttling indicator only appears while throttling is actually happening, so it is usually absent:

```
GPU  96%  2.9 GHz  74°C     CPU  38%  4.1 GHz  66°C     VRAM  6.5 GiB     RAM  13.2 GiB     118 FPS     8.5 ms    ▁▂▁▁▃▂▁▁
```

| Column | Directive |
|---|---|
| `GPU 96%` / `74°C` | `gpu_stats` load + `gpu_temp` |
| `2.9 GHz` (GPU) | `gpu_core_clock` (+ `gpu_mem_clock`) |
| `CPU 38%` / `66°C` | `cpu_stats` load + `cpu_temp` |
| `4.1 GHz` (CPU) | `cpu_mhz` |
| `VRAM 6.5 GiB` / `RAM 13.2 GiB` | `vram` + `ram` (shared from the APU's unified memory pool) |
| `118 FPS` / `8.5 ms` | `fps` + `frametime` |
| `▁▂▁▁▃▂▁▁` | `frame_timing` inline frametime graph |
| (only when throttling) | `throttling_status` + `throttling_status_graph` |

## Files

```
mangohud-gtr9-pro/
├── CHANGELOG.md
├── MangoHud.conf
└── README.md
```

## License

MIT
