# archer.background

Omarchy background renderer that notifies archer.whimsy when the wallpaper reveal starts (so widget layouts can wipe in sync).

A modified copy of the built-in `omarchy.background` plugin from [Omarchy](https://github.com/basecamp/omarchy) (MIT). It replaces the stock plugin when enabled; `omarchy plugin remove archer.background` restores it.

Install: `omarchy plugin add <this repo's git URL> --enable`

Personal config, shared as-is: no support or compatibility promises.

Optional integration: if `archer.whimsy` is installed (as a sibling plugin directory), the reveal start is reported to it. Without it the hook does nothing.
