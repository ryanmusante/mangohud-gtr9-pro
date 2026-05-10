# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.0.1-blue.svg)](CHANGELOG.md)
[![mangohud](https://img.shields.io/badge/mangohud-%E2%89%A5%200.8.3-f5af19.svg)](https://github.com/flightlessmango/MangoHud)
[![distro](https://img.shields.io/badge/distro-CachyOS-6a4c93.svg)](https://cachyos.org/)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](#license)

> Production-grade MangoHud configuration for the **Beelink GTR9 Pro** — Ryzen AI Max+ 395 (Strix Halo, gfx1151) / Radeon 8060S — running CachyOS Wayland with the RADV Vulkan stack.

Horizontal status bar, top-left anchored, tuned with the metrics advanced Linux benchmarkers actually use: FPS plus 1% / 0.1% lows, frametime graph with throttle overlay, GPU edge **and** junction temp, RAPL package power, RADV / Wine / ntsync diagnostics. UMA-aware memory readout (`vram` + `ram` + `swap`) for the Strix Halo carved-VRAM caveat.

## Table of contents

- [Compatibility](#compatibility)
- [Install](#install)
- [Verify](#verify)
- [Default keybinds](#default-keybinds)
- [Steam launch options](#steam-launch-options)
- [Strix Halo UMA caveat](#strix-halo-uma-caveat)
- [RAPL CPU power note](#rapl-cpu-power-note)
- [Customization](#customization)
- [Files](#files)
- [License](#license)

## Compatibility

| Component       | Required                                                                   |
|-----------------|----------------------------------------------------------------------------|
| MangoHud        | `≥ 0.8.3` — required for `ram_temp`, `cpu_custom_temp_sensor`, Panthor support |
| Mesa / RADV     | Modern (24.x+); junction temp wants 25.x for reliable readout              |
| Kernel          | `≥ 6.14` (`6.18.4+` recommended for ntsync / amdgpu fixes)                 |
| Distro          | CachyOS — any Arch derivative works; non-Arch needs MangoHud 0.8.3 backport |
| Display server  | Wayland (XWayland and X11 also work)                                       |
| Hardware        | Beelink GTR9 Pro / Strix Halo — generic AMD APUs work; VRAM nuance is Strix-specific |

## Install

One-shot, fish shell:

```fish
mkdir -p ~/.config/MangoHud
install -m 0644 ./MangoHud.conf ~/.config/MangoHud/MangoHud.conf
sed -i "s|/home/USERNAME/mangologs|$HOME/mangologs|" ~/.config/MangoHud/MangoHud.conf
mkdir -p ~/mangologs
```

If you already have a `MangoHud.conf`, the `install -m 0644` line will overwrite it; back it up first if you care.

## Verify

```fish
# 1) Confirm MangoHud version (should be 0.8.3 or newer on CachyOS as of 2026-05).
mangohud --version

# 2) Vulkan smoke test — full HUD via the RADV path.
mangohud vkcube

# 3) OpenGL smoke test (uses dlsym hook, on by default in 0.8.x).
mangohud glxgears

# 4) Confirm RADV is the loaded ICD (the HUD's vulkan_driver line should also show this).
vulkaninfo --summary | grep -E "driverName|deviceName"

# 5) Confirm RAPL CPU power is readable (else cpu_power shows 0 W).
test -r /sys/class/powercap/intel-rapl:0/energy_uj
  and echo "RAPL readable"
  or echo "RAPL needs chmod o+r — see RAPL note below"

# 6) Inspect the Strix Halo VRAM window the HUD will display.
cat /sys/class/drm/card*/device/mem_info_vram_total \
  | awk '{printf "%.2f GiB\n", $1/1024/1024/1024}'

# 7) 60-second benchmark + log inspection.
mangohud vkcube &
set vkpid $last_pid
sleep 1; xdotool key shift+F2     # or press Shift_L+F2 manually
sleep 60
xdotool key shift+F2
kill $vkpid
ls -lh ~/mangologs/ | tail -n 5
head -n 30 (ls -t ~/mangologs/*.csv | head -n 1)

# 8) Plot the result locally (mangoplot ships with mangohud >= 0.7.1).
mangoplot ~/mangologs/

# 9) Reload config in-game without restarting: Shift_L+F4.
```

## Default keybinds

| Key             | Action                                                |
|-----------------|-------------------------------------------------------|
| `Shift_R + F12` | Toggle HUD visibility                                 |
| `Shift_R + F11` | Cycle HUD position (top-L / top-R / bot-L / bot-R)    |
| `Shift_R + F10` | Cycle preset                                          |
| `Shift_R + F9`  | Reset FPS metrics (1% / 0.1% lows)                    |
| `Shift_L + F1`  | Toggle FPS limit (`0 → 240 → 144 → 60 → 30`)          |
| `Shift_L + F2`  | Toggle logging                                        |
| `Shift_L + F3`  | Upload latest log to flightlessmango.com              |
| `Shift_L + F4`  | Reload config                                         |

These do not collide with Steam (`Shift+Tab`), Discord (`Ctrl+Shift+*`), or common in-game bindings.

## Steam launch options

Per-game enablement (when not using `cachyos-gaming-meta`'s global hook):

```
mangohud %command%
```

Benchmarking with the OpenGL hook explicitly:

```
MANGOHUD_DLSYM=1 mangohud %command%
```

Combined with GameMode:

```
gamemoderun mangohud %command%
```

## Strix Halo UMA caveat

The `vram` line shows only the BIOS-carved VRAM window (typically `0.5–96 GB` depending on `amdttm.pages_limit` / BIOS UMA setting). It does **not** reflect the GTT-backed shared pool that Strix Halo's iGPU also draws from. This config compensates by also showing `ram` + `swap` + `proc_vram` so total memory pressure is always visible.

References:

- [ROCm issue #5444](https://github.com/ROCm/ROCm/issues/5444) — kernel `mem_info_vram_total` reporting on `gfx1151`
- [ollama issue #12062](https://github.com/ollama/ollama/issues/12062) — reproducer of the same artifact
- [AMD ROCm — Strix Halo system optimization](https://rocm.docs.amd.com/en/latest/how-to/system-optimization/strixhalo.html)

If you want to grow the VRAM window, set `amdttm.pages_limit` on the kernel command line. (If you use `ry-install`, it already manages `/etc/kernel/cmdline`.)

## RAPL CPU power note

`cpu_power` reads `/sys/class/powercap/intel-rapl:0/energy_uj`, which on Zen 5 is restricted to `root` by default (Platypus side-channel mitigation). If the HUD shows `0 W`, either:

```fish
sudo chmod o+r /sys/class/powercap/intel-rapl:0/energy_uj
```

or install the `zenpower3` / `zenergy` kernel module for an unprivileged Zen-native reading.

## Customization

| Tweak | Action |
|---|---|
| Per-core load on 32 threads | Uncomment `core_load`, `core_load_change`, `core_bars`, `core_type`. Expect a much wider bar. |
| Frametime histogram | Add `histogram=1`. Adds an ImGui draw cost. |
| Closer viewing distance | Bump `font_size=22` to `26` or `28`. |
| Fahrenheit | Add `temp_fahrenheit=1`. |
| Joules-per-frame metrics | Uncomment `gpu_efficiency`, `cpu_efficiency`, `flip_efficiency`. |
| DDR5 memory temp | Uncomment `ram_temp` (requires `spd5118` kernel module). |
| Detailed frametime graph | Uncomment `frame_timing_detailed`. |
| Nerd Font | Uncomment `font_file=` and point it at an installed font. |

> **Goverlay note:** the GUI will edit the same file but may strip your comments. Treat this file as the canonical source and copy values back after a Goverlay session.

## Files

```
mangohud-gtr9-pro-v1.0.1/
├── CHANGELOG.md
├── MangoHud.conf      # drop into ~/.config/MangoHud/
└── README.md
```

## License

MIT
