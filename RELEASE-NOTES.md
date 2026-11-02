# Fihri OS 1.0 — Release Notes

**Release date:** 2026-11-02
**Image:** `fihri-os-1.0-amd64.iso` — 5,494,874,112 bytes (5.12 GiB)
**SHA-256:** `f2a70ebff3bf535fd7e52c1e75951fc9804616e43eeb20c4ccfa22ffd4954f07`
**Base:** Ubuntu 24.04.5 LTS "noble" · Deepin DDE 6.x on X11 · Linux 6.8
**Full report:** [docs/FIHRI-OS-1.0-REPORT.md](docs/FIHRI-OS-1.0-REPORT.md)

Fihri OS 1.0 is the first public release: a free, privacy-first,
Windows-familiar desktop for students and everyday work, measured and
validated end-to-end on real hardware (Acer Aspire A515-51G, i5-8250U,
12 GB RAM, Intel UHD 620 + GeForce MX150, 1920×1080).

> **About this build:** the published image is the **final 2026-11-02 rewrap
> (r4)** of the 2026-11-01 reseal. An earlier draft build (sealed 2026-10-29)
> was superseded: the desktop landed its final October fixes after that seal,
> so the image was rebuilt from the finished live system and re-verified
> (7-gate ISO verification, zero failures) before release. The r4 image was
> then **QEMU boot-verified end-to-end**: BIOS and UEFI boot both reach the
> live desktop, the desktop renders correctly, and a full Calamares install
> run completed with the swap ≥ RAM policy applied. Verify the SHA-256 above
> after downloading.

## What the reseal added (changes since the 2026-10-29 draft)

- **Theme engine, final state** — native XSETTINGS flow; light/dark flips
  apply live and write through to every application; tray and dock popup
  contrast fixes (network, Bluetooth, battery, eye-comfort, volume);
  dark-theme bridge/reassert services; `dde-appearance 1.1.33-1+canlinux3`.
- **Quick Actions panel** — restored right-to-left slide-in with the full
  widget set (weather, calendar, memory, world clock) and readable light
  and dark styling; `dde-tray-loader` / network plugin at `canlinux15`.
- **Icon fill** — the bloom icon set filled across dock, launcher, file
  manager and the login screen (101 icons touched, incl. macaron DCI
  variants).
- **Fresh-user wallpaper** — the VM-reference hero image
  (Colorful-Abstraction02) is the out-of-box default, with the full
  picker set behind it.
- **Desktop menu fix** — deduplicated right-click menu with a working
  "Set Wallpaper" action wired to Personalization.
- **Fihri OS Updates** — contrast-fixed toolbar and sectioned list:
  apt updates, **Application Updates** (CryptPad, ElixirOffice) and
  **Driver Updates** (ubuntu-drivers engine; consent-gated NVIDIA offer).
- **Settings search** — synonym-aware search (mouse / trackpad /
  pointer…); `dde-control-center 6.0.62-1+canlinux1`.
- **Fonts & locales** — Noto CJK preference fix; 7 fully staged locales
  (en, fr, es, de, ar, zh-Hans, zh-Hant) with Arabic shaping and CJK
  fonts in the image.
- Plus launcher/dock sort and title-band fixes, eye-comfort light pass,
  and refreshed first-login desktop entries. 2,920 packages in the
  shipped manifest.

## Highlights

- **Desktop experience** — Deepin DDE with the Fihri theme: translucent
  dock, windowed launcher with working search, Quick Actions slide-in
  panel (notifications + weather/calendar/memory/world-clock widgets),
  light/dark themes with accent colors, pixel-matched menus, smooth
  trackpad scrolling, live display scaling, DisplayLink dock support.
- **Office included** — ElixirOffice (DOCX/XLSX/PPTX/ODF/RTF/CSV) plus a
  self-hosted, end-to-end-encrypted **CryptPad** (loopback-only,
  on-demand, ribbon toolbars). No subscription, no cloud account.
