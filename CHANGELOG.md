# Changelog

Notable changes, newest first. Versioning follows SemVer.

1.15.0 (2026-07-03)
- Activate cpu_temp: uncomment the directive to match ry-install, which toggled cpu_temp on (installer 7.73.0–7.76.1) but whose config the 1.14.0 reconcile re-added only as a commented stub. Config now 19 active directives, no commented stubs.
- README: change the cpu_temp Options row from commented to active; add CPU temperature to the intro CPU block and the example; drop the "omitted by default" note.

1.14.0 (2026-06-27)
- Restore lockstep with ry-install's embedded config: re-add toggle_hud=Shift_R+F12, gpu_power, text_outline, and a commented # cpu_temp stub. The 1.13.0 reconcile dropped these, but the installer still ships them; this realigns to the installer as source of truth.
- Config now 18 active directives plus one commented stub.
- README: add gpu_power, text_outline, and the commented cpu_temp to the Options table; restore power to the GPU block and the temp/power readout in the example; note toggle_hud is now set explicitly.

1.13.0 (2026-06-22)
- Reconcile config to the layout deployed by ry-install: FPS-first order, 16 directives.
- Drop gpu_power, cpu_temp, fps_metrics, text_outline, toggle_hud (Shift_R+F12 stays via MangoHud default).
- Resync README to the new set.

1.12.1 (2026-06-19)
- Strip inline comments from MangoHud.conf; keep one-line header.
- README: fold per-directive notes into a single Options section.

1.12.0 (2026-06-19)
- Reorder GPU block to load, clock, temp, power to match CPU block order.
- Trim header to one line; drop throttling_status and core_load stubs.

1.11.1 (2026-06-16)
- Drop section banners and blank lines; keep header and inline comments.

1.11.0 (2026-06-15)
- Reorder bar to GPU, CPU, memory, FPS for the common horizontal layout.

1.10.0 (2026-06-15)
- Add gpu_power, cpu_temp, fps_metrics (1% / 0.1% lows).
- Drop gpu_mem_clock and swap; not useful on a shared-memory APU.
- Note vram shows only the BIOS carveout; ram is the figure to watch.

1.9.0 (2026-06-15)
- Bump MangoHud floor 0.8.3 to 0.8.4 (Steam Overlay fix).
- Document the ry-install MANGOHUD=1 auto-enable integration.

1.8.0 (2026-06-14)
- Sync docs to MangoHud.conf as source of truth; readout-only bar, 19 directives.
- Drop bundled uppercase font, font_file step, and license carve-out.
- Remove example.png; replace with a text sketch of element order.

1.7.0
- Skipped; no release published. Recorded so history has no gap.

1.6.2 (2026-06-14)
- Re-render example.png in one font, size, color.

1.6.1 (2026-06-14)
- Doc fixes: name horizontal directive; split GPU core and memory clocks.

1.6.0 (2026-06-14)
- Add example-output section and example.png preview.

1.5.0 (2026-06-14)
- Remove gpu_junction_temp; hotspot mirrors the edge value gpu_temp reports.

1.4.0 (2026-06-14)
- Remove frame_count, gpu_name, vulkan_driver, engine_version.

1.3.0 (2026-06-14)
- Expand to a diagnostics set: fps_metrics, memory temps, core_load, resolution, swap, adapter/driver IDs.

1.2.0 (2026-06-14)
- Remove throttling_status and throttling_status_graph.

1.1.0 (2026-06-03)
- Switch to a horizontal top bar trimmed to the common gamer layout with temps.

1.0.2 (2026-05-16)
- Minimise from 87 to 17 directives, aligned with typical CachyOS configs.

1.0.1 (2026-05-10)
- Resync to MangoHud 0.8.3 directive forms; README to GitHub style.

1.0.0 (2026-05-10)
- Initial release: horizontal bar for Strix Halo on CachyOS Wayland and RADV.
