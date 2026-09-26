# ESPHome MIPI RGB

Versioned external copies of ESPHome components used by the alarm clock and
Memeboard control panel.

This branch is based on ESPHome 2026.9.0. Production configurations should use
an immutable release tag rather than `main` or a maintenance branch.

## Usage

```yaml
external_components:
  - source:
      type: git
      url: https://github.com/aclight/esphome-mipi-rgb.git
      ref: esphome-2026.9.0-r2
      path: components
    components: [mipi_rgb, runtime_image]
    refresh: 0s
```

The external components shadow ESPHome's built-in components. The component tag
must therefore be chosen for the ESPHome version used to compile the
configuration. Consumers that do not use runtime image pools may list only
`mipi_rgb`.

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

### `runtime_image`

ESPHome 2026.9 retains a decoder object after each runtime image finishes. That
is useful when one image is refreshed repeatedly, but a pool of many distinct
images accumulates one decoder per loaded image. This override restores the
2026.7 behavior of destroying the decoder after each success or error while
keeping the decoded pixel buffer cached.

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
C++ component files are GPL-3.0-or-later. Component-specific license notices
identify the modified files, and the complete upstream license text is included in
[`LICENSES/ESPHome-LICENSE.txt`](LICENSES/ESPHome-LICENSE.txt). The repository's
own documentation is covered by the root MIT license.
