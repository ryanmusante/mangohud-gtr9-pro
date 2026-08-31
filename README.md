# mangohud-gtr9-pro

**Version 1.19.0** · [Changelog](CHANGELOG.md)

Readout-only [MangoHud](https://github.com/flightlessmango/MangoHud) config for the Beelink GTR9 Pro (Ryzen AI Max+ 395 / gfx1151 / Strix Halo) on CachyOS Wayland with RADV. 18 directives, one horizontal top bar, display-only — nothing here changes frame pacing, rendering, or power state.

## Quick Start

> [!NOTE]
> Under [ry-install](https://github.com/ryanmusante/ry-install) this file is a managed destination and is already deployed. Copy it by hand only outside that setup.

```fish
mkdir -p ~/.config/MangoHud
cp ./MangoHud.conf ~/.config/MangoHud/
```

## Requirements

| Component | Floor | Reason |
|---|---|---|
| MangoHud | `0.8.4` | Steam Overlay coexistence fix — Vulkan layer procaddr and trampolining |
| MangoHud | `0.8.2` | `gpu_metrics` v3_0 parsing — the APU path behind every readout here |
| Mesa / RADV | `24.1` | first release with gfx1151 RadeonSI and RADV enablement |
| Kernel | `6.14` | amdgpu gfx1151 support and stable `gpu_metrics` v3_0 export |
| Session | Wayland or X11 | keybinds work on both; Wayland is the deployed target |

CachyOS ships `mangohud` at `0.8.4-1.1`, so the repository default satisfies every floor. Under ry-install, `mangohud` and `lib32-mangohud` arrive as dependencies of `cachyos-gaming-applications`, which is in the installer's `PKGS_ADD` — no separate install step.

## Usage

```
mangohud %command%
gamemoderun mangohud %command%
```

Under ry-install the overlay is enabled for every Vulkan app through `MANGOHUD=1` in `~/.config/environment.d/10-environment.conf`, so the prefix is optional. Toggle the HUD with `Shift_R+F12`. Move the bar by editing `position=` — `top-center`, `top-right`, `middle-left`, `middle-right`, `bottom-left`, `bottom-center`, `bottom-right`. Changes apply at the next application launch; no service restart and no session logout.

## Bar Layout

One horizontal row, top-left. Both blocks read load, temperature, clock, power; `cpu_temp` is the only empty slot ([Deliberate Omissions](#deliberate-omissions)). Illustrative — element order only, not exact runtime formatting.

```
FPS 142  8.5 ms  [graph]  GPU 96% 74°C 2901 MHz 102 W  CPU 38% 4102 MHz 42 W  VRAM 0.5 GiB  RAM 13.2 GiB
```

## Directives

In file order. **Default** marks a directive whose value equals MangoHud's own default, kept explicit so the file is self-describing and immune to upstream default changes — 6 of 18. The rest are overrides.

| # | Directive | Default | Effect |
|---|---|---|---|
| 1 | `horizontal` | no | single top-of-screen row instead of the stacked block |
| 2 | `legacy_layout=0` | no | render strictly in config element order |
| 3 | `position=top-left` | yes | bar location |
| 4 | `toggle_hud=Shift_R+F12` | yes | show and hide bind |
| 5 | `fps` | yes | current frame rate |
| 6 | `frametime` | no | frametime in ms, inline with FPS |
| 7 | `frame_timing` | yes | frametime line graph |
| 8 | `gpu_stats` | yes | GPU load percent |
| 9 | `gpu_temp` | no | GPU temperature |
| 10 | `gpu_core_clock` | no | GPU core clock |
| 11 | `gpu_power` | no | GPU power draw in watts |
| — | `# cpu_stats` | — | commented stub, slot 1 of the CPU block; load still renders |
| — | `# cpu_temp` | — | commented stub, slot 2 of the CPU block |
| 12 | `cpu_mhz` | no | CPU frequency, highest active core |
| 13 | `cpu_power` | no | CPU power draw in watts |
| 14 | `vram` | no | GPU VRAM usage |
| 15 | `ram` | no | system RAM usage |
| 16 | `font_size=20` | no | upstream default is `24` |
| 17 | `text_outline` | yes | outline glyphs for legibility over bright frames |
| 18 | `background_alpha=0.4` | no | backdrop opacity, `0.0` to `1.0`; upstream default is `0.5` |

`fps`, `frame_timing` and `gpu_stats` are on by default upstream and can only be turned off with `=0`; listing them is documentation, not activation. `cpu_stats` is in that same class but is commented here to hold parity with the installer — CPU load percent still renders.

The file carries no header comment and no blank lines — bare directives plus the two commented stubs. Comment syntax is `#` at line start.

## Sensor Sources

Strix Halo is an APU, so MangoHud takes its APU code paths. Every GPU figure and the CPU power figure come from one file, `/sys/class/drm/card*/device/gpu_metrics` at format revision 3 — not from hwmon. Verified against MangoHud `v0.8.4` sources, `src/amdgpu.cpp` and `src/cpu.cpp`.

| Field | Source | Exact value |
|---|---|---|
| `fps`, `frametime`, `frame_timing` | swapchain present timing | driver-independent |
| `gpu_stats` | `gpu_metrics` v3_0 | `average_gfx_activity` |
| `gpu_temp` | `gpu_metrics` v3_0 | `temperature_gfx / 100` — GFX die, not an edge sensor |
| `gpu_core_clock` | `gpu_metrics` v3_0 | `average_gfxclk_frequency` — interval average |
| `gpu_power` | `gpu_metrics` v3_0 | `average_gfx_power / 1000` — GFX rail, not APU package |
| `cpu_stats` | `/proc/stat` | aggregate jiffy deltas |
| `cpu_mhz` | `cpufreq/scaling_cur_freq` | per core, max of set; served by `amd-pstate-epp` here |
| `cpu_power` | `gpu_metrics` v3_0 | `average_apu_power - average_gfx_power`, clamped at 0 |
| `vram` | amdgpu sysfs | `mem_info_vram_used` over `mem_info_vram_total` |
| `ram` | `/proc/meminfo` | `MemTotal` minus `MemAvailable` |

**`ram` is the memory figure to watch** — on a unified-memory part `vram` reports the small BIOS carveout, not the pool a game draws from. **`gpu_temp` and `gpu_power` do not sum to a package figure** — `temperature_soc` and `average_apu_power` sit in the same struct but no directive in this config exposes them.

**`cpu_power` does not use RAPL here.** MangoHud's selection order is hwmon (`k10temp`, `zenpower`, `zenergy`, `apm_xgene`), then the amdgpu APU metric, then powercap RAPL. On Zen 5 `k10temp` exposes temperature inputs only — no `Pcore` or `Psoc`, no `Vcore` or `Icore` — so its initializer returns null and the APU metric wins. Making `energy_uj` world-readable is not required on this box.

**`cpu_custom_temp_sensor` is inert here.** It steers only the hwmon file lookup, and `UpdateCpuTemp()` short-circuits to `gpu_metrics` on any APU before that file is read. It cannot correct a CPU temperature reading on this hardware.

## Deliberate Omissions

Not oversights. Each shipped at some point and was removed for the stated reason; measure before re-adding.

| Directive | Reason |
|---|---|
| `cpu_temp` | [MangoHud #1794](https://github.com/flightlessmango/MangoHud/issues/1794), open since 2025-09-04 — see below |
| `gpu_junction_temp` | hotspot mirrors what `gpu_temp` already reports |
| `gpu_mem_clock` | UMA part — memory clock is system memory clock, not a GPU-side lever |
| `swap` | no meaningful swap pressure under the zram configuration deployed here |
| `fps_metrics` | 1% and 0.1% lows are a benchmarking readout, not a play-time one |
| `throttling_status`, `throttling_status_graph` | v3_0 exposes delta counters for temperature and power only, with no current or other class, so the display is partial |
| `frame_count`, `gpu_name`, `vulkan_driver`, `engine_version` | static or near-static; consume bar width for no live signal |

> [!WARNING]
> \#1794 reports that on Zen 5, enabling `cpu_temp` drives `cpu_power` to 0. It was filed from a discrete-GPU desktop reading `cpu_power` from RAPL — a different source than this machine uses. The mechanism is therefore **untested on the `gpu_metrics` path**. The stub stays commented to hold parity with the installer, not because the coupling is confirmed here. Uncomment `cpu_temp` on its own line and check whether `cpu_power` still reports non-zero before concluding either way.

## Lockstep

The installer's embedded generator is the source of truth. Divergence in the directive set is a defect in this repository, not in the installer; reconcile toward the installer. Since `7.190.0` the installer ships as a pair — [ry-install](https://github.com/ryanmusante/ry-install) deploys and [ry-verify](https://github.com/ryanmusante/ry-verify) checks — and the generator body is byte-identical in both, so either script pins this file.

| Property | Value |
|---|---|
| Installer baseline | `7.193.0` |
| Generator | `_content_HOME_.config_MangoHud_MangoHud.conf` |
| Managed destination | `~/.config/MangoHud/MangoHud.conf`, mode `0600` |
| Post-hook tag | `mangohud` — announces that the change applies at next launch |
| Format validator | `_grep_mangohud_entry` — needs one bareword or `key=value` directive |
| Runtime check | `--verify` in `ry-verify.fish` — greps the deployed file for `fps` |
| Directive parity | 18/18 identical in set and order |
| Comment delta | 2 lines — this repository omits the installer's header comment and its inert-sensor note, and carries both stubs bare |

## Verify

```fish
diff (string match -rv '^#' < ~/.config/MangoHud/MangoHud.conf | psub) \
     (string match -rv '^#' < ./MangoHud.conf | psub)
```

An empty `diff` means the deployed file and this repository agree on every active directive. Byte 3 of `/sys/class/drm/card*/device/gpu_metrics` is the format revision and should read `3` on this hardware.

## License

MIT — see [LICENSE](LICENSE).
