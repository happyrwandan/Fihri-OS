# Fihri OS Lean 1.0

**Release date:** 2026-11-04
**Image:** `fihri-os-lean-1.0-amd64.iso` — 5,369,333,760 bytes (5.00 GiB)
**SHA-256:** `7551ed24ed06265b409944f48f9fed70dcf0f325c900ad49d3e3a69b52d73a1b`
**Base:** Ubuntu 24.04.5 LTS "noble" · Deepin DDE 6.x on X11 · Linux 6.8 · Calamares 3.3.5

Fihri OS **Lean** 1.0 is the same Fihri OS 1.0 desktop — same DDE experience, same office stack, same RDP server, same privacy defaults — retuned for the machines the full release priced out: **2 GB of RAM and 22 GB-class disks**. If your SSD is a 29–32 GB hand-me-down, the full 1.0 installer refused to proceed (32 GB floor, and its erase layout could not complete on those disks); Lean installs on it.

## Download & verify

The ISO exceeds GitHub's 2 GiB per-asset limit, so it ships as **three split parts** (same layout as the 1.0 release). Download all three parts plus the checksum files:

```
fihri-os-lean-1.0-amd64.iso.00.part   1,900,000,000 bytes
fihri-os-lean-1.0-amd64.iso.01.part   1,900,000,000 bytes
fihri-os-lean-1.0-amd64.iso.02.part   1,569,333,760 bytes
```

Reassemble and verify before writing to USB:

```bash
# 1) reassemble (order matters)
cat fihri-os-lean-1.0-amd64.iso.*.part > fihri-os-lean-1.0-amd64.iso

# 2) verify part integrity as downloaded
sha256sum -c SHA256SUMS.parts

# 3) verify the reassembled ISO
sha256sum fihri-os-lean-1.0-amd64.iso
# must print: 7551ed24ed06265b409944f48f9fed70dcf0f325c900ad49d3e3a69b52d73a1b  fihri-os-lean-1.0-amd64.iso

# or, with the signed checksums from the assets:
sha256sum -c SHA256SUMS

# 4) GPG verification of the checksums (key attached to this release):
gpg --import fihri-release-key-1.0.pub.asc
gpg --verify SHA256SUMS.asc SHA256SUMS   # Good signature from "Fihri OS Release <release@fihrios.org>"
```

Release signing key: same key as the 1.0 release — RSA 4096, sign-only, no expiry, fingerprint `4541 31F3 E510 0EEB B99F  E67F BF46 C856 82A1 6D3D`.

## Lean vs full 1.0

| | Fihri OS 1.0 | Fihri OS Lean 1.0 |
|---|---|---|
| **RAM floor (installer blocks below)** | 4 GB | **2 GB** |
| **Storage floor (installer blocks below)** | 32 GB | **22 GB** (64 GB SSD still recommended) |
| **RAM usage, live desktop** | needs ~2 GB to run the live session comfortably | **runs the live desktop in 2 GB** — verified in QEMU (BIOS + UEFI) with zero OOM kills and ~590–650 MB still available after the desktop settles |
| **Auto (erase-disk) partitioning** | erase layout cannot complete on ≤ ~31 GiB disks (root minimum 25 GB) | **enabled and fixed for small disks** — root minimum lowered to 16 GB so the guided layout completes; verified end-to-end in QEMU on a real 29.12 GB disk |
| **Swap default** | RAM-sized, hibernate-capable (user-adjustable) | **small swap** (≤ 10% of disk, capped) + **zram** (zram-config added), `vm.swappiness=180` |
| **ClamAV antivirus** | preinstalled, on-access scanning | **not in the base image — offered on demand from the Application Updates / Offers section** (one click, pkexec-gated; installs scanner + signature DB with **no resident daemon**, plus a dormant weekly on-demand scan timer) |
| **Background services** | Samba (smbd/nmbd), ModemManager, whoopsie/apport crash reporting | **masked off** (Samba, ModemManager, whoopsie, apport) |
| **Autostart apps** | full set | trimmed (Evolution alarm notifier, geoclue demo agent, print applet, Déjà Dup monitor, snap-userd hidden) |
| **Desktop effects** | full KWin animation set | **reduced for low-RAM machines** — 11 KWin animations off, instant feedback settings; dock translucency and the theme engine stay |
| **Dock extras** | screen recorder, wireless casting, screenshot plugins | removed (reclaimed RAM; the rest of the dock is unchanged) |
| **Boot polish** | — | GRUB timeout 2 s (BIOS + UEFI), quiet splash, no debug serial; 10 boot hogs disabled (NetworkManager-wait-online, fwupd, snapd seeding, gpu-manager, smartd, pollinate, casper-md5check, …); graphical.target reached at **~16 s** monotonic in the QEMU measurement |
| **ISO size** | 5,494,874,112 bytes (5.12 GiB) | **5,369,333,760 bytes (5.00 GiB)** — 120 MiB smaller |
| **Packages** | 2,920 | 2,914 (−7 ClamAV packages, + zram-config) |

