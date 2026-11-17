# Third-party components

This repository bundles binary EFI and kext components from the owner's 1401-generated snapshot. Their upstream licenses, copyright notices and attribution requirements remain applicable. No blanket license is applied to third-party binaries. Review upstream licenses before redistributing modified copies.

| Components | Upstream |
| --- | --- |
| OpenCore, OpenRuntime, OpenCanopy, ResetNvramEntry | https://github.com/acidanthera/OpenCorePkg |
| HfsPlus EFI driver / boot resources | https://github.com/acidanthera/OcBinaryData |
| Lilu | https://github.com/acidanthera/Lilu |
| WhateverGreen | https://github.com/acidanthera/WhateverGreen |
| VirtualSMC and SMC sensor/battery plugins | https://github.com/acidanthera/VirtualSMC |
| VoodooPS2Controller and plugins | https://github.com/acidanthera/VoodooPS2 |
| VoodooI2C, VoodooI2CHID and VoodooInput | https://github.com/VoodooI2C/VoodooI2C |
| AirportItlwm Tahoe alpha | https://github.com/laobamac/itlwm |
| itlwm, disabled alternative | https://github.com/OpenIntelWireless/itlwm |
| IntelBluetoothFirmware / IntelBTPatcher | https://github.com/OpenIntelWireless/IntelBluetoothFirmware |
| BlueToolFixup | https://github.com/acidanthera/BrcmPatchRAM |
| RealtekRTL8100 | https://github.com/Mieze/RealtekRTL8100 |
| RealtekCardReader | https://github.com/0xFireWolf/RealtekCardReader and https://github.com/Baio1977/RealtekCardReader |
| USBToolBox / UTBDefault | https://github.com/USBToolBox/kext |
| RestrictEvents | https://github.com/acidanthera/RestrictEvents |
| BrightnessKeys | https://github.com/acidanthera/BrightnessKeys |
| ECEnabler | https://github.com/1Revenger1/ECEnabler |
| AMFIPass | https://github.com/dortania/OpenCore-Legacy-Patcher |

Version numbers and enabled states: [docs/components.json](docs/components.json).

Modifications to this snapshot: controller Info.plist adds the Dell `DLLK063E` identifier; public plist files remove personal SMBIOS and Wi-Fi configuration; OpenCore config selects native Tahoe Wi-Fi and a keyboard-only PS/2 test. Existing per-component notices are preserved. Apple Recovery, InstallAssistant.pkg and Apple graphics-system payloads are not included.
