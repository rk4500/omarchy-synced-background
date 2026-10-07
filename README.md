# archer.background

A modified copy of Omarchy's built-in `omarchy.background` (MIT). It renders the wallpaper exactly as the stock plugin does, with one addition: when the wallpaper-change animation starts, it tells [archer.whimsy](https://github.com/rk4500/whimsy) so desktop widgets can change layout in step with it.

## Why

`archer.whimsy` keeps a separate widget layout per wallpaper. When the wallpaper changes, the widgets for the new one wipe in with the same slanted reveal the wallpaper uses. Whimsy can't read the background plugin's animation state, so it needs to be told when the reveal begins. The reveal only starts once the new image has decoded, which is a few hundred milliseconds after the change is requested. Without the signal the widgets move visibly early.

## It's optional on both sides

- **Without `archer.whimsy`:** the hook file is loaded lazily and a missing file is handled, so this plugin behaves exactly like the stock one. Checked by loading a nonexistent component, which reports an error status and carries on.
- **Without `archer.background`** (stock `omarchy.background` instead): whimsy sees that no background plugin has announced itself and swaps layouts immediately, running its own wipe a little ahead of the wallpaper's. Checked with the stock plugin enabled.
- **With both:** whimsy waits for the reveal-start signal (with a 1.2 s fallback for wallpaper changes that don't animate) so the two edges line up.

The only coupling is a relative path: this plugin looks for `../archer.whimsy/WallpaperHook.qml`, so whimsy has to be installed in the same plugins directory under that id.

## Install

```bash
omarchy plugin add https://github.com/rk4500/omarchy-archer-background.git --enable
omarchy restart shell
```

Enabling it replaces the stock background plugin. To go back:

```bash
omarchy plugin remove archer.background
omarchy plugin enable omarchy.background
```

If the desktop ever goes black after a change here, switch back first.

## Changes from stock

`Background.qml` adds `ensureWhimsyHook()`, called once on load (to announce itself) and again when each reveal starts (to signal it). Nothing else differs, apart from the manifest id and name.

Changes to the stock plugin are not picked up automatically; they have to be merged in.

## Credits

Based on Omarchy's built-in background plugin by the Omarchy team (MIT, David Heinemeier Hansson). See [LICENSE](LICENSE).
