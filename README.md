# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.0.2-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Minimal MangoHud config for the Beelink GTR9 Pro (Ryzen AI Max+ 395 / Radeon 8060S / Strix Halo) on CachyOS Wayland with RADV. Top-left vertical overlay: FPS, frametime, GPU/CPU stats, UMA-aware memory (`vram` + `proc_vram` + `ram`).

## Table of contents

- [Compatibility](#compatibility)
- [Install](#install)
- [Usage](#usage)
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

## Files

```
mangohud-gtr9-pro-v1.0.2/
├── CHANGELOG.md
├── MangoHud.conf
└── README.md
```

## License

MIT
