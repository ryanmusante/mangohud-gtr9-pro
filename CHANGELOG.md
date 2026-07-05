# Changelog

Notable changes, newest first. Versioning follows SemVer.

# 1.17.0 - 2026-07-04

  * Reorder both blocks to temp-first: GPU is now load, temp, clock,
    power, matching MangoHud's upstream default template and the bulk
    of community configs. CPU follows the same shape (load, freq,
    power). This supersedes the 1.16.0 load, power, clock, temp
    ordering shipped earlier the same day.
  * Position the commented cpu_temp stub in slot two of the CPU block
    so uncommenting it yields load, temp, freq, power, parallel to the
    GPU block.
  * README: resync the Options table, the intro block order, and the
    example line to the new element order.

# 1.15.0 - 2026-07-03

  * Add cpu_power to the bar for CPU package draw, matching the GPU
    block which already reports gpu_power. Config now 19 active
    directives plus one commented cpu_temp stub.
  * Keep cpu_temp commented: the CPU package sensor is not reported on
    the GTR9 Pro (Strix Halo), so the directive would render nothing.
    The stub is retained to document the intent.
  * README: add cpu_power to the Options table, the intro CPU block,
    and the example; state that only gpu_temp is shown and why
    cpu_temp stays off.

# 1.14.0 - 2026-06-27

  * Restore lockstep with ry-install's embedded config: re-add
    toggle_hud=Shift_R+F12, gpu_power, text_outline, and a commented
    cpu_temp stub. The 1.13.0 reconcile dropped these, but the
    installer still ships them; this realigns to the installer as
    source of truth.
  * Config now 18 active directives plus one commented stub.
  * README: add gpu_power, text_outline, and the commented cpu_temp to
    the Options table; restore power to the GPU block and the
    temp/power readout in the example; note toggle_hud is now set
    explicitly.

# 1.13.0 - 2026-06-22

  * Reconcile config to the layout deployed by ry-install: FPS-first
    order, 16 directives.
  * Drop gpu_power, cpu_temp, fps_metrics, text_outline, toggle_hud
    (Shift_R+F12 stays via MangoHud default).
  * Resync README to the new set.

# 1.12.1 - 2026-06-19

  * Strip inline comments from MangoHud.conf; keep one-line header.
  * README: fold per-directive notes into a single Options section.

# 1.12.0 - 2026-06-19

  * Reorder GPU block to load, clock, temp, power to match CPU block
    order.
  * Trim header to one line; drop throttling_status and core_load
    stubs.

# 1.11.1 - 2026-06-16

  * Drop section banners and blank lines; keep header and inline
    comments.

# 1.11.0 - 2026-06-15

  * Reorder bar to GPU, CPU, memory, FPS for the common horizontal
    layout.

# 1.10.0 - 2026-06-15

  * Add gpu_power, cpu_temp, fps_metrics (1% / 0.1% lows).
  * Drop gpu_mem_clock and swap; not useful on a shared-memory APU.
  * Note vram shows only the BIOS carveout; ram is the figure to
    watch.

# 1.9.0 - 2026-06-15

  Consolidated 1.0.0 through 1.9.0 (2026-05-10 to 2026-06-15), the
  initial development series, newest first:

  * Bump MangoHud floor 0.8.3 to 0.8.4 (Steam Overlay fix); document
    the ry-install MANGOHUD=1 auto-enable integration.
  * Sync docs to MangoHud.conf as source of truth; readout-only bar,
    19 directives. Drop bundled uppercase font, font_file step, and
    license carve-out. Remove example.png in favour of a text sketch
    of element order.
  * 1.7.0 skipped; no release published. Recorded so history has no
    gap.
  * Add an example-output section and example.png preview, re-rendered
    in one font, size, and color; name the horizontal directive and
    split GPU core and memory clocks in the docs.
  * Remove gpu_junction_temp; hotspot mirrors the edge value gpu_temp
    reports.
  * Remove frame_count, gpu_name, vulkan_driver, engine_version.
  * Briefly expand to a diagnostics set (fps_metrics, memory temps,
    core_load, resolution, swap, adapter/driver IDs) before trimming
    back.
  * Remove throttling_status and throttling_status_graph.
  * Switch to a horizontal top bar trimmed to the common gamer layout
    with temps.
  * Minimise from 87 to 17 directives, aligned with typical CachyOS
    configs; resync to MangoHud 0.8.3 directive forms and move the
    README to GitHub style.
  * Initial release: horizontal bar for Strix Halo on CachyOS Wayland
    and RADV.
