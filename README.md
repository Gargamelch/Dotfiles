# dotfiles

Personal configuration files for [niri](https://github.com/niri-wm/niri) (a scrollable-tiling Wayland compositor) and [Waybar](https://github.com/Alexays/Waybar).

## Structure

```
dotfiles/
├── niri/
│   ├── config.kdl         # Thin loader — includes the files below
│   ├── settings.kdl       # Input, output, layout, theming, startup, debug
│   ├── window-rules.kdl   # Per-app window rules, blur tuning, layer-rules (Waybar blur)
│   └── keybinds.kdl       # All keybindings
├── waybar/
│   ├── config.jsonc
│   └── style.css
└── vicinae/
    └── settings.json      # Launcher config (theme, keybinds, extensions, etc.)
```

## Requirements

- [niri](https://github.com/niri-wm/niri)
- [Waybar](https://github.com/Alexays/Waybar)
- Fonts: `Lexend`, `JetBrainsMono Nerd Font Propo`
- [Bibata-Modern-Classic](https://github.com/ful1e5/Bibata_Cursor) cursor theme
- `swaybg`, `dunst`, [vicinae](https://github.com/vicinaehq/vicinae), [Noctalia](https://github.com/noctalia-dev/noctalia-shell)
- `gnome-keyring`, `brightnessctl`, `playerctl`, `wpctl` (PipeWire/WirePlumber)

## Installation

Clone the repo, then symlink each app's config directory into place. This lets you edit configs normally through `~/.config/...` while git tracks the same files from `~/dotfiles/...`.

```bash
git clone <your-repo-url> ~/dotfiles

# Back up / remove any existing configs first
rm -rf ~/.config/niri ~/.config/waybar ~/.config/vicinae

# Symlink
ln -s ~/dotfiles/niri ~/.config/niri
ln -s ~/dotfiles/waybar ~/.config/waybar
ln -s ~/dotfiles/vicinae ~/.config/vicinae
```

Verify:

```bash
niri validate
```

Then restart Waybar (or just re-login / restart niri) to pick up the new configs.

## niri highlights

- **Config split across 4 files** via niri's [`include`](https://github.com/niri-wm/niri/wiki/Configuration:-Include) directive — all four are live-reloaded, so editing any of them applies instantly with no restart needed.
- **Theme**: purple focus ring + gradient border (`#9141ac` → `#bd93f9`), soft drop shadows, rounded window corners.
- **Blur**: enabled globally via `blur {}` tuning plus per-app `window-rule` opt-ins, and extended to Waybar itself (and its tooltips) via `layer-rule`, since layer-shell surfaces need a separate rule from regular windows.
- **AZERTY-aware keybinds**: workspace switching bound to the AZERTY symbol row (`&`, `é`, `"`, `'`, etc.) instead of number keys.
- Custom launcher (`vicinae`), lock screen (`swaylock`), and a handful of app-specific quick-launch binds (browser, code editor, office suite, VPN, LM Studio).

## Waybar highlights

- Catppuccin Mocha-inspired palette, semi-transparent bar background (`rgba(24, 24, 37, 0.6)`) to work with niri's compositor-level blur.
- Modules: workspaces, window title, CPU, memory, PulseAudio, MPRIS (media playback), clock, tray, custom logo/power buttons, Spotify integration.

## Vicinae highlights

- Config lives at `~/.config/vicinae/settings.json` — Vicinae writes to this file directly when settings are changed via its GUI, so expect it to get rewritten/reformatted by the app itself over time (not purely hand-authored).
- Launched via `vicinae open` (bound to `Mod+space` in niri's keybinds) with `vicinae server` running as a background daemon at startup.
- Config supports an `imports` array if you want to split it into multiple read-only files later, similar to niri's `include` mechanism.

## Notes

- `settings.kdl` references a wallpaper path (`spawn-at-startup "swaybg" ...`) and an output name (`DP-2`) specific to this machine — update these if reusing on another system.
- Run `niri msg outputs` to find your own output name/modes, and `niri msg layers` to find layer-shell namespaces if you want to extend the `layer-rule` blur to other bars/launchers.
