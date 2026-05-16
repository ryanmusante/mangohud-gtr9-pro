mangohud-gtr9-pro ChangeLog
===========================

v1.0.2 - 2026-05-16
-------------------

Documentation and packaging cleanup. Active directive set unchanged.

  * conf: removed all commented-out optional directives and the
    "Intentionally OFF" list. File now contains only the recommended
    active set (87 directives) plus section headers and the USERNAME
    substitution hint. Active set byte-identical to v1.0.1.
  * conf: `fps_limit_method` rationale narrowed to gamescope#1917.
  * doc: README — Strix Halo UMA caveat rewritten. `vram` reflects the
    BIOS-carved UMA Frame Buffer Size (`mem_info_vram_total`),
    independent of `ttm.pages_limit` (mainline) / `amdttm.pages_limit`
    (DKMS). Kernel >= 6.16.9 makes both unnecessary (ROCm#5444,
    ROCm#5562).
  * doc: README — install collapsed to `mkdir + sed > dst + chmod`.
    xdotool removed from Verify (X11-only on Wayland target). RAPL fix
    moved to a persistent `/etc/tmpfiles.d/99-rapl-readable.conf` drop-
    in. CVE-2020-8694 and CVE-2020-8695 cited.
  * doc: README — added **Example HUD output**, **Known issues**,
    **Troubleshooting**, **References** sections. Removed Customization
    section (it described uncommenting now-removed directives).
    Migrated to GitHub admonitions.
  * doc: README — tables trimmed to vital columns; Verify reduced to
    three smoke-test commands.
  * doc: CHANGELOG — title `ChangeLog`, headers `vN.N - YYYY-MM-DD`,
    single-blank separators (kernel.org style, matches `ry-install`).

v1.0.1 - 2026-05-10
-------------------

Resync release — corrects directive forms to match upstream MangoHud
master (data/MangoHud.conf at HEAD, MangoHud 0.8.3) and refreshes
documentation. No behavioural change at runtime — older forms parsed
identically; new forms are the canonical upstream syntax.

  * conf: change `legacy_layout=false` to `legacy_layout=0` (upstream
    uses `0`/`1` for boolean toggles).
  * conf: change `fps_metrics=AVG,0.01,0.001` to
    `fps_metrics=avg,0.01,0.001` (lowercase matches upstream example).
  * conf: change `frame_timing=1` to bare `frame_timing` (canonical
    bare-flag form per upstream).
  * conf: clarify `font_size_secondary` comment — at `font_size=22`,
    the default 0.55 ratio already yields ~12 px.
  * conf: refine "Intentionally OFF" note for `fan` — Strix Halo's
    unified fan is platform/EC-controlled, not amdgpu.
  * doc: README converted to GitHub style with badges, TOC, and
    customization table.
  * doc: README — soften the Mesa requirement (MangoHud version is
    what matters; junction temp benefits from Mesa 25+).
  * doc: README — RAPL check uses `test -r` for fish idiomaticity.
  * doc: README — add `gamemoderun mangohud %command%` Steam example.

  Compatibility: requires MangoHud `>= 0.8.3`; CachyOS
  `cachyos-extra-v4/mangohud-0.8.3-2.1` and Arch
  `extra/mangohud 0.8.3-2` both qualify.

v1.0.0 - 2026-05-10
-------------------

Initial release. Horizontal, top-left-anchored MangoHud configuration
for the Beelink GTR9 Pro (Ryzen AI Max+ 395 / Radeon 8060S, gfx1151,
Strix Halo) on CachyOS Wayland with the RADV Vulkan stack.

  * Layout: `horizontal`, `horizontal_stretch`, `legacy_layout=0`,
    `table_columns=20`, `hud_no_margin`, `cellpadding_y=-0.085`,
    `position=top-left`, `round_corners=8`.
  * Typography: `font_size=22`, `text_outline`, `background_alpha=0.55`.
  * FPS metrics: `fps_metrics=avg,0.01,0.001`, `frame_count`,
    `frame_timing`, `frametime`, `dynamic_frame_timing`,
    `throttling_status_graph`; color thresholds at 30/60.
  * FPS limiter: cycle `0,240,144,60,30`, `fps_limit_method=late`
    (gamescope#1917).
  * GPU (Radeon 8060S): load, edge + junction + mem temp, core + mem
    clock, package power, voltage, throttle reason; thresholds 50/90;
    `gpu_text=iGPU`.
  * CPU (Ryzen AI Max+ 395, 16C/32T Zen 5): aggregate load, package
    temp, top-core MHz, RAPL package power; thresholds 50/90.
  * Memory (UMA-aware): `vram` + `ram` + `swap` + `proc_vram`.
  * Wine / Vulkan: `engine_version`, `engine_short_names`,
    `vulkan_driver`, `wine`, `winesync`, `arch`, `exec_name`,
    `present_mode`, `resolution`, `display_server`.
  * Status: `gamemode`, `vkbasalt`, `throttling_status`, `time` (`%H:%M`).
  * Logging: `output_folder`, `log_interval=100`, `log_duration=60`,
    `permit_upload=1`, `benchmark_percentiles=97,AVG,1,0.1`.
  * Keybinds: `Shift_R+F12/F11/F10/F9`, `Shift_L+F1/F2/F3/F4`.
  * Blacklist: Steam helpers, Proton tooling, gamescope-session, zenity.

  Known issues at release: `gpu_junction_temp` 0 on some kernel/Mesa
  combinations (upstream #2001); native Wayland OpenGL gap (upstream
  #477); `output_folder` USERNAME placeholder; `cpu_power` requires
  RAPL world-readable.
