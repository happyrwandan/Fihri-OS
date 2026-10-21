# Fihri OS 1.0 Screenshots

Captured on Fihri OS 1.0 (CanLinux base, Deepin DDE) on 2026-10-21.
All windows were launched via the Deepin Application Manager and closed after
capture; no input was injected into any application. 1920x1080 display.

| File | Application | Content |
|------|-------------|---------|
| `fihri-os-1.0-desktop.png` | Desktop | Full frame: Fihri OS 1.0 wallpaper and dock |
| `fihri-os-1.0-launchpad.png` | Launchpad | Windowed launcher with app grid and categories |
| `fihri-os-1.0-browser.png` | Brave Browser | New tab page (shields stats, news/rewards cards) |
| `fihri-os-1.0-elixiroffice.png` | ElixirOffice | Start page: Home / New / Open with templates |
| `fihri-os-1.0-elixiroffice-writer.png` | ElixirOffice Writer | Sample .docx open with full ribbon |
| `fihri-os-1.0-elixiroffice-spreadsheet.png` | ElixirOffice Spreadsheet | Sample .xlsx with ribbon and formula bar |
| `fihri-os-1.0-elixiroffice-presentation.png` | ElixirOffice Presentation | Sample .pptx open |
| `fihri-os-1.0-cryptpad.png` | CryptPad | Rich text pad with structural ribbon (File / Font / Paragraph, Share / Collaborate) |
| `fihri-os-1.0-draw.png` | Deepin Draw | Maximized blank canvas |
| `fihri-os-1.0-files.png` | File Manager | Computer view with Quick access sidebar |
| `fihri-os-1.0-terminal.png` | Deepin Terminal | Clean shell prompt |
| `fihri-os-1.0-settings.png` | Control Center | Settings grid |
| `fihri-os-1.0-calculator.png` | Deepin Calculator | Standard calculator, natural size |
| `fihri-os-1.0-feedback.png` | Fihri Feedback | Feedback form with local bundle preview |
| `fihri-os-1.0-updates.png` | Fihri OS Updates | Update list (46 packages, 19 security, 1 app update) |
| `fihri-os-1.0-music.png` | Deepin Music | Empty library state |

## Notes

- `fihri-os-1.0-cryptpad.png` shows CryptPad's anonymous "store this pad?"
  toast at the bottom-right. It was left in frame deliberately: dismissing it
  would require injecting a click, which the capture policy forbids.
- `fihri-os-1.0-desktop.png` replaces the earlier dated capture
  (`fihri-os-1.0-desktop-20261020.png`), which predated the Fihri wallpaper
  and has been removed.
- Launchpad was captured in windowed (mini) mode; full-screen mode is not
  reachable via the public D-Bus API on this build.
- Capture method: X11 `import` per window after raise/activate; full-frame
  shots via `scrot`. All captures visually verified.
