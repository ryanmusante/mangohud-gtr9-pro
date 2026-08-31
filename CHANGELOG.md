Changes for mangohud-gtr9-pro
=============================

Newest first. Versioning is MAJOR.MINOR.PATCH.


1.19.0
------

  - conf: comment cpu_stats for installer parity; 18 active directives
    plus commented cpu_stats and cpu_temp stubs
  - conf: cpu_stats is an upstream default that only =0 disables, so
    CPU load percent still renders
  - readme: re-pin the installer baseline from 7.141.0 to 7.193.0
  - readme: record the installer split; ry-install.fish deploys and
    ry-verify.fish checks, sharing one generator body
  - readme: resync the directive table, stub rows and default count to
    the 18-directive set


1.18.1
------

  - conf: strip the header comment and all blank lines; bare
    directives plus the commented cpu_temp stub
  - readme: adopt the ry-install layout, trading the badge block for a
    version line and moving to Title Case headings and callouts
  - changelog: adopt the ry-install format, with underlined blocks,
    hyphen bullets, entry tags and a two-line bullet ceiling
  - changelog: drop per-block dates; X - Y now denotes a version range
  - changelog: reflow historical bullets to one line where they fit,
    and normalize spelling to US


1.18.0
------

  - readme: add Sensor Sources from MangoHud v0.8.4 code; every GPU
    figure and cpu_power read gpu_metrics revision 3 on this APU
  - readme: correct gpu_temp to temperature_gfx, the GFX die
  - readme: correct gpu_power to average_gfx_power, the GFX rail only
  - readme: correct gpu_core_clock to average_gfxclk_frequency, an
    interval average rather than an instantaneous clock
  - readme: k10temp exposes no power or voltage inputs on Zen 5, so
    cpu_power takes the APU metric and never reaches RAPL
  - readme: cpu_custom_temp_sensor is inert here; UpdateCpuTemp
    short-circuits to gpu_metrics on any APU
  - readme: mark the 7 of 19 directives that restate MangoHud
    defaults; the other 12 are overrides
  - readme: qualify the cpu_temp omission; #1794 was filed from a RAPL
    path, so the coupling is untested on gpu_metrics
  - readme: raise the Mesa floor to 24.1 and add a 0.8.2 MangoHud
    floor for gpu_metrics v3_0 parsing
  - readme: consolidate removals into one table with reasons, and add
    a lockstep table for the generator, validator and post-hook
  - readme: note mangohud arrives through cachyos-gaming-applications
    and that the file lands at mode 0600
  - readme: add a verify section diffing against the deployed file
  - readme: drop the version badge; a shields.io request can fail at
    render time and the changelog is the version of record
  - license: add LICENSE; the MIT grant was asserted in the README
    with no license text in the archive
  - conf: unchanged; parity re-checked against ry-install 7.141.0


1.17.0
------

  - conf: reorder both blocks temp-first; GPU and CPU each read load,
    temp, clock, power
  - conf: supersede the 1.16.0 load, power, clock, temp ordering
    shipped the same day
  - conf: move the commented cpu_temp stub to slot two, parallel to
    the GPU block
  - readme: resync the directive table, block order and example line


1.15.0
------

  - conf: add cpu_power for CPU package draw; 19 active directives
    plus one commented cpu_temp stub
  - conf: keep cpu_temp commented; the CPU package sensor was not
    reported on this box, so the directive would render nothing
  - readme: add cpu_power to the table, the CPU block and the example;
    state that only gpu_temp is shown and why cpu_temp stays off


1.14.0
------

  - conf: restore lockstep with the installer, re-adding toggle_hud,
    gpu_power, text_outline and a commented cpu_temp stub
  - conf: 1.13.0 dropped these but the installer still ships them; 18
    active directives plus one commented stub
  - readme: add gpu_power, text_outline and cpu_temp to the table, and
    restore power and temp in the example


1.13.0
------

  - conf: reconcile to the ry-install layout; FPS-first, 16 directives
  - conf: drop gpu_power, cpu_temp, fps_metrics, text_outline and
    toggle_hud, whose bind stayed on the MangoHud default
  - readme: resync to the new set


1.12.1
------

  - conf: strip inline comments; keep the one-line header
  - readme: fold per-directive notes into a single options section


1.12.0
------

  - conf: reorder the GPU block to load, clock, temp, power like CPU
  - conf: trim the header to one line; drop throttling_status and
    core_load


1.11.1
------

  - conf: drop section banners and blank lines; keep header and
    inline comments


1.11.0
------

  - conf: reorder the bar to GPU, CPU, memory, FPS for a horizontal
    layout


1.10.0
------

  - conf: add gpu_power, cpu_temp and fps_metrics for 1% and 0.1% lows
  - conf: drop gpu_mem_clock and swap; neither suits a UMA APU
  - readme: note vram shows only the BIOS carveout; ram is the figure
    to watch


1.0.0 - 1.9.0
-------------

  Initial development series, 2026-05-10 to 2026-06-15, newest first.

  - docs: bump the MangoHud floor from 0.8.3 to 0.8.4 for the Steam
    Overlay fix; document the MANGOHUD=1 auto-enable
  - docs: sync to MangoHud.conf as source of truth at 19 directives;
    drop the bundled font, the font_file step and the carve-out
  - docs: remove example.png for a text sketch of element order
  - 1.7.0 skipped; no release published, recorded so history has no
    gap
  - docs: add an example-output section and preview, name the
    horizontal directive, split GPU core and memory clocks
  - conf: remove gpu_junction_temp; hotspot mirrors the gpu_temp value
  - conf: remove frame_count, gpu_name, vulkan_driver, engine_version
  - conf: expand to a diagnostics set then trim back, covering
    fps_metrics, memory temps, core_load, resolution and swap
  - conf: remove throttling_status and throttling_status_graph
  - conf: switch to a horizontal top bar in the common gamer layout
  - conf: minimize from 87 to 17 directives; resync to MangoHud 0.8.3
    directive forms and move the README to GitHub style
  - conf: initial release; horizontal bar for Strix Halo on CachyOS
