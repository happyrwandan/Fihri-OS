# Fihri OS 1.0 — Students Desktop: Release Summary Report

**Release:** Fihri OS 1.0 (Students Desktop)
**Report date:** 2026-10-31
**Measurement host:** the Fihri OS 1.0 reference machine (Acer Aspire A515-51G,
Intel Core i5-8250U 4C/8T, 12 GB RAM, Intel UHD Graphics 620 + NVIDIA GeForce
MX150, 1920×1080 panel) — the same hardware the ISO was built and validated on.
**Data basis:** every number below is measured from this system (live session
PSS/RSS scans, ISO squashfs content audit, the recorded sysreq evidence of
2026-10-28, and the shipped ISO itself), except Windows 11 facts, which are
Microsoft's documented behavior and are marked as such.

## 0. Release identity

| Item | Value (measured) |
|---|---|
| OS name / version | Fihri OS 1.0 (`/etc/os-release`: `ID=ubuntu`, base Ubuntu 24.04.5 LTS "noble") |
| Desktop | Deepin DDE 6.x on X11 (deepin-kwin-x11, LightDM greeter) |
| Kernel | Linux 6.8.0-147-generic |
| ISO image | `fihri-os-1.0-amd64.iso`, 5,020,581,888 bytes (4.68 GiB) |
| ISO SHA-256 | `8414d41cff1eb8ac6d31b3a0214722f6b62c21354845a00e8ab5e6031463e9e3` (from `/srv/iso-work/SHA256SUMS`) |
| Compressed rootfs (squashfs) | 4,842,344,448 bytes (4.51 GiB); 2,901 packages in manifest |
| Uncompressed payload | ~12.0 GiB source tree → **13.5 GB raw installed tree** (clone `du -xs` = 13,788,024,832 B) |
| Installer | Calamares 3.3.5 (erase / replace / alongside-Windows / manual; swap defaults to RAM size) |
| Update channels shipped | Ubuntu noble + noble-updates + noble-backports + noble-security, UbuntuDDE stable PPA, Brave repo, Fihri repo |

---

## 1. System requirements (measured on this machine)

### 1.1 Summary table

| Resource | Minimum | Preferred | Measured basis |
|---|---|---|---|
| **RAM** | 4 GB | 8 GB | DDE core session ≈ 0.9–1.1 GB; full session with browser ≈ 2.1 GB (below) |
| **Storage** | 32 GB (installer blocks below) | 64 GB SSD | Installed payload 13.5 GB + swap/growth/user data |
| **CPU** | 64-bit x86-64, dual-core | 4+ cores (reference: i5-8250U 4C/8T) | Live session with apps open measured at ~24% of one core (≈3% of the 8-core reference CPU) |
| **GPU** | Any Mesa-supported GPU (Intel/AMD open drivers); software rendering fallback (llvmpipe) works | Intel/AMD hardware GL; NVIDIA proprietary optional | Intel UHD 620 validated; NVIDIA path via consent-based offer |
| **Display** | 1366×768 | 1920×1080 (validated); virtual screens up to 16384×16384 | eDP-1 1920×1080@60 native; multi-monitor supported |
| **Boot/Install media** | 8 GB USB | — | ISO is 4.68 GiB |
| **Network** | Optional at install; required for updates | Broadband for Citrix/Horizon/RDP use | internet check is advisory (`check:`, not `required:`) |

### 1.2 RAM — measurements

Live measurements on the reference machine (students desktop session, uid 1000):

