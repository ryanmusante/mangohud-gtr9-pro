mangohud-gtr9-pro ChangeLog
===========================

v1.2.0 - 2026-06-14
-------------------

Removed the throttling indicator; pure performance/sensor readout.

  * conf: dropped `throttling_status` and `throttling_status_graph`.
    Remaining layout is FPS + frametime + frame_timing graph, GPU load +
    `gpu_core_clock` + `gpu_mem_clock` + `gpu_temp`, CPU load + `cpu_mhz`
    + `cpu_temp`, and `vram` + `ram`. Style and the single toggle keybind
    are unchanged. Net 18 active directives, all readout-only.
  * doc: README description, "Example output" preamble, and the legend
    table updated to drop the throttling row. Version badge bumped to
    1.2.0.
  * Verified against upstream MangoHud master config and the 0.8.3/0.8.4
    release notes (current release is 0.8.4, May 2026). All directives
    remain valid and display-only; none come from the PERFORMANCE or
    WORKAROUNDS sections.

v1.1.0 - 2026-06-03
-------------------

Switched back to a horizontal top-of-screen bar, trimmed to the most
common gamer layout with temperatures.

  * conf: added `horizontal`; `position` stays at the top (`top-left`).
    Layout is now FPS + frametime + frame_timing graph, GPU load +
    `gpu_core_clock` + `gpu_mem_clock` + `gpu_temp`, CPU load +
    `cpu_mhz` + `cpu_temp`, `vram` + `ram`, and `throttling_status` +
    `throttling_status_graph` (active-throttling indicator, shown only
    while throttling occurs). Dropped `gpu_power`, `proc_vram`, and
    `round_corners` to match the popular minimal set. Style kept minimal
    (`font_size=20`, `background_alpha=0.4`, `text_outline`); single
    toggle keybind. Net 20 active directives, all readout-only.
  * doc: README description updated from vertical to horizontal. Added
    an "Example output" section showing the rendered top bar with a
    legend mapping each column to its directive (including the GPU/CPU
    clocks and the conditional throttling indicator), plus a note on
    moving the bar with `position`. Files tree and version badge bumped
    to 1.1.0.

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
