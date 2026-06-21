# dwm_lut for current Windows 11 builds

Maintenance update of [ledoge/dwm_lut](https://github.com/ledoge/dwm_lut), based on
the lauralex 24H2 port and updated for the Windows 11 25H2 build 26200
servicing family.

The 25H2 path uses the current DWM `COverlayContext::Present` layout and obtains
the D3D11 back buffer through the overlay swap-chain vtable. DirectFlip/MPO helper
hooks are intentionally optional: a cumulative update that changes one of those
private functions no longer prevents LUT injection from initializing.

> [!CAUTION]
> This software injects a DLL into `dwm.exe` and hooks undocumented Windows
> internals. A future cumulative update can still break it. Disable it before a
> Windows feature update and keep a normal desktop recovery path available.

## Dependencies
- Visual C++ runtime (https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/)

# About
This tool applies 3D LUTs to the Windows desktop by hooking into DWM. It works in both SDR and HDR modes, and uses tetrahedral interpolation on the LUT data. In SDR, blue-noise dithering is applied to the output to reduce banding.

Compatibility targets:

- Windows 11 25H2, build 26200.x (including the June 2026 KB5094126 family)
- Windows 11 24H2, build 26100.x
- Earlier Windows 11 and the Windows 10 versions supported by the upstream fork

Windows 11 26H1 (build 28000) is deliberately rejected rather than being treated
as 25H2: its private DWM layout has not been verified.

The 25H2 signatures and offsets were tested by the contributing patch author on
build 26200 with two 2560x1440 displays. Source-level verification is possible on
other platforms, but runtime validation must be done on the target Windows build.

# Usage
Use DisplayCAL or similar to generate .cube LUT files of any size, run `DwmLutGUI.exe`, assign them to monitors and then click Apply. Note that LUTs cannot be applied to monitors that are in "Duplicate" mode.


For ColourSpace users with HT license level, 65^3 eeColor LUT .txt files are also supported.

HDR LUTs must use BT.2020 + SMPTE ST 2084 values as input and output.

Minimizing the GUI will make it disappear from the taskbar, and you can use the context menu of the tray icon to quickly apply or disable all LUTs. For automation, you can start the exe with any (sensible) combination of `-apply`,  `-disable`, `-minimize` and `-exit` as arguments.

Closing the window or choosing **Exit** disables the LUT and unloads the DLL from
DWM first. If unloading fails, shutdown is cancelled and a warning is shown.
Minimizing to the tray keeps the LUT active. Consequently, `-apply -exit` applies
and then immediately disables the LUT; use `-apply -minimize` for a persistent
automated session.

Note: DirectFlip and MPO get force disabled on monitors with an active LUT. These features are designed to improve performance for some windowed applications by allowing them to bypass DWM (and therefore also the LUT). This ensures that LUTs get applied properly to all applications (except exclusive fullscreen ones).

# Compiling
Install [vcpkg](https://vcpkg.io/en/getting-started.html) for C++ dependency management:

- Create and switch to your desired install folder (e.g. _%LOCALAPPDATA%\vcpkg_)
- `git clone https://github.com/Microsoft/vcpkg.git .`
- `.\bootstrap-vcpkg.bat`
- `vcpkg integrate install`

Just open the projects in Visual Studio and compile a x64 Release build.
