mangohud-gtr9-pro changelog

1.7.0 (2026-06-14)
- Force all HUD text uppercase with a bundled font (GTR9-Caps-Mono.ttf,
  loaded via font_file). MangoHud has no uppercase option and renders ms,
  MHz, GiB, and the Resolution label as fixed mixed-case strings; the font
  remaps lowercase glyphs to uppercase, so every string renders in caps.
- Add legacy_layout=0 so the bar follows the order written in the file.
  Regroup all directives into labelled sections. 24 directives, all
  readout-only.
- Correct the docs to match real rendering: memory temp and clock render
  in the VRAM group, swap as a bare value after RAM, resolution uses a
  lowercase-x separator, fps metrics carry an FPS suffix. Add the font
  install step, FONT-LICENSE, and a license note.

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