Everything else is identical to 1.0: the full DDE desktop and theme engine, ElixirOffice + CryptPad, the RDP server and Remote Work wizard, Windows-style selectable updates with Driver Updates, Active Directory join, 7 staged locales, UFW on by default, no telemetry, no accounts, no ads.

## Why a 22 GB floor (measured, not guessed)

The Lean installed tree measures **13.34 GiB on ext4**. Budget: root payload 13.22 GiB + kernel slot, apt cache, journal and ext4-growth margin (~1.96 GiB) → root minimum 16 GiB; + EFI 0.29 GiB + /boot 1.0 GiB + home floor 2.0 GiB + small swap → **21.44 GiB, rounded to a 22 GiB floor**. The old 32 GB floor was inherited from full Fihri OS 1.0 (whose 13.5 GB payload plus a generous SSD-era pad chose 32) and stranded real 29–30 GB-class student SSDs. The full measurement basis ships inside the image at `/usr/local/share/canlinux/storage-floor-rationale.md`.

## Verification evidence

- **QEMU gates on the final r3 image (2026-11-04): all pass, zero failures** —
  - Live desktop at **2 GB RAM** under **BIOS and UEFI**: graphical target reached, desktop non-black, zero OOM kills, ≥ 590 MB available after settle.
  - **Storage floor on real disks**: a 29.12 GB disk passes the live sysreq check and Calamares requirements, the **auto partition page is offered and reached**; 20 GB and 21.9 GB targets are correctly **blocked**; the fixture suite (1G/2G RAM × 20/21.9/22/29.12 GB disk) passes in-image.
  - **Boot speed**: 2 s GRUB timeout, no wait-services, top post-boot process ≤ 20% CPU, no FAILED serial lines.
  - **USB-Ethernet dongle**: cold-plug connects (DHCP, default route, full connectivity, outbound HTTP 200), clean unplug, **hot-plug autoconnects within 90 s**, connectivity stays "full" even with check-URLs blocked.
- **Static ISO gates (r2, 2026-11-03): zero failures** — size, structure, 2,914-package manifest with a strict diff against full 1.0 (only the 7 ClamAV packages removed, zram-config added), masking/autostart/theme gates, secrets sweep, signed-hash seal.

## Install

1. Download + verify (above).
2. Write to USB (≥ 8 GB) with [balenaEtcher](https://etcher.balena.io/), [Rufus](https://rufus.ie/) (DD mode) or `dd if=fihri-os-lean-1.0-amd64.iso of=/dev/sdX bs=4M status=progress oflag=sync`.
3. Boot (UEFI recommended) — the live session is a full Fihri OS desktop and runs in 2 GB.
4. Launch **Install Fihri OS** (Calamares): the guided **auto (erase-disk)** layout is the default path; manual partitioning remains available.

## Which one should you install?

- **Lean** — machines with 2–4 GB RAM, or disks in the 22–64 GB range; you still get the whole desktop, and can add ClamAV later from the Updates window if you want it.
- **Full 1.0** — 4 GB+ RAM and 32 GB+ disk; on-access ClamAV, Samba sharing, full desktop effects and the hibernate-capable swap default out of the box.

## License

GPLv3 — individual components retain their upstream licenses.
