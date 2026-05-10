mangohud-gtr9-pro Changelog
===========================


v1.0.1 (2026-05-10)
-------------------

Resync release — corrects directive forms to match upstream MangoHud master
(data/MangoHud.conf at HEAD, MangoHud 0.8.3) and refreshes documentation. No
behavioural change at runtime — older forms parsed identically; new forms are
the canonical upstream syntax.

  * conf: change `legacy_layout=false` to `legacy_layout=0` (upstream uses
    `0`/`1` for boolean toggles).
  * conf: change `fps_metrics=AVG,0.01,0.001` to `fps_metrics=avg,0.01,0.001`
    (lowercase `avg` matches upstream example).
  * conf: change `frame_timing=1` to bare `frame_timing` (canonical bare-flag
    form per upstream).
  * conf: clarify `font_size_secondary` comment — at `font_size=22`, the
    upstream default of `0.55 * font_size` already yields ~12 px, so the
    previous suggested override was a no-op; reworded as a true override.
  * conf: add commented `frame_timing_detailed`, `gpu_power_limit`, `gpu_fan`,
    `gpu_name`, `gpu_efficiency`, `cpu_efficiency`, `flip_efficiency`,
    `core_load_change` for power-aware tuning on a UMA APU.
  * conf: refine "Intentionally OFF" note for `fan` — Strix Halo's unified
    fan is platform/EC-controlled, not amdgpu, so `gpu_fan` typically reads
    zero on this hardware.
  * doc: README converted to GitHub style with badges, table of contents,
    and customization table.
  * doc: README — soften the Mesa version requirement (MangoHud version is
    what matters; junction temp benefits from Mesa 25+ but works earlier).
  * doc: README — RAPL check command reworked to use `test -r` for fish
    shell idiomaticity.
  * doc: README — add `gamemoderun mangohud %command%` Steam launch example.

  Compatibility: requires MangoHud `>= 0.8.3`; CachyOS `cachyos-extra-v4/
  mangohud-0.8.3-2.1` and Arch `extra/mangohud 0.8.3-2` both qualify.


v1.0.0 (2026-05-10)
-------------------

Initial release. Horizontal, top-left-anchored MangoHud configuration for
the Beelink GTR9 Pro (Ryzen AI Max+ 395 / Radeon 8060S, gfx1151, Strix Halo)
on CachyOS Wayland with the RADV Vulkan stack.

Layout & rendering:

  * Horizontal status bar (`horizontal`, `horizontal_stretch`,
    `legacy_layout=0`, `table_columns=20`, `hud_no_margin`,
    `cellpadding_y=-0.085`).
  * Top-left anchor (`position=top-left`).
  * 4K-tuned typography (`font_size=22`, `text_outline`,
    `background_alpha=0.55`, `round_corners=8`).

Performance metrics:

  * FPS plus AVG / 1% low / 0.1% low via `fps_metrics=avg,0.01,0.001`.
  * Frametime diagnostics: `frame_timing`, `frametime`,
    `dynamic_frame_timing`, `throttling_status_graph`, `frame_count`.
  * FPS color thresholds: red `<30`, yellow `<60`, green otherwise.
  * FPS limiter cycle `0,240,144,60,30` with `fps_limit_method=late`
    (lowest input latency; `early` causes VRR spikes per gamescope #1917).

GPU sensors (Radeon 8060S, gfx1151):

  * Load, edge temp, junction (hotspot) temp, memory temp, core clock,
    memory clock, package power, voltage, throttle reason.
  * Threshold coloring at 50/90% with AMD red / amber / green.
  * `gpu_text=iGPU` to label the column on this UMA APU.

CPU sensors (Ryzen AI Max+ 395, 16C/32T Zen 5):

  * Aggregate load, package temp, top-core MHz, RAPL package power.
  * Threshold coloring at 50/90%.
  * Per-core grid available but commented (heavy on 32 threads).

Memory (UMA-aware):

  * `vram` + `ram` + `swap` + `proc_vram` together — VRAM alone reflects
    only the BIOS-carved window, not Strix Halo's full GTT-backed pool.

Wine / Vulkan diagnostics:

  * `engine_version`, `engine_short_names`, `vulkan_driver`, `wine`,
    `winesync`, `arch`, `exec_name`, `present_mode`, `resolution`,
    `display_server`.

Status indicators:

  * `gamemode`, `vkbasalt`, `throttling_status`, `time` (`%H:%M`).

Logging (FlightlessMango.com workflow):

  * `output_folder`, `log_interval=100`, `log_duration=60`,
    `permit_upload=1`, `benchmark_percentiles=97,AVG,1,0.1`.

Keybinds (non-conflicting with Steam / Discord):

  * Toggles: `Shift_R+F12` HUD, `Shift_R+F11` position, `Shift_R+F10`
    preset, `Shift_R+F9` reset metrics.
  * Logging: `Shift_L+F1` FPS limit, `Shift_L+F2` log toggle,
    `Shift_L+F3` upload, `Shift_L+F4` reload config.

Quality of life:

  * Blacklist for Steam helpers, Proton tooling, gamescope-session, zenity.

Known issues:

  * `gpu_junction_temp` may report 0 on some Mesa/kernel combinations
    (upstream #2001). Confirmed working on RDNA 3.5 with kernel `>=6.14`
    and Mesa `>=25.0`.
  * Native Wayland OpenGL games may not load the overlay (upstream #477);
    Vulkan and XWayland paths are reliable.
  * `output_folder` requires manual replacement of the `USERNAME`
    placeholder; the `sed` command in the README handles this.
  * `cpu_power` returns 0 W unless `/sys/class/powercap/intel-rapl:0/
    energy_uj` is world-readable.
