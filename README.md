# NV-UV
**GPU Undervolt Companion Tool for NVIDIA RTX 30 (Ampere), RTX 40 (Ada Lovelace) and RTX 50 (Blackwell)**

NV-UV simplifies GPU undervolting by working alongside MSI Afterburner. It combines one-click presets, automatic game detection, crash recovery, a built-in DX12+DXR stress-test scanner and optional clock-efficiency control for games.

NV-UV is a **companion tool**, not a replacement for Afterburner. For overclocking, OSD, fan control and advanced curve editing, Afterburner remains the tool of choice.

**Platform:** Windows 10/11 (64-bit) only. NV-UV does not run on Linux or macOS.

> **New in v0.99.5:** Dynamic Clock Clapping (DCC), NVIDIA Auto-UV, NVIDIA Power Efficiency Mode, verified manual updates with rollback, optional private diagnostic reports and numerous setup, telemetry and recovery fixes.

## Discover NV-UV Play

**NV-UV Play** offers the same features as NV-UV, with a focus on ease of use, and works independently of MSI Afterburner. Easily create your own undervolting profiles, manage fan curves and assign profiles to your games. For monitoring, you can use RTSS alongside Play or choose Play's own overlay with all sensor readings available there.

Advanced users and experts can also explore **experimental expert settings** on supported GPUs, such as Crossbar (XBAR), SYS and video clocks, NVVDD/MSVDD voltage limits, Voltage Boost and more. Play is free and currently available as a prerelease.

