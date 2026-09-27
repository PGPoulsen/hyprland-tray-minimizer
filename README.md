# Hyprland tray minimizer

Minimizes the currently focused window in Hyprland to the tray. The minimized
application shows up in the Waybar tray; the icon uses the window's class (or
title, if the class is empty), so you may need to adjust your Waybar config to
show the right icon. Left-click the tray icon to restore the window to the
current workspace and focus it; the menu (right-click) also offers "open on
original workspace" and "close". Middle-click closes the window directly.

Minimizing is done by moving the window to a special workspace
(`special:minimized`) and registering a `org.kde.StatusNotifierItem` /
`com.canonical.dbusmenu` D-Bus service that Waybar's tray module picks up. It
re-registers automatically if Waybar restarts, and exits on its own if the
window is closed or restored some other way.

This is a rewrite of `hyprland-minimizer`, which stopped working once
Hyprland moved its `hyprctl dispatch` command to a Lua-based dispatcher API
(Hyprland >= 0.56): the old code sent classic string dispatchers like
`movetoworkspacesilent special:minimized,address:0x...`, which the new
`hyprctl` parses as Lua and rejects. This version issues dispatches the new
way instead, e.g.:

```
hyprctl dispatch 'hl.dsp.window.move({ window = "address:0x...", workspace = "special:minimized", follow = false })'
```

If a future Hyprland release changes this API again, that's the function to
fix: `hyprctl_dispatch_lua` in `src/main.rs`, plus the `move_window_to_workspace`
and `close_window` helpers that build the dispatcher expressions.

## Installation

Clone the repo, then

```
cargo build --release
```

to get a binary in the `target/release/` directory.
