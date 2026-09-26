# ESPHome MIPI RGB

A versioned external copy of ESPHome's `mipi_rgb` component with additional
configuration for RGB panels that need larger DMA bounce buffers or control over
ESP-IDF's per-frame restart behavior.

This branch is based on ESPHome 2026.9.0. Production configurations should use
an immutable release tag rather than `main` or a maintenance branch.

## Usage

```yaml
external_components:
  - source:
      type: git
      url: https://github.com/aclight/esphome-mipi-rgb.git
      ref: esphome-2026.9.0-r1
      path: components
    components: [mipi_rgb]
    refresh: 0s
```

The external component shadows ESPHome's built-in `mipi_rgb` component. The
component tag must therefore be chosen for the ESPHome version used to compile
the configuration.

## Additional options

- `bounce_buffer_lines` controls the number of scanlines in each of ESP-IDF's
  two internal-SRAM bounce buffers. It defaults to ESPHome's stock value of 10
  and must divide the display height exactly.
- `force_restart` controls the upstream S3-only per-loop
  `esp_lcd_rgb_panel_restart()` call. It defaults to `true`, preserving upstream
  behavior. ESPHome 2026.9.0 does not make that call on P4 or S31.
- `desync_report_interval` enables optional VSYNC and frame-completion
  instrumentation. It defaults to `0s`, which disables the instrumentation.
- `late_frame_threshold` sets the interrupt-latency threshold used by that
  instrumentation.

See [`components/mipi_rgb/LICENSE`](components/mipi_rgb/LICENSE) for the precise
changes from upstream.

## Versioning

Maintenance branches are named for the compatible ESPHome release series, such
as `esphome-2026.7`. Immutable releases add the exact upstream version and a
fork revision, such as `esphome-2026.7.4-r1`. Consumers should always pin a
release tag so that upgrading one device does not change another device's build.

A component tag pins only this repository. It does not select the ESPHome
compiler version; keep the matching ESPHome build environment or known-good
firmware binary when rollback is required.

## Licensing

ESPHome is split-licensed. The Python component files are MIT licensed and the
C++ component files are GPL-3.0-or-later. SPDX headers identify each file's
license, and the complete upstream license text is included in
[`LICENSES/ESPHome-LICENSE.txt`](LICENSES/ESPHome-LICENSE.txt). The repository's
own documentation is covered by the root MIT license.
