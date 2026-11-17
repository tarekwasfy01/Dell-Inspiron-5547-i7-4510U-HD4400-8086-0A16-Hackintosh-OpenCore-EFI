# Dell Inspiron 5547 — i7-4510U / Intel HD 4400 — OpenCore EFI

OpenCore 1.0.9 configuration for a Dell Inspiron 15 5547 targeting macOS Tahoe 26. Includes the complete EFI and all kext bundles from the working Recovery snapshot. Based on a NullMoth 1401-generated configuration, with native Intel Wi-Fi and a keyboard-only PS/2 test.

**Status: owner confirms working graphics acceleration, Bluetooth, function keys, and shutdown.** This is a work in progress, not a fully tested installation guide. Last updated: 17 November 2026.

## Hardware

| Component | Hardware |
| --- | --- |
| Laptop | Dell Inspiron 15 5547; BIOS A13 |
| CPU | Intel Core i7-4510U, Haswell-ULT |
| Integrated GPU | Intel HD Graphics 4400, `8086:0A16` |
| Discrete GPU | AMD Radeon R7 M260, `1002:6900`; disabled |
| Display | Internal 1366 × 768, connected to Intel iGPU |
| Wi-Fi | Intel Dual Band Wireless-AC 3160, `8086:08B3` |
| Ethernet | Realtek PCIe FE controller, `10EC:8136` |
| Audio | Realtek ALC255, `10EC:0255` |
| Keyboard | PS/2, `DLLK063E`, ACPI `\_SB.PCI0.LPCB.PS2K` |
| Touchpad | Synaptics I2C HID, `DLL063E` |
| Bluetooth | Intel USB, `8087:07DC` |
| Card reader | Realtek USB, `0BDA:0129` |
| SMBIOS model | MacBookPro11,1; personal identifiers removed |

## What works and what does not

| Feature | Status | Evidence / limitation |
| --- | --- | --- |
| OpenCore and Tahoe Recovery | ✅ Confirmed | Owner successfully reached Recovery |
| Intel Wi-Fi | ✅ Confirmed in Recovery | Owner confirmed working after AirportItlwm Tahoe change; alpha driver |
| Battery reporting | ✅ Confirmed in Recovery | Owner confirmed working; battery runtime/sleep not tested |
| Touchpad | ✅ Confirmed in Recovery | Owner confirmed working; every gesture not verified |
| Internal keyboard | ✅ Confirmed | Owner confirmed working after `ps2kbdonly=1`; PS/2 mouse/trackpad plugins disabled |
| Intel graphics display output | ✅ Recovery displays | This does not establish acceleration |
| GPU acceleration / Metal | ✅ Confirmed by owner | Intel HD 4400 acceleration is active; exact Metal feature set not independently measured |
| AMD discrete GPU | ❌ Disabled | Not used for display or acceleration |
| Ethernet | ✅ Confirmed working | Owner confirmed Ethernet works with RealtekRTL8100 2.0.1 |
| Audio and microphone | ✅ Confirmed by owner | Owner reports working audio output and microphone |
| Bluetooth | ✅ Confirmed by owner | Owner reports Bluetooth working |
| Brightness, webcam, card reader, HDMI | ⏳ Not tested | Inclusion of a driver does not prove functionality |
| Sleep / wake / all USB ports | ⏳ Not tested | UTBDefault is a temporary map, not a custom port map |
| Full Tahoe installation | ✅ Owner reached installed macOS | GPU root patching and shutdown issue remain open |
| Shutdown | ✅ Confirmed by owner | No shutdown panic now |
| Ubuntu in OpenCore | ⏳ Configured; boot test pending | OpenLinuxBoot and ext4 driver enabled |

## Configuration notes

- Intel HD 4400 uses `device-id = 12040000` (HD 4600) and `AAPL,ig-platform-id = 0600260A`, with the Haswell cursor-memory patch. Do not interpret a visible desktop or reported VRAM as proof of Metal acceleration.
- AirportItlwm is the laobamac Tahoe 2.4.0 alpha build `7204c69`, restricted to Darwin 25. The included `itlwm.kext` is disabled; do not enable both drivers together.
- VoodooPS2Controller contains an additional `DLLK063E` device match. `ps2kbdonly=1` and disabled PS/2 pointing-device plugins were confirmed by the owner to restore the internal keyboard. VoodooI2C, VoodooI2CHID and the single active VoodooInput instance remain enabled for the working touchpad.
- Bluetooth, battery, sensors, brightness and card-reader drivers remain from the original snapshot. They are not all confirmed to function.
- All current kext bundles are included. See [the version and enabled-state inventory](docs/components.json). Do not enable every bundled plugin indiscriminately.

## Before using this EFI

1. Back up your own working EFI. Use a separate test device where possible.
2. Generate your own valid SMBIOS identifiers for the selected model. `SystemSerialNumber`, `MLB` and `SystemUUID` are intentionally empty; ROM is six zero bytes. These placeholders are not a usable identity.
3. Check that your hardware matches. Another Inspiron 5547 can have different Wi-Fi, display and input devices.
4. Validate with `ocvalidate` from OpenCore 1.0.9. The sanitized configuration passes its structural validation; the additional 1401 check intentionally reports the three empty personal fields. See [validation details](docs/validation.json).
5. Obtain macOS from Apple separately. No Recovery image, installer package, Apple system drivers or OCLP payloads are distributed here.

For a complete local installation package, the owner is preparing Apple's InstallAssistant.pkg alongside [UnPlugged](https://github.com/corpnewt/UnPlugged). This is distinct from a bootable installer made with Apple's `createinstallmedia`. This repository does not promise that Apple's installer will never make an online request.

Tahoe graphics root patches and other post-install work must match the installed OS build. OpenCore validation checks structure, not hardware functionality.

## Privacy

Only a sanitized copy is prepared for publication. Serial number, MLB, SystemUUID and ROM are cleared; itlwm WiFiConfig is empty. Personal build profiles, logs, ACPI hardware dumps, recovery files and full installers are excluded. The local active EFI is not sanitized or overwritten by this publication step.

## Credits

- NullMoth 1401: initial build tool; see the distribution used by the owner.
- [Acidanthera OpenCorePkg](https://github.com/acidanthera/OpenCorePkg), Lilu, WhateverGreen, VirtualSMC and VoodooPS2.
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/): configuration and troubleshooting guidance.
- [laobamac/itlwm](https://github.com/laobamac/itlwm): native Tahoe Intel Wi-Fi build.
- [cuong95nddev/Dell-5547-Hackintosh-OpenCore](https://github.com/cuong95nddev/Dell-5547-Hackintosh-OpenCore): Catalina/Big Sur reference; not copied wholesale.
- [sokdiff/Inspiron-15-5547-hackintosh](https://github.com/sokdiff/Inspiron-15-5547-hackintosh): older Clover/ApplePS2 reference; its legacy driver is not enabled in this snapshot.

See [third-party component notes](THIRD_PARTY.md). Bundled components retain their upstream licenses; this repository does not relicense them.

## Function-key mapping

| Key | Action | Status |
| --- | --- | --- |
| F1 | Mute | ✅ Confirmed by owner |
| F2 | Volume down | ✅ Confirmed by owner |
| F3 | Volume up | ✅ Confirmed by owner |
| F11 | Brightness down | ✅ Confirmed by owner |
| F12 | Brightness up | ✅ Confirmed by owner |

VoodooPS2Keyboard Custom ADB Map handles standard F-key and Dell multimedia scan codes. The owner confirms the requested mute, volume, and brightness keys work. The original keyboard-only controller setup is preserved.
