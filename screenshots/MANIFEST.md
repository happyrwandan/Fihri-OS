# Fihri OS 1.0 Screenshots

**Showcase set refreshed on 2026-11-01** on the Fihri OS 1.0 reference
machine (1920×1080, square.light theme) after the October dock/popup/theme
fixes; remaining gallery shots are from the 2026-10-21 set.

All windows were launched through the system's normal application-launch
paths (Deepin Application Manager D-Bus launches, the launcher's own
`org.deepin.dde.Launcher1` / `org.deepin.dde.Widgets1` interfaces) and
closed again after capture; no input was injected into any application.
Window shots are cropped from full-frame captures of the real display.

## Refreshed 2026-11-01 (showcase set)

| File | Application | Content |
|------|-------------|---------|
| `fihri-os-1.0-desktop.png` | Desktop | Full 1920×1080 frame: themed wallpaper, dock, tray |
| `fihri-os-1.0-launchpad.png` | Launcher | Windowed launcher: app grid with search and categories |
| `fihri-os-1.0-files.png` | File Manager | Computer view: My Directories, Disks, Vault, quick-access sidebar |
| `fihri-os-1.0-updates.png` | Fihri OS Updates | Selectable update list with details pane ("Fihri OS Updates" window) |
| `fihri-os-1.0-elixiroffice.png` | ElixirOffice | Start page (Home / New / Open with templates) |
| `fihri-os-1.0-cryptpad.png` | CryptPad | Home page with document-type grid ("Fihri OS CryptPad" app window) |
| `fihri-os-1.0-feedback.png` | Fihri Feedback | Feedback form: consent checkboxes, local bundle preview |
| `fihri-os-1.0-remote.png` | Remote Work | First-run wizard (clean profile: Citrix Workspace / VMware Horizon / RDP) |
| `fihri-os-1.0-quickpanel.png` | Quick Actions panel | Slide-in panel: Notification Center, weather / calendar / memory / world-clock widgets |

Notes on the 2026-11-01 set:

- `fihri-os-1.0-remote.png` was captured against a **clean profile**
  (temporary `XDG_CONFIG_HOME`), i.e. exactly what a new user sees on
  first run — no accounts, gateways or credentials are in frame.
- `fihri-os-1.0-quickpanel.png` shows the panel's empty-notification
  state ("No new notifications") plus the default widget set.
- Capture evidence for the app-launch shots: guitest run
  `gui-20261101-163532` (file manager, updates, ElixirOffice, feedback)
  plus direct launch-capture-close passes for CryptPad, Remote Work,
  the launcher and the quick panel; every shot was reviewed before
  publishing (no emails, accounts, tokens or internal identifiers).

## From the 2026-10-21 set (unchanged)

| File | Application | Content |
|------|-------------|---------|
| `fihri-os-1.0-browser.png` | Brave Browser | New tab page (shields stats, news/rewards cards) |
| `fihri-os-1.0-elixiroffice-writer.png` | ElixirOffice Writer | Sample .docx open with full ribbon |
| `fihri-os-1.0-elixiroffice-spreadsheet.png` | ElixirOffice Spreadsheet | Sample .xlsx with ribbon and formula bar |
| `fihri-os-1.0-elixiroffice-presentation.png` | ElixirOffice Presentation | Sample .pptx open |
| `fihri-os-1.0-draw.png` | Deepin Draw | Maximized blank canvas |
| `fihri-os-1.0-terminal.png` | Deepin Terminal | Clean shell prompt |
| `fihri-os-1.0-settings.png` | Control Center | Settings grid |
| `fihri-os-1.0-calculator.png` | Deepin Calculator | Standard calculator, natural size |
| `fihri-os-1.0-music.png` | Deepin Music | Empty library state |

- Launchpad is shown in windowed (mini) mode; fullscreen mode is not
  reachable via the public D-Bus API on this build.
- Capture method: X11 full-frame capture (`scrot`) per state, window
  crops via ImageMagick from the real framebuffer using the window's
  geometry. All captures visually verified.