* **DDE core session (idle, no apps): ≈ 0.9–1.1 GB.**
  * Recorded 2026-10-28 sysreq pass: DDE core subset RSS (Xorg/kwin/dde-\*/deepin daemons) = **892 MB**.
  * Live PSS re-measurement 2026-10-31 (sum of `/proc/*/smaps_rollup`, uid 1000): **1,074 MB** *including* open test apps (RSS estimates: terminal ≈ 96 MB, two file-manager windows ≈ 145 MB, Evolution alarm ≈ 50 MB, Citrix `icasessionmgr` ≈ 16 MB) → core ≈ 0.8–0.9 GB PSS.
  * Component breakdown (PSS, 2026-10-31): tray-plugin host (16 instances) 436 MB; dde-shell (dock+panel) 242 MB; deepin-kwin-x11 75 MB; dde-widgets 41 MB; dde-desktop 39 MB; dde-lock 29 MB; dde-osd 12 MB; startdde 10 MB.
* **Full session with a browser: ≈ 2.1 GB** (all-process RSS for the students user including Brave — sysreq evidence 2026-10-28).
* **System services outside the session** (root + system users): ≈ 230 MB PSS.
* **Free-memory behavior:** with 12 GB fitted and heavy parallel use, `MemAvailable` stayed ≥ 6.2 GB; swap usage 0 B when idle (swap-idle watch, 2026-10-13, read-only monitor).
* **Policy:** installer `requiredRam: 4` (GiB) **blocks** below 4 GB (verified Calamares welcome-module semantics from the shipped binary's strings); < 6 GB shows an advisory. Live-session floor is 2 GB (casper + DDE). 4 GB machines leave ~1.9 GB headroom for the installer; 8 GB gives comfortable multitasking (browser + office + remote session).

### 1.3 Storage — measurements

* Installed raw tree: **13.5 GB** (`du -xs` of the cloned rootfs: 13,788,024,832 B; breakdown `/usr` 8.6 G, `/opt` 3.6 G, `/var` 636 M, `/boot` 124 M).
* Squashfs compresses that 12.0 GiB source to 4.51 GiB (ratio 37.5%); ISO totals 4.68 GiB.
* **Policy:** installer `requiredStorage: 32` (GiB) **blocks** below 32 GB; 64 GB recommended (advisory) — SSD recommended so updates, Timeshift snapshots and mail profiles stay comfortable. The old UbuntuDDE default (20 GB) was deliberately raised after measuring the real payload (evidence: `canlinux-work/sysreq-20261028/`).
* Swap defaults to RAM size at install (hibernate-capable choice), user-adjustable down to none in the installer.

### 1.4 CPU

* Reference: Intel Core i5-8250U (4C/8T, 1.6–3.4 GHz). Measured 2026-10-31: the whole students session (desktop + open apps) summed to **~24% of one core** (≈3% of the 8-core machine), so any modern dual-core comfortably runs the desktop; 1080p H.264 video decode runs in-kernel/GPU, not CPU.
* Practical floor: any 64-bit x86-64 dual-core from the last decade runs the desktop; Citrix/H.264 remoting benefits from 4+ cores (the ctxh264.so decode path is FFmpeg-backed and CPU-capable, so GPU decode is **not** required for HDX).

### 1.5 GPU — generic vs NVIDIA path

* **Generic (ships by default):** open Mesa drivers. Validated on Intel UHD 620 (GL "gl2" compositor path; kwin scissor-on/blur-off policy tied to hardware GL). Any Intel/AMD GPU with a Mesa driver gives the full compositing path. A software-rendering (llvmpipe) fallback exists and was exercised during RDP performance work — the desktop remains usable without hardware GL, with reduced effects.
* **NVIDIA:** the ISO **ships the open driver stack only** — the proprietary `nvidia-*-580-server` packages are explicitly purged at build. At first graphical boot, `fihri-nvidia-offer` (shipped, enabled) detects NVIDIA hardware (PCI 10de:) and shows a **consent dialog**: "Install the tested proprietary driver (nvidia-580-server stack)?" — nothing installs silently; declining is remembered and never nags; the offer is also reachable later from **Fihri OS Updates → Driver Updates**. This mechanism was verified on this MX150 machine (the offer fired; the proprietary stack currently installed on the reference host was installed through exactly this consent path).

### 1.6 Display / resolution

* Native panel: 1920×1080@60 (validated). XRandR reports modes from 640×480 up; **maximum virtual screen 16384×16384**; multi-monitor supported.
* Remote desktop paths: RDP server mirrors the physical console at native resolution with multi-monitor support; the shipped wfclient.ini template pins Citrix to "span all local monitors by default" (per-session override in `.ica`/store settings). No `.rdp` files ship in the ISO (verified — the `.rdp` connection helpers on the build host are dev conveniences only).

---

## 2. Feature comparison — Fihri OS 1.0 vs Windows 11

Legend: ✅ matches or better · 🟡 partial / with caveats · ❌ not matched · ⚪ not applicable.

| Area | Fihri OS 1.0 | Windows 11 | Verdict |
|---|---|---|---|
| **Desktop / UX** | Deepin DDE: bottom dock with launchpad, Grand Search, notification center, Quick Actions sliding panel, dark/light themes, rounded corners | Taskbar + Start menu, widgets, Snap layouts | ✅ (own visual identity, verified against the Deepin reference VM; Snap-layout window tiling not equivalent) |
| **Theming** | Light/dark + 11 deepin themes, accent colors, live theme switching with dock/tray contrast bridges, custom wallpapers, lock-screen theming | Light/dark + accent, lock screen "Windows spotlight" | ✅ |
| **RDP server (remote into the machine)** | FreeRDP 3.31.1 shadow engine (`freerdp2-shadow` unit) on :3389, NLA + TLS, H.264 (AVC420) + Win11-24H2 bitmap-compat, clipboard, session handoff/lock, same password as local login (PAM→SAM auto-sync) | RDP host only on **Pro/Enterprise** — Home cannot be remoted into | ✅ **advantage** (all installs include it; E2E input/auth verified) |
| **RDP / remote client out** | "Remote Work" wizard: Citrix Workspace, VMware Horizon, Windows RDP (native FreeRDP with host/user/domain/password, own window) | mstsc + store clients | ✅ |
| **Citrix / VMware Horizon** | Citrix Workspace pre-installed with enhanced open stack (ctxh264 plugin, USB, audio/mic/webcam + Teams media config; .ica/.cr MIME wired); Horizon guided-setup slot, client auto-detected if present | Both available as vendor downloads | ✅ (clean-profile: users add their own accounts — see §3) |
| **Updates** | "Fihri OS Updates" GUI (checkable list, selective install, details pane, reboot banner, **Driver Updates** section) on Ubuntu 24.04 LTS: security support to 2029 (+ESM to 2032); apt back-end | Windows Update: monthly + driver push, active hours | ✅ for OS/app updates; 🟡 driver-update breadth (see Driver mgmt) |
| **Office suite** | **ElixirOffice** (local editors; DOCX/XLSX/PPTX/ODF/RTF/CSV) + **CryptPad** (self-hosted, end-to-end-encrypted collaboration) | Microsoft 365 native / web | 🟡 formats covered by ElixirOffice; no native MS Office app, no Outlook/OneDrive desktop integration — power-MS-Office users will notice |
| **Mail / Calendar** | Thunderbird (140 ESR line, snap) with built-in calendar; dde-calendar | New Outlook / Mail | ✅ |
| **Browser** | Brave 1.96 (default) + Chromium available | Edge | ✅ (no Edge sync/Edge-extension ecosystem) |
| **Feedback channel** | **Fihri Feedback**: local-first, consent-gated, 11-pattern secret redaction, **zero network calls** — user submits the bundle themselves | Feedback Hub (cloud, account-tied) | ✅ **advantage** on privacy |
| **Driver management** | Driver Updates in updater; consent-based NVIDIA offer (§1.5); fwupd 2.x firmware updates; kernel covers most hardware | Windows Update driver distribution + vendor ecosystems | 🟡 (no automatic vendor-driver back-catalog; NVIDIA/printer/firmware paths explicit) |
| **Domain join (AD)** | **fihri-domain-join** GUI wizard (realmd + sssd), detection→prompt→join→leave, polkit-gated helper, **domain field in the installer** handed to first-boot join | Settings → Access work or school (AD + Entra ID join) | ✅ AD join; ❌ no Entra ID/MDM (Intune) equivalent yet |
| **Security / AV** | ufw firewall **enabled by default**; ClamAV + **clamonacc on-access scanning** + freshclam (definitions refreshed at build); screen lock (dde-lock, opaque-lock verified); no telemetry | Microsoft Defender (real-time, cloud ML reputation), SmartScreen, Secure-core PC | 🟡 ClamAV lacks Defender's ML/reputation engine and browse-time SmartScreen; no Secure-Core/HVCI hardening story yet |
| **Privacy / telemetry** | No cloud account required, no ad ID, local feedback, self-hosted CryptPad, per-install identity regenerated at first boot | Mandatory diagnostics, ad ID, cloud account push | ✅ **advantage** |
| **Disk encryption** | ❌ No LUKS pre-configured (installer does not yet offer an encrypted-install path) | BitLocker device encryption (ubiquitous on modern hardware) | ❌ **not matched yet** — top honest gap |
| **Biometric login** | ❌ No Windows-Hello equivalent (password + lock screen only; reference hardware has no reader) | Windows Hello face/fingerprint/PIN | ❌ (⚪ where no reader exists) |
| **Backup / restore** | Timeshift daily snapshots + Déjà Dup file backup (target user-chosen) | Restore points + File History | ✅ |
| **System health** | smartd disk health, fwupd firmware, power-profiles-daemon (balanced/perf/battery) | Storage health, battery report, power modes | ✅ |
| **Remote assist** | RDP in/out (§ above); no bundled assist tool (RustDesk/Tailscale exist on the dev reference but are **not shipped** in the ISO) | Quick Assist | 🟡 (RDP covers most; no dedicated attended-assist app shipped) |
| **Media / codecs** | gstreamer-libav + H.264/H.265 decode paths (4 HEVC decoders incl. Intel QSV), deepin-movie/music/album, screen recorder, camera app | Built-in (HEVC paid extension) | ✅ |
| **Printing / Bluetooth** | CUPS 2.4.7 + system-config-printer; BlueZ + DDE tray | Native | ✅ |
| **App store / packages** | dde-store + apt (Ubuntu/UbuntuDDE universe); 2,901 packages preinstalled; flatpak **not shipped** (was on dev machine only) | Microsoft Store + winget | 🟡 (huge apt catalog; fewer consumer-brand store apps; flathub installable post-install) |
| **Accessibility** | Orca screen reader, onboard OSK, imwheel + DDE accessibility | Narrator, Magnifier, extensive A11y | 🟡 core equivalents present, less polish |
| **AI assistant** | ❌ none built in | Copilot | ❌ (deliberate scope) |
| **Cost / licensing** | **$0**, no license keys, no activation | $139 Home / $199 Pro per device | ✅ **advantage** |

### 2.1 What Windows 11 does that Fihri OS 1.0 does not match yet (honest list)

1. **BitLocker-class full-disk encryption** — no encrypted-install path yet (top gap).
2. **Microsoft Office native apps** — ElixirOffice/CryptPad cover formats and collaboration, but are not MS Office.
3. **Defender-grade real-time AV** (ML/reputation/SmartScreen) — ClamAV on-access is shipped, but it is signature-based.
4. **Entra ID join / MDM (Intune)** — AD-join is there; cloud-identity management is not.
5. **Windows Hello biometrics**, **Copilot**, **Snap-layouts tiling**, **Xbox/Game Bar/DirectStorage**, **Find-my-device with cloud account**, **WSL** — no shipped equivalents.
6. **Driver breadth via Windows Update** — Fihri relies on the kernel + explicit NVIDIA/firmware paths.

### 2.2 What Fihri OS 1.0 does that Windows 11 does not (advantages)

1. **RDP server on every install** (Windows 11 Home cannot host RDP).
2. **$0 license**, no activation, no forced cloud account.
3. **Telemetry-free by design**, local feedback with redaction, self-hosted E2E-encrypted CryptPad.
4. **Consent-based driver installs** (nothing silent), verified-rollbackable updates (apt + Timeshift + per-component rollback scripts).
5. **Open remote-work stack**: Citrix Workspace pre-integrated with an open H.264 plugin (ctxh264) and zero-touch certs/audio/webcam config.

---

## 3. Citrix / VMware clean-profile verification

**Question:** does the Fihri OS 1.0 Remote Work GUI + TLX stack ship with **any** user
credentials, profiles, accounts or session state baked in? **Answer: no — verified
clean, with two minor flags** (§3.4). Every claim below was checked against the
shipped ISO's squashfs, the live stack, and the GUI source.

### 3.1 What ships in the ISO (audited file-by-file)

| Shipped path | Content | Credential/profile state |
|---|---|---|
| `/opt/fihri-remote/` (stack) | binaries, docs, `versions.env` | Clean; ships only **templates** (see next rows) |
| `/opt/fihri-remote/share/templates/wfclient.ini` | audio/drive/USB/H.264/webcam defaults | **Clean** — no URLs, no accounts, no usernames; extracted from the ISO and verified line-by-line |
| `/opt/fihri-remote/share/templates/usb-policy.conf`, `x-ica.xml` | USB policy + MIME routing | Clean |
| `/usr/local/bin/fihri-remote` (+ helpers) | Remote Work GUI + backend tools | Clean; no embedded secrets (flag F2 below is cosmetic-only) |
| `/etc/icaclient/config/*` + `/opt/Citrix/ICAClient/config/*` (Citrix Workspace app is **preinstalled**) | vendor defaults, `usertemplate/`, `appsrv.template` | **Clean** — all `Username=`/`Password=`/`StoreFrontDiscoveryURL=` keys present but **empty** (vendor template shape); only URL present anywhere is Microsoft's extension-store URL inside a vendor browser-extension manifest |
| `/etc/skel/` | bashrc, qt-theme, icons, autostart `dde-first-run` | Clean; **no** remote-work state |
| `/home/**` | **absent entirely** — the image contains **no user home directories** | Clean by construction |
| `.ica`/`.cr` session files, `Appsrv.ini` (non-template), `LastConnectedGateway`, `cache/Stores`, any `config.json` | — | **Zero occurrences** in the whole image (verified by full squashfs listing; the only `config.json` hits are CryptPad node_modules) |
| `.eula_accepted` | — | **Not shipped** — every user gets the in-app Citrix EULA consent flow themselves |

**VMware Horizon:** no Horizon client package ships (only the standard
`xserver-xorg-video-vmware` X driver). The wizard's Horizon slot is a guided-setup
page that detects an installed client (`vmware-view` binaries / omnissa packages)
and otherwise helps the user add their own — exactly the clean-profile contract.

### 3.2 Where user state CAN live at runtime (and why none of it ships)

The GUI's entire persistence model (verified in `fihri-remote-gui.py`):

* `~/.config/fihri-remote/config.json` — mode 0600, dir 0700; stores **only**: account entries (`kind` = email/url, `value`, `store_url`, `label`, `saved` timestamp), Citrix EULA acceptance version/timestamp, and per-account setup progress (stage ids + timestamp). **No passwords are ever written** — the RDP password field is used solely to build the one-shot `xfreerdp` argv and is never saved anywhere.
* `~/.ICAClient/**` — vendor per-user state (store caches, gateway records, auth logs) is created by the Citrix client **only after the user signs in**; none exists in the image.
* `~/.local/share/fihri-remote/fihri-remote.log` + `~/.cache/fihri-remote/*.log` — runtime logs (0600, rotated); created on first run, none ship.
* Citrix credentials themselves live inside the vendor client's session store under the user's home after login — again per-user, never pre-seeded.

### 3.3 Build-pipeline guarantees (why residue cannot leak)

The ISO is built from a cleaned clone; the shipped build scripts were read directly:

* `clean-clone.sh`: **deletes every build/test user account with its home** (`userdel -r students rdpcheck clipaa ubuntu`) — this removes the dev machine's entire test profile including all Citrix session residue; removes `/root/.ICAClient`; removes RDP QA credentials (`/etc/freerdp3-shadow/sam`, `qa-creds`) and SSH host keys; truncates `/etc/machine-id`; clears root shell history; purges dev tools (tailscale, code, flatpak, **nvidia-580-server stack**, old kernels) and the AD-join cache.
* `clean-final.sh`: purges root's freerdp/pulse/ibus configs and caches — "freerdp shadow regenerates its cert at runtime; nothing of it ships".
* `fihri-firstboot.service` (shipped, enabled): regenerates SSH host keys + machine-id on first boot — **per-install identity, nothing baked**.
* RDP authentication by design carries no stored credentials: the NLA SAM syncs from the user's own Linux password at every login (PAM hook); on a fresh install the SAM is rebuilt on first login.
* Post-install verification of the shipped squashfs (this audit): **no `/home`, no Citrix session state, no Horizon state, no RDP `.rdp` files, no saved accounts** — the clean-profile contract holds.

### 3.4 Flags (must-fix / watch items)

* **F1 — dev-machine residue exists but does NOT ship (informational, do not leak):** the reference machine's `/home/students` (deleted at build) contains real test-session data: a saved account in `~/.config/fihri-remote/config.json` (a real user email — redacted here), Citrix store caches + `LastConnectedGateway` + AuthManager logs + subscribed-app launchers in `~/.ICAClient`, and LAN-IP `.rdp` files in `~/Public`. **Action:** none for the ISO (guarantee §3.3); but *GitHub evidence packs must exclude this home and any screenshot that shows the Citrix storefront email/apps* (e.g., remote-work evidence must use the generic/fixture runs listed in §5).
* **F2 — cosmetic: internal test-target domain in a shipped code comment.** `/usr/local/bin/fihri-remote` (and the source `fihri-remote-gui.py` line 299) contains a comment mentioning a government storefront domain (`tc.gc.ca`) as a "live incident" example in the gateway-discovery explanation. It is a public gateway domain, **not a credential**; still, ship a genericized example in the next ISO spin.
* **F3 — cosmetic: `/opt/fihri-remote/.backup/` trees ship** (six timestamped internal tool backups, ~old versions of the same clean files). No secrets, pure size/craft polish — trim in the next spin.
* **Not a flag:** `/etc/icaclient/config`'s empty `Username=/Password=` keys are the vendor's own template format (all values blank) — nothing to purge.

**Verdict: Citrix/VMware compatibility is clean-profile in `fihri-os-1.0-amd64.iso` — no user credentials, profiles, accounts, session files or gateway records ship; users add their own accounts on first use.** Nothing blocks the ISO on this axis; F2/F3 are cosmetic cleanups for the next build.

---

## 4. QA / verification state at report time (reference machine)

* `canlinux-dde-selftest`: **210 pass / 0 fail / 4 skip** (skips: RDP keyboard-sync needs a live client toggle; adjoin journal scans need a join to have run; run 2026-10-31). Gate: 2 consecutive passes of the required 3 — keep sweeping before public claim.
* RDP: `freerdp2-shadow` listening on :3389 (dual-stack), NLA + H.264 AVC420 + Win11-24H2 bitmap-compat flags verified by the selftest; capture-heartbeat alive.
* Services enabled in the ISO: `ufw`, `clamav-daemon` + `clamonacc` + `freshclam`, `freerdp2-shadow`, `fihri-nvidia-offer`, `fihri-firstboot`, Fihri watchdog/heal units.
* GUI production runner: latest single-app runs **PASS** (2026-10-30: deepin-calculator, canlinux-updater); sweep history: 10-app sweep **9/10 PASS** (2026-10-10), later batch **8 pass / 2 deferred** (2026-10-19 — deferred by instance-occupancy guard, not failures).
* Broker audit chain: hash-chained, verified intact (per §120 record).

---

## 5. Screenshot & evidence references

Existing evidence, all on the reference machine. The items listed below were checked
to be credential-free (fixture or generic-account runs). **Per flag F1: any
other remote-work run that shows the real storefront account/email must not be
published.**

| Topic | Evidence location | GitHub status |
|---|---|---|
| Full desktop, dark + light (post-rebrand) | `~/FINAL.png`, `~/final_light.png`, `~/e2e_dark.png`, `~/e2e_light.png` (2026-10-22) | Usable; **refresh recommended** — several dock/theme fixes landed Oct 24–31 (see below) |
| Dock / panel / quick panel detail | `~/dock_*.png`, `~/qp*.png`, `~/qs*.png`, `~/panel_*.png`, `~/tray*.png` (2026-10-22) | Usable; refresh with the same script after the Oct 31 popup-QSS work |
| Latest labeled full-frame acceptance bundle | `canlinux-desktop-loop/var/acceptance/acc-20261031-202541-5545/` (manifest + frame + thumbnail) + fresh capture `canlinux-desktop-loop/var/captures/cap-20261031-215419-ec0a.png` (2026-10-31, 1920×1080, brightness ok) | **Fresh — use these as the GitHub hero shots** |
| Theme/icon parity (VM comparison) | `~/theme-evidence/`, `~/fm-icon-evidence/`, `canlinux-work/vm-theme-baseline-*`, `canlinux-work/darkparity-20261030/` | Usable |
| Remote Work GUI wizard (generic/fixture run — safe) | `fihri-remote/docs/evidence/remote-work-gui-20261022/`, `remote-work-gui-singleprompt-20261022/` (mock-pkexec single-prompt proof), `zero-touch-2026-10-22/` | Usable; **never** include runs that show a real storefront account |
| Boot / installer visuals | `~/plymouth_logo.png`, `~/initrd_logo.png`, installer evidence `canlinux-work/installer-*-20261029/`, `fihri-installer/` | Usable |
| NVIDIA driver offer | `canlinux-work/nvidia-offer-20261026/` | Usable |
| Screen-lock privacy proof | `ProjectProgressReport_deepin.md` §32.x (before/after RMSE evidence) + `~/locktest-*.png` (2026-09-25) | Usable |

**Needs refresh before GitHub:** hero desktop screenshots (light+dark) re-taken
after the 2026-10-31 quick-panel/popup-QSS and wallpaper work (the Oct 22 set
predates it); an "Fihri OS Updates" window shot with the Driver Updates section;
an installer (Calamares) end-to-end set from the shipped ISO; and — post-install,
on a **clean test account** — one Remote Work wizard walkthrough to completion.

---

## 6. Known limitations / follow-ups

1. Selftest consumer gate needs a 3rd consecutive pass (2/3 now).
2. F2/F3 cosmetic ISO cleanups (§3.4) for the next spin.
3. Encrypted-install (LUKS) path — the biggest honest gap vs Windows 11; candidate for 1.1.
4. RDP keyboard-sync selftest flip needs one live client CapsLock toggle to auto-PASS.
5. No GitHub push / no ISO rebuild performed here (out of scope by instruction).

---

*Prepared from live system measurements and shipped-ISO audit, 2026-10-31. All
sizes in decimal GB/GiB as labeled; all personal/internal identifiers redacted.*
