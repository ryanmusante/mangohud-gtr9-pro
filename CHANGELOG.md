mangohud-gtr9-pro changelog

1.8.0 (2026-06-14)
- Sync docs to MangoHud.conf as the source of truth. The config is a
  plain readout-only bar of 19 directives with no uppercase font, so
  remove every doc reference that the config does not implement.
- Drop the bundled uppercase font (GTR9-Caps-Mono.ttf), the font_file
  install step, FONT-LICENSE, and the license carve-out; the config sets
  no font_file and renders MangoHud's default mixed-case strings.
- Drop fps_metrics (1% / 0.1% lows), cpu_temp, gpu_mem_temp, and
  resolution from the metrics table, prose, and example; none are present
  in the config.
- Remove example.png and its README embed. The previous image depicted
  the old uppercase build (lows, CPU/VRAM temps, resolution) and could
  not be regenerated accurately without a real capture, so the example
  is now a text sketch of element order only.
- Correct the directive count to 19, all readout-only.

1.6.2 (2026-06-14)
- Re-render example.png in one font, one size, one color.

1.6.1 (2026-06-14)
- Doc fixes: name the horizontal directive instead of preset=2, and split
  the GPU core and memory clocks into separate tokens in the example.

1.6.0 (2026-06-14)
- Add an example-output section and an example.png preview.

1.5.0 (2026-06-14)
- Remove gpu_junction_temp; on this APU the hotspot sensor mirrors the
  edge value that gpu_temp already reports.

1.4.0 (2026-06-14)
- Remove frame_count, gpu_name, vulkan_driver, and engine_version.

1.3.0 (2026-06-14)
- Expand to a diagnostics set: fps_metrics, memory temps, core_load,
  resolution, swap, and adapter/driver identification.

1.2.0 (2026-06-14)
- Remove throttling_status and throttling_status_graph.

1.1.0 (2026-06-03)
- Switch to a horizontal top bar trimmed to the common gamer layout with
  temperatures.

1.0.2 (2026-05-16)
- Minimise from 87 to 17 directives, aligned with typical CachyOS configs.

1.0.1 (2026-05-10)
- Resync to MangoHud 0.8.3 directive forms. README moved to GitHub style.

1.0.0 (2026-05-10)
- Initial release: horizontal bar for Strix Halo on CachyOS Wayland and
  RADV with FPS metrics, GPU/CPU sensors, and UMA-aware memory readout.