[Download NV-UV Play](https://github.com/christianp403-spec/NV-UV-Play/releases)

More information: [Website (English)](https://christianp403-spec.github.io/NV-UV-Play/en.html)

## NV-UV v0.99.6.1

### Scanner and setup

- Afterburner setup now checks GPU usage monitoring for the assigned NVIDIA GPU, including systems where integrated graphics and an NVIDIA GPU are both present. GPU1/GPU2 refers to Afterburner's GPU and sensor assignment. **This change does not add SLI support.** It offers to enable missing sources, preserves existing selections and checks them again after applying. Previous setup confirmations are rechecked.
- The Voltage Step Scanner distinguishes missing readings, idle, insufficient test load and stale data. Voltage and GPU usage are shown separately. Missing readings alone do not block UV profiles; unverified scan points remain unavailable for use.
- Core and memory clock sources are recognized without a GPU prefix when the GPU assignment, units and readings match.

### Fixes

- Disabling UV-Pilot no longer requests a restore to 0 Hz when Smart Hz has not been used.
- Improved error-dialog handling and reconnection for the NVML telemetry process. The previous process must be confirmed as stopped before reconnecting.
- Power-limit restoration is only reported as successful after a successful driver call. A failed restore preserves the saved target and displays a warning.

### New restriction after a restore failure

**After a failed power-limit restore, further scans are blocked for the current session. Check or restore the power limit in Afterburner, then restart NV-UV before scanning again.**

### Game database and signing

- Game database 2.34 now contains **708 entries in total**, including Total War: WARHAMMER II with Balanced and corrected Space Marine 2 retail-process detection.
- The NV-UV executable is digitally signed with a **Certum code-signing certificate and an RFC 3161 timestamp**.
- VirusTotal reported **0/67 detections** for the signed release executable on **October 10, 2026**. [View scan report](https://www.virustotal.com/gui/file/4aa34838d2dda11f7efc566348754528bc29fdc3dee73cf82697d3ef7bf276d8).

Thank you all for helping NV-UV grow! Your support, feedback, bug reports and diagnostic data help me keep improving the program.

[Release notes and download](https://github.com/christianp403-spec/NV-UV/releases/tag/v0.99.6.1)

> **Not to be confused with** [doums/nvuv](https://github.com/doums/nvuv), a separate CLI tool for NVIDIA undervolting on Linux written in Zig. Different platform, different scope, different project.

---

## Download
The latest build is available as a ZIP under [Releases](https://github.com/christianp403-spec/NV-UV/releases).

---

## Requirements
- **OS:** Windows 10 22H2 (fully updated) or Windows 11, 64-bit
- **GPU:** NVIDIA RTX 5090 / 5080 / 5070 Ti / 5070 / 5060 Ti / 5060 (Blackwell)  
  or NVIDIA RTX 4090 / 4080 / 4070 Ti Super / 4070 Ti / 4070 Super / 4070 / 4060 Ti / 4060 (Ada Lovelace)  
  or NVIDIA RTX 3090 Ti / 3090 / 3080 Ti / 3080 / 3070 Ti / 3070 / 3060 Ti / 3060 (Ampere, new in v0.97, verified on RTX 3090 only, other models experimental)
- **Mobile / Laptop GPUs:** Not directly supported and not tested by me. The scanner may run, but there is no guarantee it works on mobile chips. You are welcome to try it at your own risk, but expect issues. Notebook support is on the ToDo.
- **Dependencies:** MSI Afterburner 4.6.6+ with RivaTuner Statistics Server (RTSS)
- **Driver:** Latest NVIDIA driver recommended

---

## Features
- **Voltage Lock**: one click, GPU runs at an exact voltage/frequency point
- **4 Presets**: Eco, Balanced, Performance, Max (community-validated per GPU)
- **OCS → UV Import**: import AB OC Scanner results, build a chip-specific UV curve
- **Voltage Step Scanner**: DX12+DXR stress engine with FMA math-error detection
- **Game Replay**: automatic frequency step-down on crash, with per-game learning loop
- **DCC: Dynamic Clock Clapping**: learns efficient clock caps for recognized games and remembers them per game and GPU
- **NVIDIA Power Efficiency Mode**: optional experimental global NVIDIA efficiency mode
- **NVIDIA Auto-UV**: previews an individual starting point and applies it only after confirmation
- **UV-Pilot**: recognizes 708 games, automatically switches to the selected UV preset
- **Smart Hz**: desktop 60 Hz, gaming native Hz (experimental)
- **Verified Updates**: manual installation, signed packages and rollback if installation fails
- **Local Diagnostics**: creates a local ZIP first; optional private sending only after review and consent
- **Mini View**, **DE/EN/RU/ES localization**, **System Tray**, **5 Skins**

---

## How It Works
NV-UV reads and writes MSI Afterburner profile files to apply voltage/frequency curves. Its central GPU-control path coordinates UV profiles and DCC so they do not write competing settings. Live telemetry is collected centrally from the available providers. The stress test runs as a separate process so a GPU crash will not take down the UI.

---

## Sensor monitoring

GPU Hotspot and individual VRAM temperatures are read through [PawnIO](https://github.com/namazso/PawnIO), independently of Afterburner's Hotspot source. Readings depend on your GPU and available sensors. Afterburner remains required for NV-UV's profile control and other monitoring functions.

The sensor dialog guides you through migration. Existing UV profiles are preserved. If PawnIO is missing, NV-UV downloads its official installer when you apply enabled sensor options; confirm the Windows administrator prompt. The PawnIO system driver is installed separately and is not bundled in the portable ZIP. Missing optional readings do not lock UV features. You can also keep or set up the optional Afterburner/RTSS Hotspot source and choose its OSD display in Afterburner.

## Troubleshooting
**NV-UV closes immediately on launch, no window, no log.** This is almost always an outdated Windows build that is missing required OS components (WinRT / COM API-set contracts). In the Windows Event Viewer it shows up as an APPCRASH with exception code 0xc0000602 in KERNELBASE.dll. Install all pending Windows updates (Windows 10 needs 22H2 fully patched) and NV-UV will start normally. The crash happens in the Windows loader before any of NV-UV's own code runs, which is why no log is written.

---

## Credits

Thank you to the projects and developers whose work supports NV-UV and NV-UV Play:

- **[Green Curve](https://github.com/aufkrawall/green-curve) by [aufkrawall](https://github.com/aufkrawall)** (MIT License), for the NVAPI V/F-curve access approach used by NV-UV's native bridge.
- **[PawnIO](https://github.com/namazso/PawnIO) by namazso and the PawnIO.Modules contributors**, for the hardware access used by NV-UV's Hotspot and VRAM readings.
- **[LibreHardwareMonitor](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor) and its contributors**, for CPU temperature monitoring in the standalone NV-UV Play application.

Special thanks to Unwinder for his longstanding work on MSI Afterburner and GPU tuning tools. Required third-party notices are included with the portable release.

---

## Community & Support
- **PCGH Forum:** [extreme.pcgameshardware.de/forums/nv-uv.3601](https://extreme.pcgameshardware.de/forums/nv-uv.3601/)
- **GitHub:** report bugs or ask questions via [Issues](https://github.com/christianp403-spec/NV-UV/issues)
- **Documentation:** [christianp403-spec.github.io/nv-uv-docs](https://christianp403-spec.github.io/nv-uv-docs/)

---

## Support the Project
NV-UV is free. If you find it useful, you can support development via [PayPal](https://www.paypal.com/paypalme/christianpapaioannou).

---

## License
NV-UV is closed-source software. See [LICENSE](LICENSE.txt) for terms.
Third-party components and attributions are documented in [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

---
**Build 22 · Cantor · Open Alpha**
