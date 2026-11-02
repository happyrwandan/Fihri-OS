# Fihri OS 1.0 Screenshots

**Showcase set refreshed on 2026-11-01 (evening pass)** on the Fihri OS 1.0
reference machine (1920×1080, light theme) against the desktop state that
ships in the rebuilt 1.0 ISO (`f2a70ebf…`, reseal 2026-11-01, final r4
rewrap 2026-11-02): final theme
engine, filled icon set, hero wallpaper as the fresh-user default, windowed
launcher, Quick Actions panel, and the fixed updater.

All windows were launched through the system's normal application-launch
paths (Deepin Application Manager D-Bus launches, the launcher's own
`org.deepin.dde.Launcher1` / `org.deepin.dde.Widgets1` interfaces) and
closed again after capture; no input was injected into any application.
Window shots are cropped from full-frame captures of the real display.

## Refreshed 2026-11-01 evening (showcase set)

| File | Application | Content |
|------|-------------|---------|
| `fihri-os-1.0-desktop.png` | Desktop | Full 1920×1080 frame: Colorful-Abstraction02 hero wallpaper (the fresh-user default in the rebuilt ISO), filled icons, translucent dock, tray |
| `fihri-os-1.0-launchpad.png` | Launcher | Windowed launcher: app grid with search box |
| `fihri-os-1.0-files.png` | File Manager | Computer view with sidebar (launch-verified via guitest) |
| `fihri-os-1.0-updates.png` | Fihri OS Updates | Selectable update list with details pane — apt security/feature updates plus "Application Updates" (CryptPad, ElixirOffice) and "Driver Updates" sections (this machine offers NVIDIA 580/580-server/470 and DisplayLink) |
| `fihri-os-1.0-elixiroffice.png` | ElixirOffice | Start page (Home / New / Open with templates) |
| `fihri-os-1.0-cryptpad.png` | CryptPad | Home page with document-type grid ("Fihri OS CryptPad" app window) |
| `fihri-os-1.0-remote.png` | Remote Work | First-run wizard (Citrix Workspace / VMware Horizon / RDP) |
| `fihri-os-1.0-quickpanel.png` | Quick Actions panel | Slide-in panel: notification center + weather / calendar / memory / world-clock widgets |
| `fihri-os-1.0-settings.png` | Control Center | Settings grid (launch-verified via guitest) |
| `fihri-os-1.0-personalization.png` | Settings — Personalization | Theme page with the full theme gallery (light/dark variants), reached via the Control Center D-Bus page API |
| `fihri-os-1.0-browser.png` | Brave Browser | New tab page (shields stats, news cards) |

Notes on this set:

- The desktop shot shows the **Colorful-Abstraction02** wallpaper (the
  VM-reference hero image that the rebuilt ISO sets as the fresh-user
  default); the capture machine's own session wallpaper was restored
  afterwards.
- Capture evidence for the app-launch shots: guitest run
  `gui-20261102-000413` (file manager, updates, ElixirOffice, remote work,
  control center — all launch/window/a11y-verified, closed cleanly) plus
  direct AM launch-capture-close passes for CryptPad, Brave, and the
  Personalization page; launcher and quick panel via their own D-Bus
  interfaces. Every shot was reviewed before publishing (no emails,
  accounts, tokens or internal identifiers).
- Launchpad is shown in windowed (mini) mode; fullscreen mode is not
  reachable via the public D-Bus API on this build.
- The remote-work window shows the wizard's clean first-run state — no
  accounts, gateways or credentials are in frame.

## From the 2026-10-21/11-01 sets (unchanged)

| File | Application | Content |
|------|-------------|---------|
| `fihri-os-1.0-feedback.png` | Fihri Feedback | Feedback form: consent checkboxes, local bundle preview (2026-11-01 morning) |
| `fihri-os-1.0-elixiroffice-writer.png` | ElixirOffice Writer | Sample .docx open with full ribbon |
| `fihri-os-1.0-elixiroffice-spreadsheet.png` | ElixirOffice Spreadsheet | Sample .xlsx with ribbon and formula bar |
| `fihri-os-1.0-elixiroffice-presentation.png` | ElixirOffice Presentation | Sample .pptx open |
| `fihri-os-1.0-draw.png` | Deepin Draw | Maximized blank canvas |
| `fihri-os-1.0-terminal.png` | Deepin Terminal | Clean shell prompt |
| `fihri-os-1.0-calculator.png` | Deepin Calculator | Standard calculator, natural size |
| `fihri-os-1.0-music.png` | Deepin Music | Empty library state |

- Capture method: X11 full-frame capture (`scrot`) per state, window
  crops via ImageMagick from the real framebuffer using the window's
  geometry. All captures visually verified.