- **Remote work, both directions** — an RDP **server on every install**
  (NLA + TLS, H.264 AVC420, clipboard, session handoff with lock,
  Windows 11 24H2 client compatible), and a Remote Work wizard for
  Citrix Workspace (preinstalled with the open ctxh264 plugin and
  zero-touch audio/webcam/Teams config), VMware Horizon, and native
  Windows RDP.
- **User-controlled updates** — a Windows-style selectable update list
  with details pane, a dedicated Driver Updates section
  (ubuntu-drivers engine, one-click Steam setup), and Ubuntu 24.04 LTS
  security maintenance to 2029 (ESM 2032).
- **Enterprise join** — Active Directory auto-detection and a guided
  realmd/sssd join wizard, with a domain field in the installer.
- **Privacy by default** — no telemetry, no crash uploads, no ads, no
  forced accounts; UFW firewall on by default; ClamAV on-access
  scanning; consent-gated Fihri Feedback with automatic secret redaction
  and zero network calls.
- **Consent-based drivers** — the ISO ships open drivers only; NVIDIA
  hardware gets a one-time consent offer for the tested proprietary
  stack (also reachable later from Updates → Driver Updates).
- **Installer** — Calamares 3.3.5: erase, replace, alongside-Windows or
  manual partitioning; 4 GB RAM / 32 GB storage floors (measured, not
  guessed); swap defaults to RAM size.

## System requirements (measured)

| Resource | Minimum | Preferred |
|---|---|---|
| RAM | 4 GB | 8 GB |
| Storage | 32 GB | 64 GB SSD |
| CPU | 64-bit x86-64 dual-core | 4+ cores |
| GPU | Mesa-supported Intel/AMD (llvmpipe fallback) | Hardware GL; NVIDIA via consent |
| Display | 1366×768 | 1920×1080 |

The full measurement basis (RAM/storage/CPU/GPU/display numbers, live
PSS scans, ISO content audit) is in the
[report §1](docs/FIHRI-OS-1.0-REPORT.md#1-system-requirements-measured-on-this-machine).

## Known limitations

- No full-disk-encryption (LUKS/BitLocker-class) install path yet — the
  top honest gap, candidate for 1.1.
- No native Microsoft Office apps (ElixirOffice/CryptPad cover formats).
- ClamAV is signature-based — no ML/reputation engine or SmartScreen.
- AD join yes; Entra ID / MDM (Intune) no.
- No biometric login, no AI assistant, no Snap-layouts tiling, no WSL.
- Flatpak/Flathub not preinstalled (installable post-install).

## Clean-profile guarantee (Citrix / VMware / RDP)

The shipped ISO was audited file-by-file: **no user credentials,
profiles, accounts, session files or gateway records ship**. Users add
their own accounts on first use; per-install identity (SSH host keys,
machine-id) is regenerated at first boot. The audit method and verdict
are documented in [report §3](docs/FIHRI-OS-1.0-REPORT.md#3-citrix--vmware-clean-profile-verification).

## QA state at release

- `canlinux-dde-selftest`: 210 pass / 0 fail / 4 skip (2026-10-31).
- Rebuilt ISO: 7-gate verification pass, zero failures (2026-11-01 —
  size, structure, 2,920-package manifest, fix-presence, secrets sweep,
  accounts, snaps/ClamAV/kernel).
- **QEMU boot test, final image (2026-11-02)**: boots under both BIOS
  (SeaBIOS) and UEFI (OVMF), live desktop renders, and a complete
  Calamares install run finished successfully with swap sized ≥ RAM.
- RDP: NLA + H.264 + Win11-24H2 bitmap-compat verified; capture
  heartbeat alive.
- GUI production runner: 32-app sweep 2026-11-01 — 24 apps pass/recovered,
  8 not-fixable in-runner (the documented known-issues class: RDP-owned
  session, vision caps — re-validated by the ISO gates on booted images);
  showcase set re-captured 2026-11-01 evening, with launcher, quick panel,
  CryptPad and Brave taken via direct launch-capture-close passes after
  the runner's strict browser-window checks.
- Broker audit chain: hash-chained, verified intact.

## License

GPLv3 — see [LICENSE](LICENSE). Individual components retain their
upstream licenses.
