mangohud-gtr9-pro ChangeLog
===========================

v1.5.0 - 2026-06-14
-------------------

Dropped a redundant sensor readout. Still 100% readout-only.

  * conf: removed `gpu_junction_temp`. On this APU (gfx1151) the
    junction/hotspot sensor mirrors the edge sensor that `gpu_temp`
    already reports, so it only duplicated the GPU temperature rather
    than exposing a distinct hotspot value as it does on discrete RDNA
    cards. GPU temperatures are now `gpu_temp` (edge) + `gpu_mem_temp`
    (memory). Layout, style, and the single toggle keybind are
    unchanged. Net 22 active directives, all readout-only.
  * doc: README description, Metrics table GPU-temp row, and the
    sensor note updated to reflect the removal and its rationale.
    Version badge bumped to 1.5.0.

v1.4.0 - 2026-06-14
-------------------

Trimmed for an effectively all-uppercase readout. Still 100%
readout-only; no performance or workaround directives.

  * conf: removed `frame_count` (frame counter), and removed
    `gpu_name`, `vulkan_driver`, and `engine_version`. The latter three
    are the only free-form strings MangoHud renders in lowercase (e.g.
    "radv", "vkd3d") and MangoHud has no uppercase/all-caps option, so
    dropping them leaves a bar whose labels are all uppercase; the only
    remaining mixed case is MangoHud's fixed unit suffixes (`ms`,
    `MHz`, `GiB`), which are not configurable. Layout, style, and the
    single toggle keybind are unchanged. Net 23 active directives, all
    readout-only.
  * doc: README Metrics table dropped the removed rows; added a note
    explaining the omitted identification fields and the uppercase
    rationale. Version badge bumped to 1.4.0.
  * Verified against upstream MangoHud master config: no uppercase/
    capitalization directive exists anywhere in the parameter set.

v1.3.0 - 2026-06-14
-------------------

Expanded from the lean readout to a diagnostics-oriented set. Still
100% readout-only; no performance or workaround directives.

  * conf: added ten display metrics. FPS group gains
    `fps_metrics=avg,1,0.1` (1% / 0.1% lows on the HUD) and
    `frame_count`. GPU group gains `gpu_junction_temp` and
    `gpu_mem_temp` (hotspot / memory-side temps; mem temp needs vram),
    plus `gpu_name`, `vulkan_driver`, and `engine_version` for adapter
    and driver/engine identification, and `resolution`. CPU group gains
    `core_load` (per-core). Memory gains `swap` to complete the
    `vram` + `ram` + `swap` triad. Layout, style, and the single toggle
    keybind are unchanged. Net 28 active directives, all readout-only.
  * doc: README reframed from "Minimal" to "Expanded"; the old
    "Example output" section replaced with a "Metrics" table covering
    every directive, plus notes on the AMD edge-vs-junction sensor, the
    vram dependency, and the shared LPDDR5X pool. TOC and version badge
    bumped to 1.3.0.
  * Verified against upstream MangoHud master config: all ten additions
    are in the DISPLAY sections, correctly named, and none come from
    PERFORMANCE or WORKAROUNDS.

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
