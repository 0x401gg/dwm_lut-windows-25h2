# Changelog

All notable changes to this maintenance fork are documented here.

## [Unreleased]

### Added

- Release-build diagnostics written from inside `dwm.exe` to
  `%TEMP%\dwm_lut.log`.
- 25H2 logging for Present hook entry, back-buffer lookup, texture format,
  monitor coordinate lookup, and configured LUT matching.

### Known limitations

- User testing on a GeForce laptop with Windows 11 25H2 build 26200.8457
  confirmed that injection succeeds but the visible LUT effect does not apply.
  Treat the current 25H2 build as diagnostic-only until the DWM path is fixed.

## [4.1.0] - 2026-06-20

### Added

- Windows 11 25H2 support for the build 26200 servicing family.
- Updated DWM signatures for `COverlayContext::Present`,
  `IsCandidateDirectFlipCompatible`, and `OverlaysEnabled`.
- D3D11 back-buffer discovery through the 25H2 overlay swap-chain vtable.
- Updated per-monitor coordinate lookup for the 25H2 DWM object layout.
- A transparent, click-through compositor overlay to keep LUTs active when
  DirectFlip would otherwise bypass DWM.

### Changed

- Made the private DirectFlip and MPO helper hooks optional. A cumulative
  update changing an optional signature no longer prevents the DLL from
  initializing when the required Present hook is still available.
- Restored DWM's original `OverlayTestMode` value when unloading instead of
  assuming it was zero.
- Replaced broad future-version detection with exact Windows servicing-family
  checks. Unverified builds newer than 26200 are rejected safely.
- Updated the GUI and package version to 4.1.0.

### Safety

- Closing the window or choosing **Exit** now disables the LUT and unloads the
  DLL from `dwm.exe` first.
- If unloading fails, the close is cancelled and the GUI warns that the LUT may
  still be active.
- Minimizing to the notification area continues to keep the LUT active.

### Known limitations

- Runtime validation must be performed on the target Windows build and GPU.
- Windows 11 26H1/build 28000 is not supported because its private DWM layout
  has not been verified.
- Anti-cheat-protected games may block DWM injection or prohibit color filters.
- Disabling DirectFlip/MPO can affect VRR, G-Sync, latency, and GPU usage.

## Previous releases

Earlier release history is available from the
[lauralex maintenance fork](https://github.com/lauralex/dwm_lut/releases) and
the [original ledoge project](https://github.com/ledoge/dwm_lut/releases).

[Unreleased]: https://github.com/0x401gg/dwm_lut-windows-25h2/compare/v4.1.0...HEAD
[4.1.0]: https://github.com/0x401gg/dwm_lut-windows-25h2/releases/tag/v4.1.0
