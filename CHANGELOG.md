mangohud-gtr9-pro ChangeLog
===========================

v1.0.2 - 2026-05-16
-------------------

Minimisation — aligned with typical CachyOS-gamer MangoHud configs.

  * conf: stripped from 87 to 17 active directives. Kept core fps +
    frametime + frame_timing; GPU stats/temp/power + vram + proc_vram
    (UMA awareness); CPU stats/temp; ram; minimal layout; one toggle
    keybind. Removed per-metric color thresholds, throttling graph,
    engine/wine/vulkan info row, logging block, fps_limit cycle,
    benchmark presets, blacklist, gamemode/vkbasalt indicators, time,
    `legacy_layout`/`horizontal`/`table_columns` row layout.
  * doc: README trimmed to install + Steam launch + toggle keybind.
    Removed Compatibility-extras, Verify, Default-keybinds table,
    Example-HUD section, Strix-Halo UMA caveat, RAPL note, Known
    issues, Troubleshooting, References (single MangoHud upstream
    badge link kept).

v1.0.1 - 2026-05-10
-------------------

Resync to upstream MangoHud 0.8.3 directive forms. No behavioural change.

  * conf: `legacy_layout=0`, `fps_metrics=avg,...`, bare `frame_timing`.
  * doc: README converted to GitHub style with badges and TOC.

v1.0.0 - 2026-05-10
-------------------

Initial release. Horizontal top-left bar tuned for Strix Halo on
CachyOS Wayland / RADV with FPS metrics, GPU/CPU sensors, UMA-aware
memory readout, logging hotkeys, and gamescope-safe FPS limiter.
