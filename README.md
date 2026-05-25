# Hackintosh HP ProDesk 600 G3

Dieses Repository enthält Anleitung und Konfiguration für macOS auf einem **HP ProDesk 600 G3** (Mini).

## ⚠️ Wichtiger Hinweis
Diese Anleitung dient nur zu Lern- und Forschungszwecken. Stelle sicher, dass du eine legitime Kopie von macOS besitzt. Die finale Seriennummer **muss von dir selbst generiert** werden – sie ist nicht in dieser Config enthalten.

---

## 🖥️ Getestete Hardware

| Komponente          | Modell / Hinweis                                  |
|---------------------|---------------------------------------------------|
| CPU                 | Intel Core i5-6500t / i5-7500t (Kaby Lake / Sky Lake) |
| GPU (iGPU)          | Intel HD Graphics 530 (Skylake) / 630 (Kaby Lake) |
| Arbeitsspeicher     | DDR4  64 GB, 2400 MHz                             |
| LAN                 | Intel I219-LM                                     |
| Audio               | Realtek ALC221 (layout-id 28                      |
| NVMe/SSD            | beliebig (keine Samsung PM981 oder SK Hynics)     |
| USB                 | Intel Serie 100/C230                              |

---

## Benötigte Tools

- [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases) (empfohlen 0.9.5 oder neuer)
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) (zur Generierung der Seriennummer)
- [ProperTree](https://github.com/corpnewt/ProperTree) (zum Bearbeiten der config.plist)
- [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools) (zum Bearbeiten der config.plist)
- macOS Installer (z. B. Monterey, Ventura, Sonoma, Tahoe)

---

## Seriennummer generieren (PFLEICHT)

**Führe diesen Schritt vor der ersten Nutzung aus!**

1. Lade **GenSMBIOS** herunter und führe es aus.
2. Wähle Option `3. Generate SMBIOS`.
3. Gib als Modell eines der folgenden ein:
   - `Macmini8,1` (empfohlen für iGPU-only)
4. Kopiere die generierten Werte:
   - Serial
   - Board Serial (MLB)
   - SmUUID
   - ROM (MAC-Adresse)
5. Öffne die `config.plist` mit ProperTree und trage sie unter `PlatformInfo > Generic` ein.
6. **Prüfe die Seriennummer auf checkcoverage.apple.com** – sie darf nicht gültig sein.

---

## BIOS-Einstellungen (HP ProDesk 600 G3)

| Einstellung                     | Wert                          |
|--------------------------------|-------------------------------|
| Secure Boot                    | Deaktiviert                   |
| Legacy Support                 | Deaktiviert (nur UEFI)        |
| VT-d                           | Deaktiviert (kann später an)  |
| CFG Lock                       | Deaktiviert (per GRUB/Mod)    |
| Fast Boot                      | Deaktiviert                   |
| USB XHCI Hand-off              | Aktiviert                     |
| SATA Mode                      | AHCI                          |
| Boot Order                     | UEFI USB/DVD zuerst           |

> Hinweis: CFG Lock kann mit `ControlMsrE2.efi` oder einem modifizierten BIOS umgangen werden.



```text
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC.aml
    │   ├── SSDT-EC.aml
    │   ├── SSDT-MCHC.aml
    │   ├── SSDT-PLUG.aml
    │   ├── SSDT-BUS.aml
    │   └── SSDT-USBX.aml
    │
    ├── Drivers
    │   ├── HfsPlus.efi
    │   ├── OpenCanopy.efi
    │   ├── OpenRuntime.efi
    │   └── ResetNvramEntry.efi
    │
    ├── Kexts
    │   ├── AMFIPass.kext
    │   ├── AppleALC.kext
    │   ├── BlueToolFixup.kext
    │   ├── IntelBluetoothFirmware.kext
    │   ├── IntelBTPatcher.kext
    │   ├── IntelMausiEthernet.kext
    │   ├── itlwm.kext
    │   ├── Lilu.kext
    │   ├── NVMeFix.kext
    │   ├── RestrictEvents.kext
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext
    │   ├── USBMapDummy.kext
    │   ├── USBToolBox.kext
    │   ├── UTBMap.kext
    │   ├── VirtualSMC.kext
    │   └── WhateverGreen.kext
    │
    ├── Tools
    │
    └── config.plist ← HIER die generierte Serial eintragen!
```

## Installation

1. Erstelle einen bootfähigen USB-Stick mit macOS (z. B. mit `createinstallmedia`).
2. Kopiere die EFI-Ordnerstruktur auf die EFI-Partition des USB-Sticks.
3. Passe `config.plist` an (SMBIOS mit deiner eigenen Seriennummer).
4. Boote von USB, starte die macOS-Installation.
5. Nach der Installation kopiere die EFI auf die interne Festplatte.

---

## Bekannte Probleme & Lösungen

| Problem                                | Lösung                                                                 |
|----------------------------------------|------------------------------------------------------------------------|
| Intel Wifi 8265                        | Läuft mit Heliport                                                     |

---

## Getestete macOS-Versionen

| Version         | Status                     |
|----------------|----------------------------|
| Monterey 12.7  | ✅ Voll funktionsfähig     |
| Ventura 13.6   | ✅ stabil                  |
| Sonoma 14.4+   | ⚠️ Patches für WLAN nötig  |
| Tahoe 26.5   | ⚠️ Patches für WLAN nötig  |

---

## Lizenz

Dieses Projekt ist unter der MIT-Lizenz lizenziert – jedoch ohne Gewähr. Die generierte Seriennummer ist nicht Teil dieser Konfiguration und muss vom Benutzer selbst erstellt werden.

---

## Nützliche Links

- [OpenCore Install Guide (Dortania)](https://dortania.github.io/OpenCore-Install-Guide/)
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- [HP ProDesk 600 G3 Hackintosh Thread (insanelymac)](https://www.insanelymac.com)

---

**Letzter Hinweis:** Ersetze die Platzhalter für `SystemSerialNumber`, `MLB`, `SystemUUID` in deiner `config.plist` mit den selbst generierten Werten. Ohne eigene Seriennummer wird iMessage/FaceTime nicht funktionieren.
