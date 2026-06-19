# Changelog

All notable changes to this config are recorded here, newest first. Versioning is [Semantic Versioning](https://semver.org); entries follow [Keep a Changelog](https://keepachangelog.com) conventions.

## 1.12.1 (2026-06-19)
- MangoHud.conf: strip all inline comments, keeping only the one-line header. No directive, order, or value change.
- README: replace the Metrics section with a single Options section documenting all 20 directives in config order; the per-directive descriptions removed from the config now live here.

## 1.12.0 (2026-06-19)
- Reorder the GPU block to load, clock, temperature, power so it matches the CPU block's load, clock, temperature order. Render order only; same directives and values. README example and metrics table resequenced to match.
- MangoHud.conf: trim the header to one line, drop the `throttling_status` and `core_load` opt-in stubs, and strip trailing whitespace on the `toggle_hud` line. Active directive count stays 20.
- README: drop the "shipped commented-out — uncomment to enable" note for the removed stubs.

## 1.11.1 (2026-06-16)
- MangoHud.conf: drop the section banners and blank lines; keep the two-line header and inline comments. No directive, order, or value change.

## 1.11.0 (2026-06-15)
- Reorder the bar to GPU, CPU, memory, FPS to match the common horizontal layout (upstream config, Steam Deck preset, GamingOnLinux). Same directives, render order only; README example and metrics table resequenced to match.

## 1.10.0 (2026-06-15)
- Add `gpu_power`, `cpu_temp`, and `fps_metrics=avg,0.01,0.001` (1% / 0.1% lows).
- Drop `gpu_mem_clock` and `swap`; neither is useful on this shared-memory APU.
- Note in the `vram` comment that it shows only the small BIOS carveout; `ram` is the figure to watch.
- Ship `throttling_status` and `core_load` commented-out (opt-in).
- Active directive count 19 to 20; README resynced to the new set.

## 1.9.0 (2026-06-15)
- Bump the MangoHud floor 0.8.3 to 0.8.4 (CachyOS ships 0.8.4; Steam Overlay fix).
- MangoHud.conf: tidy comments to one-directive-per-line form.
- README: document the ry-install integration (`MANGOHUD=1` auto-enable).

## 1.8.0 (2026-06-14)
- Sync docs to MangoHud.conf as the source of truth. The config is a plain readout-only bar of 19 directives with no uppercase font, so remove every doc reference that the config does not implement.
- Drop the bundled uppercase font (GTR9-Caps-Mono.ttf), the `font_file` install step, FONT-LICENSE, and the license carve-out; the config sets no `font_file` and renders MangoHud's default mixed-case strings.
- Drop `fps_metrics` (1% / 0.1% lows), `cpu_temp`, `gpu_mem_temp`, and `resolution` from the metrics table, prose, and example; none are present in the config.
- Remove example.png and its README embed. The previous image depicted the old uppercase build (lows, CPU/VRAM temps, resolution) and could not be regenerated accurately without a real capture, so the example is now a text sketch of element order only.
- Correct the directive count to 19, all readout-only.

## 1.7.0

- Version skipped; no release was published under this number. Recorded here so the history has no silent gap.

## 1.6.2 (2026-06-14)
- Re-render example.png in one font, one size, one color.

## 1.6.1 (2026-06-14)
- Doc fixes: name the `horizontal` directive instead of `preset=2`, and split the GPU core and memory clocks into separate tokens in the example.

## 1.6.0 (2026-06-14)
- Add an example-output section and an example.png preview.

## 1.5.0 (2026-06-14)
- Remove `gpu_junction_temp`; on this APU the hotspot sensor mirrors the edge value that `gpu_temp` already reports.

## 1.4.0 (2026-06-14)
- Remove `frame_count`, `gpu_name`, `vulkan_driver`, and `engine_version`.

## 1.3.0 (2026-06-14)
- Expand to a diagnostics set: `fps_metrics`, memory temps, `core_load`, `resolution`, `swap`, and adapter/driver identification.

## 1.2.0 (2026-06-14)
- Remove `throttling_status` and `throttling_status_graph`.

## 1.1.0 (2026-06-03)
- Switch to a horizontal top bar trimmed to the common gamer layout with temperatures.

## 1.0.2 (2026-05-16)
- Minimise from 87 to 17 directives, aligned with typical CachyOS configs.

## 1.0.1 (2026-05-10)
- Resync to MangoHud 0.8.3 directive forms. README moved to GitHub style.

## 1.0.0 (2026-05-10)
- Initial release: horizontal bar for Strix Halo on CachyOS Wayland and RADV with FPS metrics, GPU/CPU sensors, and UMA-aware memory readout.
