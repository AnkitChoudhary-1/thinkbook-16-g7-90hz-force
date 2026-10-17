# ThinkBook 16 G7 ARP: experimental refresh-rate profiles

**Unsupported display overclocking for one Lenovo LEN9156 internal panel. These files are not an official Lenovo upgrade and are not proven safe.**

## Disclaimer — read before using

Lenovo specifies **60 Hz** for the ThinkBook 16 G7 ARP's 1920 × 1200 display options. The included **70, 75, and 90 Hz modes exceed that rating**. Use them entirely at your own risk. **Keeping the factory 60 Hz setting is the recommended choice if avoiding hardware damage is your priority.**

These profiles and instructions are provided **AS IS, without any promise of compatibility, stability, performance, durability, or damage-free operation**. Possible problems include flickering, image corruption, frame skipping, blank screens, driver failures, or an unusable display configuration. The likelihood of permanent damage or reduced panel life has not been established for this panel. A normal-looking picture does not establish long-term safety.

The author and contributors cannot guarantee an outcome or accept responsibility for losses or damage caused by these experiments, to the extent permitted by applicable law. Damage caused by operation outside manufacturer specifications may not be covered by a warranty; check the terms that apply to your device. This project is not affiliated with or endorsed by Lenovo, AMD, Microsoft, or the CRU developer.

**Reinstalling Windows cannot repair physical panel or display-electronics damage.** Incorrect software settings can often be undone by removing the override. Actual hardware damage requires hardware repair or replacement. Windows' display-confirmation timeout is a configuration rollback, not protection against every possible consequence.

## Intended hardware and evidence

These files were prepared from an existing 60 Hz timing on one machine:

| Item | Observed value |
| --- | --- |
| Laptop | Lenovo ThinkBook 16 G7 ARP, machine type 21MW |
| Internal panel identifier | LEN9156 |
| Resolution | 1920 × 1200 |
| Graphics | AMD Radeon 660M |
| Driver observed during the experiment | 32.0.21030.11004 |
| Existing 60 Hz detailed timing | Approximately 60.01006 Hz |

**A laptop model name alone is not a compatibility check.** Machines sold under the same name can contain different panels. Even a matching panel identifier does not guarantee identical behavior across units or driver versions. Do not import these files into an unrelated panel, an external monitor, or a different ThinkBook variant.

Windows was observed reporting 70, 75, and 90 Hz on this one machine during a brief manual experiment. This was **not** an independent measurement of the panel's physical refresh rate. No frame-skipping photography, sustained stability testing, thermal characterization, or long-term lifetime testing was completed. Therefore these are experimental settings, not a list of validated safe refresh rates.

An earlier custom timing with different sync/blanking values flickered at 63–65 Hz. Factory-derived sync settings allowed subsequent experimentation, but that does not prove they remove overclocking risks. Modes above 90 Hz were reported missing from Windows; the reason was not established. **This is not evidence of a universal 90 Hz hardware limit.**

## Files

Each `.bin` is a 128-byte EDID base-block profile for importing into Custom Resolution Utility (CRU), not an executable or firmware installer.

| File | First/preferred detailed timing | Additional detailed timing | Additional-mode pixel clock |
| --- | --- | --- | --- |
| [70 Hz profile](profiles/70hz-native-sync-test.bin) | 1920 × 1200, ~60.01006 Hz | ~70.00030 Hz | 183.72 MHz |
| [75 Hz profile](profiles/75hz-native-sync-test.bin) | 1920 × 1200, ~60.01006 Hz | ~74.99924 Hz | 196.84 MHz |
| [90 Hz profile](profiles/90hz-native-sync-test.bin) | 1920 × 1200, ~60.01006 Hz | ~89.99985 Hz | 236.21 MHz |
| [60 Hz recovery profile](profiles/60hz-recovery.bin) | 1920 × 1200, ~60.01006 Hz | None | — |

The overclock profiles preserve the existing 60 Hz timing's totals, sync polarity, front porches, and sync widths; the additional detailed timing changes its pixel clock. Horizontal total is 2120 pixels and vertical total is 1238 lines. The original range descriptor remains 40–60 Hz vertically. **Adding a detailed timing outside that range does not establish panel support**, and a driver may omit or reject it.

The recovery file retains that same 60 Hz timing and removes the custom higher-rate detailed timing. It is **not a byte-for-byte factory EDID dump**. Back up your own original configuration before importing any file.

Checksums and decoded details are included in [SHA256SUMS.txt](SHA256SUMS.txt) and [profiles.json](profiles.json). A valid EDID checksum confirms only basic file integrity, not compatibility or safety. The distributed profiles contain no numeric unit serial or serial-number descriptor.

## Before any experiment

- Read the [official CRU documentation and recovery instructions](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU). Obtain CRU directly from its developer; no CRU executables are bundled here.
- Save your work and export your own current display configuration. Prepare Windows recovery/Safe Mode access and a fallback display if available.
- Confirm the active internal panel identifier and resolution. Stop if they do not match the intended hardware above.
- A selectable mode, a Windows refresh-rate readout, or an absence of flicker does not establish that every requested frame is being displayed.

## Importing a profile

1. Open CRU with administrator permission and select the correct active panel.
2. Import **one** profile. Confirm that the **60 Hz timing remains first**, followed by the chosen experimental rate.
3. Save with **OK**, then reload the graphics driver using CRU's restart utility.
4. In Windows **Settings → System → Display → Advanced display**, try the listed rate temporarily.
5. Revert immediately for flickering, lines, corruption, or blanking. If the mode is absent, stop rather than assuming it must be forced.

Do not assume the driver will preserve a recovery mode merely because it is in this file. Establish a recovery route before changing anything.

## Recovery

- If Windows remains usable, select **60 Hz**. Remove the affected display's override in CRU and reboot to restore its default configuration.
- The included 60 Hz recovery profile is an alternative configuration, not a substitute for removing an override or keeping your own backup.
- If CRU's restart utility leaves the screen blank, its documented **F8 recovery mode** temporarily unloads overrides. Follow the upstream recovery instructions.
- If necessary, use Windows Safe Mode. CRU's `reset-all.exe` removes **all displays' overrides**; reboot afterward. This also affects unrelated custom monitor settings.

If the screen remains abnormal after removing overrides and returning to its supported configuration, seek hardware diagnosis. Reinstalling the OS is not a repair for physical display damage.

## References

- [Lenovo ThinkBook 16 G7 ARP product specifications](https://psref.lenovo.com/syspool/Sys/PDF/ThinkBook/ThinkBook_16_G7_ARP/ThinkBook_16_G7_ARP_Spec.pdf)
- [CRU documentation and downloads — ToastyX](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU): CRU writes software EDID overrides; it does not modify the monitor's hardware.
- [Microsoft: change the refresh rate in Windows](https://support.microsoft.com/en-us/windows/hardware/display-graphics/change-the-refresh-rate-on-your-monitor-in-windows)
