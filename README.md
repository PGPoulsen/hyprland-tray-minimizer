Note this is completely wipe coded. I have no idea how it works.

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

## Installation

Clone the repo, then

```
cargo build --release
```

to get a binary in the `target/release/` directory.

## Configuration

Update for your hyprland.lua

```
hl.bind(mainMod .. " + Z", hl.dsp.exec_cmd(".../target/release/hyprland-tray-minimizer"))
```


