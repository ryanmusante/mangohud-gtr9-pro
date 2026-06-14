# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.7.0-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

All-uppercase, readout-only MangoHud config for the Beelink GTR9 Pro (Radeon 8060S / Strix Halo) on CachyOS Wayland with RADV. A single horizontal top bar: FPS with frametime and 1% / 0.1% lows, a frametime graph, GPU and CPU load with clocks and temperatures, the unified `vram` / `ram` / `swap` pool, and render resolution. Display-only — nothing changes frame pacing or rendering.

Uppercase is done with a bundled font (`GTR9-Caps-Mono.ttf`, set via `font_file`): MangoHud has no uppercase option and renders `ms`, `MHz`, `GiB`, and labels like `Resolution` as fixed mixed-case strings, so the font remaps lowercase glyphs to uppercase. `legacy_layout=0` keeps the on-screen order identical to the config.

## Install

Needs MangoHud ≥ 0.8.3, kernel ≥ 6.14, RADV (Mesa 24+). Copy the config and the bundled font:

```fish
mkdir -p ~/.config/MangoHud
cp ./MangoHud.conf      ~/.config/MangoHud/
cp ./GTR9-Caps-Mono.ttf ~/.config/MangoHud/
```

`font_file` uses `~/`, which MangoHud expands; edit it if you install the font elsewhere.

## Usage

```
mangohud %command%
gamemoderun mangohud %command%
```

Toggle with `Shift_R + F12`. Move the bar with `position=top-center` or `top-right`.

## Example

```
118 FPS 8.5 MS  │  AVG 121 FPS  1% 84 FPS  0.1% 71 FPS  │  ▁▂▃▁▂▁  │  GPU 96%  74°C  2901 MHZ  │  CPU 38%  66°C  4102 MHZ  │  VRAM 6.5 GIB  70°C  2400 MHZ  │  RAM 13.2 GIB  0.0 GIB  │  RESOLUTION 3840X2160
```

![Example MangoHud bar](example.png)

Column spacing and the engine label before FPS are set by MangoHud at runtime; the casing and element order are fixed by the config.

## Metrics

| Directives | Shows |
|---|---|
| `fps` `frametime` `fps_metrics=avg,1,0.1` `frame_timing` | FPS and frametime, 1% / 0.1% lows, frametime graph |
| `gpu_stats` `gpu_temp` `gpu_core_clock` | GPU load, edge temperature, core clock |
| `cpu_stats` `cpu_temp` `cpu_mhz` | CPU load, package temperature, frequency |
| `vram` `gpu_mem_temp` `gpu_mem_clock` `ram` `swap` | unified pool: VRAM + memory temp/clock, RAM + swap |
| `resolution` | render resolution |

`gpu_mem_temp` and `gpu_mem_clock` render inside the VRAM group and `swap` as a bare value after RAM, since the GTR9 Pro shares one LPDDR5X pool between CPU and GPU; both memory readouts require `vram`.

## Files

```
mangohud-gtr9-pro/
├── CHANGELOG.md
├── FONT-LICENSE
├── GTR9-Caps-Mono.ttf
├── MangoHud.conf
├── README.md
└── example.png
```

## License

MIT, except the bundled `GTR9-Caps-Mono.ttf`, a DejaVu Sans Mono derivative licensed separately under the Bitstream Vera Fonts Copyright — see [FONT-LICENSE](FONT-LICENSE).
