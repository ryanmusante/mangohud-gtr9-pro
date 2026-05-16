# mangohud-gtr9-pro

[![version](https://img.shields.io/badge/version-1.0.2-blue.svg)](CHANGELOG.md)
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
- [Known issues](#known-issues)
- [Troubleshooting](#troubleshooting)
- [References](#references)
- [Files](#files)
- [License](#license)

## Compatibility

| Component       | Required                                                                   |
|-----------------|----------------------------------------------------------------------------|
| MangoHud        | `≥ 0.8.3` — required for `ram_temp`, `cpu_custom_temp_sensor`, Panthor support |
| Mesa / RADV     | Modern (24.x+); junction temp reliability tracks amdgpu kernel module, not Mesa |
| Kernel          | `≥ 6.14` (`6.18.4+` recommended for ntsync / amdgpu fixes)                 |
| Distro          | CachyOS — any Arch derivative works; non-Arch needs MangoHud 0.8.3 backport |
| Display server  | Wayland (XWayland and X11 also work)                                       |
| Hardware        | Beelink GTR9 Pro / Strix Halo — generic AMD APUs work; VRAM nuance is Strix-specific |

## Install

One-shot, fish shell:

```fish
mkdir -p ~/.config/MangoHud ~/mangologs
sed "s|/home/USERNAME/mangologs|$HOME/mangologs|" ./MangoHud.conf \
  > ~/.config/MangoHud/MangoHud.conf
chmod 0644 ~/.config/MangoHud/MangoHud.conf
```

> [!CAUTION]
> The `>` redirect overwrites any existing `~/.config/MangoHud/MangoHud.conf`. Back it up first if you have customizations to preserve.

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
#    On native Wayland, xdotool cannot reach Wayland-native windows (it speaks X11 only).
#    Either press Shift_L+F2 manually, or install `ydotool` (with its user service running).
mangohud vkcube &
set vkpid $last_pid
sleep 1
# Press Shift_L+F2 in the vkcube window to start logging.
sleep 60
# Press Shift_L+F2 again to stop.
kill $vkpid
ls -lh ~/mangologs/ | tail -n 5
set -l latest (ls -t ~/mangologs/*.csv 2>/dev/null | head -n 1)
test -n "$latest"; and head -n 30 $latest; or echo "no log produced yet"

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

Strix Halo's iGPU draws from two distinct memory regions, controlled independently:

| Region | What MangoHud shows | Controlled by | Typical range on 128 GB systems |
|---|---|---|---|
| BIOS-carved VRAM | `vram` column (`mem_info_vram_total`) | BIOS UMA Frame Buffer Size | `0.5 GB` (`Auto`/min) to `96 GB` (max custom) |
| GTT shared pool | not directly — covered indirectly by `ram` + `proc_vram` | `ttm.pages_limit` kernel param (in-kernel amdgpu) | bounded by remaining system RAM |

This config compensates for the hidden GTT pool by displaying `vram` + `ram` + `swap` + `proc_vram` together — total memory pressure is always visible regardless of how the iGPU got the bytes.

References:

- [ROCm issue #5444](https://github.com/ROCm/ROCm/issues/5444) — kernel `mem_info_vram_total` reporting on `gfx1151`; kernel 6.16.9+ removes the need for tuning params entirely
- [ROCm issue #5562](https://github.com/ROCm/ROCm/issues/5562) — `amdttm.*` vs `ttm.*` parameter divergence (DKMS amdgpu uses `amdttm.*`; mainline in-kernel amdgpu uses `ttm.*`)
- [ollama issue #12062](https://github.com/ollama/ollama/issues/12062) — reproducer of `mem_info_vram_total`-only memory reporting on APUs
- [AMD ROCm — Strix Halo system optimization](https://rocm.docs.amd.com/en/latest/how-to/system-optimization/strixhalo.html)

> [!IMPORTANT]
> **Growing the `vram` column** = change BIOS UMA Frame Buffer Size (reboot into firmware setup). The kernel cannot enlarge it at runtime.
>
> **Growing the GTT shared pool** = set `ttm.pages_limit=<pages>` on the kernel command line (pages are 4 KiB; `26214400` ≈ 100 GiB). Mainline in-kernel amdgpu (the `ry-install` target) uses `ttm.pages_limit`; the out-of-tree `amdgpu-dkms` module uses `amdttm.pages_limit`. **Kernel ≥ 6.16.9 makes both unnecessary** — the full pool is exposed automatically. If you use `ry-install`, edit the `KERNEL_PARAMS` profile global; `/etc/kernel/cmdline` is managed.

## RAPL CPU power note

`cpu_power` reads `/sys/class/powercap/intel-rapl:0/energy_uj`, which on Zen 5 is restricted to `root` by default (Platypus side-channel mitigation, CVE-2020-8694 + CVE-2020-8695). If the HUD shows `0 W`:

```fish
# One-shot (reverts on reboot — sysfs is regenerated):
sudo chmod o+r /sys/class/powercap/intel-rapl:0/energy_uj
```

> [!TIP]
> The sysfs node is recreated at every boot, so the `chmod` above is non-persistent. For a persistent fix on CachyOS or any systemd distro, drop in a `tmpfiles.d` rule (compatible with `ry-install`'s `/etc/tmpfiles.d/` convention):
>
> ```fish
> echo 'f /sys/class/powercap/intel-rapl:0/energy_uj 0444 root root - -' \
>   | sudo tee /etc/tmpfiles.d/99-rapl-readable.conf
> sudo systemd-tmpfiles --create /etc/tmpfiles.d/99-rapl-readable.conf
> ```
>
> Alternative: install `zenpower3` or `zenergy` for an unprivileged Zen-native power reading that doesn't need RAPL permissions at all.

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

> [!NOTE]
> Goverlay edits the same file but may strip comments. Treat this file as the canonical source and copy values back after a Goverlay session.

## Known issues

| Issue | Detail | Workaround |
|---|---|---|
| `gpu_junction_temp` reads 0 | Junction-temp reliability tracks the amdgpu kernel module version, not Mesa ([upstream #2001](https://github.com/flightlessmango/MangoHud/issues/2001)). Confirmed working on RDNA 3.5 with kernel ≥ 6.14. | Upgrade kernel; if still zero, fall back to `gpu_temp` (edge). |
| Native Wayland OpenGL games | Overlay may not load via the GL path ([upstream #477](https://github.com/flightlessmango/MangoHud/issues/477)). | Vulkan and XWayland paths are reliable; for OpenGL try `MANGOHUD_DLSYM=1`. |
| `output_folder` placeholder | The shipped `MangoHud.conf` contains `/home/USERNAME/mangologs`; the Install step rewrites it. | The Install snippet handles this; verify with `grep mangologs ~/.config/MangoHud/MangoHud.conf`. |
| `cpu_power` reads 0 W | RAPL sysfs is root-only on Zen 5 by default. | See [RAPL CPU power note](#rapl-cpu-power-note) for a persistent fix. |
| `Shift_R+F10` overrides this config | The toggle cycles MangoHud's built-in presets (`-1` default → `0` off → `1` fps-only → `2` horizontal → `3` extended → `4` detailed). | Either avoid the keybind or define a `presets.conf` in `~/.config/MangoHud/`. |

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| HUD doesn't appear in a Vulkan game | Both `mangohud` and `lib32-mangohud` installed but Vulkan layer mismatch | `vulkaninfo \| grep MANGOHUD` to confirm the layer loads; reinstall the AUR git build if the stable layer is mismatched ([Arch ref](https://bbs.archlinux.org/viewtopic.php?id=286478)) |
| HUD doesn't appear in an OpenGL game | LD_PRELOAD overridden by game launcher | Set `MANGOHUD_DLSYM=1` or prepend in the game's start script: `LD_PRELOAD=/usr/lib/mangohud/libMangoHud.so` |
| Keybinds don't work | Game window has not received the key (Wayland-native + xdotool, or wrong shift side) | Press in the game window directly; use `ydotool` for synthetic input on Wayland; check `Shift_L` vs `Shift_R` |
| `0 W` cpu_power | RAPL not readable | See [RAPL CPU power note](#rapl-cpu-power-note) |
| GPU values stuck at 0 | `gpu_stats` line removed, or amdgpu hwmon not yet populated | Confirm `gpu_stats` is uncommented; check `ls /sys/class/drm/card*/device/hwmon/` |
| HUD shows numbers but no frametime graph | Game uses an unsupported render path (e.g., DX9 without DXVK) | Force DXVK via Proton or use the Vulkan renderer in the game |
| Log files never appear | `output_folder` placeholder not substituted, or directory not writable | `ls -ld ~/mangologs` and re-run the Install snippet |

## References

- [MangoHud — flightlessmango/MangoHud](https://github.com/flightlessmango/MangoHud) — upstream
- [MangoHud master `data/MangoHud.conf`](https://raw.githubusercontent.com/flightlessmango/MangoHud/master/data/MangoHud.conf) — canonical option reference
- [MangoHud 0.8.3 release notes](https://github.com/flightlessmango/MangoHud/releases/tag/v0.8.3) — `ram_temp`, `cpu_custom_temp_sensor`, Panthor
- [MangoHud #1101](https://github.com/flightlessmango/MangoHud/issues/1101) — `table_columns` and horizontal-mode width
- [MangoHud #1957](https://github.com/flightlessmango/MangoHud/issues/1957) — `fps_metrics` decimal-form semantics
- [MangoHud #2001](https://github.com/flightlessmango/MangoHud/issues/2001) — `gpu_junction_temp` zero readings
- [MangoHud #477](https://github.com/flightlessmango/MangoHud/issues/477) — native Wayland OpenGL overlay
- [gamescope #1917](https://github.com/ValveSoftware/gamescope/issues/1917) — `fps_limit_method=early` VRR spikes in gamescope Wayland backend
- [ROCm #5444](https://github.com/ROCm/ROCm/issues/5444), [ROCm #5562](https://github.com/ROCm/ROCm/issues/5562) — Strix Halo VRAM / GTT exposure
- [AMD ROCm — Strix Halo system optimization](https://rocm.docs.amd.com/en/latest/how-to/system-optimization/strixhalo.html) — `ttm.pages_limit` reference

## Files

```
mangohud-gtr9-pro-v1.0.2/
├── CHANGELOG.md
├── MangoHud.conf      # drop into ~/.config/MangoHud/
└── README.md
```

## License

MIT
