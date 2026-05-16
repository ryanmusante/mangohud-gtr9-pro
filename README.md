# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.0.2-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![distro](https://img.shields.io/badge/distro-CachyOS-6a4c93.svg)](https://cachyos.org/)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

Horizontal MangoHud bar for the Beelink GTR9 Pro (Ryzen AI Max+ 395 / Radeon 8060S / Strix Halo / gfx1151) on CachyOS Wayland with RADV. Top-left anchor, FPS plus 1% / 0.1% lows, frametime + throttle graph, GPU edge + junction temp, RAPL package power, UMA-aware memory readout (`vram` + `ram` + `swap` + `proc_vram`).

## Table of contents

- [Compatibility](#compatibility)
- [Install](#install)
- [Example HUD output](#example-hud-output)
- [Verify](#verify)
- [Default keybinds](#default-keybinds)
- [Steam launch options](#steam-launch-options)
- [Strix Halo UMA caveat](#strix-halo-uma-caveat)
- [RAPL CPU power note](#rapl-cpu-power-note)
- [Known issues](#known-issues)
- [Troubleshooting](#troubleshooting)
- [References](#references)
- [Files](#files)
- [License](#license)

## Compatibility

| Component | Required |
|---|---|
| MangoHud | `≥ 0.8.3` (`ram_temp`, `cpu_custom_temp_sensor`, Panthor) |
| Kernel   | `≥ 6.14` (`≥ 6.18.4` recommended for ntsync / amdgpu fixes) |
| Vulkan   | RADV (Mesa 24.x+) |
| Display  | Wayland (XWayland and X11 also work) |
| Hardware | Strix Halo / Radeon 8060S; generic AMD APUs work |

## Install

```fish
mkdir -p ~/.config/MangoHud ~/mangologs
sed "s|/home/USERNAME/mangologs|$HOME/mangologs|" ./MangoHud.conf \
  > ~/.config/MangoHud/MangoHud.conf
chmod 0644 ~/.config/MangoHud/MangoHud.conf
```

> [!CAUTION]
> The `>` redirect overwrites any existing `~/.config/MangoHud/MangoHud.conf`.

## Example HUD output

Top-left, horizontal, semi-transparent. Schematic of the rendered bar in a 4K Vulkan game running at 144 fps capped, with GameMode and vkBasalt active:

```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│  144 fps   avg 142   1%▼ 138   0.1%▼ 128   #8521   ▁▂▂▁▂▃▂▁▂▂                       17:42   │
│  iGPU   78%   72°C   85°C   74°C   2750 MHz   2800 MHz   78 W   0.95 V                       │
│  CPU    42%   58°C   4850 MHz   35 W                                                          │
│  VRAM   4.2 GiB   RAM   18.4 GiB   SWAP   0.0 GiB   proc_vram   3.8 GiB                       │
│  Vulkan 1.3.290   RADV MESA 25.0.5   WINE 9.0   ntsync   64-bit   game.exe   FIFO            │
│  3840×2160   Wayland   GAMEMODE   VKBASALT   Limit: 144                                       │
└──────────────────────────────────────────────────────────────────────────────────────────────┘
```

Row-by-row, left to right:

| Row | Fields |
|---|---|
| 1 | FPS, AVG / 1% low / 0.1% low, frame count, frametime+throttle graph, wall clock |
| 2 | GPU: load, edge temp, junction temp, mem temp, core MHz, mem MHz, package W, voltage |
| 3 | CPU: load, package temp, top-core MHz, RAPL package W |
| 4 | VRAM (BIOS UMA), system RAM, swap, per-process VRAM |
| 5 | Vulkan API, RADV ICD, Wine/Proton, sync (ntsync/fsync/esync), arch, exec name, present mode |
| 6 | Render resolution, display server, GameMode badge, vkBasalt badge, active FPS limit |

FPS, GPU load, CPU load color-shift at 30/60 and 50/90 thresholds.

## Verify

```fish
mangohud --version
mangohud vkcube
vulkaninfo --summary | grep -E "driverName|deviceName"
```

## Default keybinds

| Key | Action |
|---|---|
| `Shift_R + F12` | Toggle HUD |
| `Shift_R + F11` | Cycle HUD position |
| `Shift_R + F10` | Cycle preset |
| `Shift_R + F9`  | Reset FPS metrics |
| `Shift_L + F1`  | Toggle FPS limit (`0 → 240 → 144 → 60 → 30`) |
| `Shift_L + F2`  | Toggle logging |
| `Shift_L + F3`  | Upload latest log |
| `Shift_L + F4`  | Reload config |

## Steam launch options

```
mangohud %command%
gamemoderun mangohud %command%
MANGOHUD_DLSYM=1 mangohud %command%
```

## Strix Halo UMA caveat

| Region | What MangoHud shows | Controlled by |
|---|---|---|
| BIOS UMA carveout | `vram` (`mem_info_vram_total`) | BIOS UMA Frame Buffer Size |
| GTT shared pool   | covered by `ram` + `proc_vram` | `ttm.pages_limit` (mainline amdgpu) / `amdttm.pages_limit` (DKMS) |

> [!IMPORTANT]
> To grow `vram`, change the BIOS UMA Frame Buffer Size. To grow GTT, set `ttm.pages_limit=<pages>` (or `amdttm.pages_limit=` on DKMS) on the kernel cmdline. Kernel ≥ 6.16.9 makes both unnecessary.

## RAPL CPU power note

`cpu_power` reads `/sys/class/powercap/intel-rapl:0/energy_uj`, root-only on Zen 5 by default (Platypus, CVE-2020-8694, CVE-2020-8695). Persistent fix:

```fish
echo 'f /sys/class/powercap/intel-rapl:0/energy_uj 0444 root root - -' \
  | sudo tee /etc/tmpfiles.d/99-rapl-readable.conf
sudo systemd-tmpfiles --create /etc/tmpfiles.d/99-rapl-readable.conf
```

Alternative: `zenpower3` or `zenergy` (no RAPL permission needed).

## Known issues

| Issue | Workaround |
|---|---|
| `gpu_junction_temp` reads 0 ([upstream #2001](https://github.com/flightlessmango/MangoHud/issues/2001)) | Upgrade kernel; fall back to `gpu_temp` (edge) |
| Native Wayland OpenGL games ([upstream #477](https://github.com/flightlessmango/MangoHud/issues/477)) | Use Vulkan/XWayland path, or `MANGOHUD_DLSYM=1` |
| `Shift_R+F10` overrides this config | Avoid the keybind or define `presets.conf` |

## Troubleshooting

| Symptom | Fix |
|---|---|
| HUD absent in Vulkan game | `vulkaninfo \| grep MANGOHUD`; reinstall AUR git build on layer mismatch |
| HUD absent in OpenGL game | `MANGOHUD_DLSYM=1`, or prepend `LD_PRELOAD=/usr/lib/mangohud/libMangoHud.so` |
| Keybinds inert | Press in game window; `ydotool` for synthetic input on Wayland |
| `0 W` cpu_power | See [RAPL CPU power note](#rapl-cpu-power-note) |
| Log files never appear | `ls -ld ~/mangologs`; re-run [Install](#install) |

## References

- [MangoHud](https://github.com/flightlessmango/MangoHud) — upstream
- [MangoHud master `data/MangoHud.conf`](https://raw.githubusercontent.com/flightlessmango/MangoHud/master/data/MangoHud.conf) — canonical option list (commented optional directives live here)
- [MangoHud 0.8.3 release](https://github.com/flightlessmango/MangoHud/releases/tag/v0.8.3)
- [MangoHud #1101](https://github.com/flightlessmango/MangoHud/issues/1101), [#1957](https://github.com/flightlessmango/MangoHud/issues/1957), [#2001](https://github.com/flightlessmango/MangoHud/issues/2001), [#477](https://github.com/flightlessmango/MangoHud/issues/477)
- [gamescope #1917](https://github.com/ValveSoftware/gamescope/issues/1917)
- [ROCm #5444](https://github.com/ROCm/ROCm/issues/5444), [#5562](https://github.com/ROCm/ROCm/issues/5562)
- [AMD ROCm — Strix Halo system optimization](https://rocm.docs.amd.com/en/latest/how-to/system-optimization/strixhalo.html)

## Files

```
mangohud-gtr9-pro-v1.0.2/
├── CHANGELOG.md
├── MangoHud.conf      # drop into ~/.config/MangoHud/
└── README.md
```

## License

MIT
