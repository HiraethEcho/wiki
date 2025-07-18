---
title: "Universal Wayland Session Manager - ArchWiki"
source: "https://wiki.archlinux.org/title/Universal_Wayland_Session_Manager"
date: 2025-06-07
description:
dg-publish: true
---
The [Universal Wayland Session Manager](https://github.com/Vladimir-csp/uwsm) (**uwsm**) wraps standalone [Wayland compositors](https://wiki.archlinux.org/title/Wayland#Compositors "Wayland") into a set of [systemd](https://wiki.archlinux.org/title/Systemd "Systemd") units on the fly. This provides robust session management including environment, [XDG Autostart](https://wiki.archlinux.org/title/XDG_Autostart "XDG Autostart") support, bi-directional binding with login session, and clean shutdown.

## Installation

[Install](https://wiki.archlinux.org/title/Install "Install") the [uwsm](https://archlinux.org/packages/?name=uwsm) package.

## Configuration

### Service startup notification and vars set by compositor

**Note:** If managed compositors already set `WAYLAND_DISPLAY` (and other useful environment variables) into the systemd activation environment, then you can skip this section and you do not need to use `uwsm finalize`.

In order to find the current compositor, a Wayland application run as a systemd service needs the `WAYLAND_DISPLAY` environment variable (or `DISPLAY` if they are intended to run through [Xwayland](https://wiki.archlinux.org/title/Xwayland "Xwayland")). Therefore this and other useful environment variables should be put into the systemd/dbus activation environment once the compositor has set their values.

The command `uwsm finalize` puts `WAYLAND_DISPLAY`, `DISPLAY` and other environment variables listed via the white-space separated `UWSM_FINALIZE_VARNAMES` list into the activation environment. It is recommended to execute this command after the compositor is ready.

If other variables set by the compositors are needed in the activation environment, they can be passed as arguments to `uwsm finalize` or put into a white-space separated list in `UWSM_FINALIZE_VARNAMES`. See the examples below:

```
exec uwsm finalize VAR1 VAR2 ...
export UWSM_FINALIZE_VARNAMES=VAR1 VAR2 ...
```

### Environment variables

All environment variables set in `${XDG_CONFIG_HOME}/uwsm/env` are sourced by *uwsm* and available to all managed compositors and graphical applications run inside such a session.

If you need certain environment variables to be set only for a specific *compositor* (and graphical applications in that graphical session), then put them in `${XDG_CONFIG_HOME}/uwsm/env-*compositor*` instead.

An example of such a file can be seen below:

```
~/.config/uwsm/env
```
```
export KEY1=VAR1
export KEY2=VAR2
export KEY3=VAR3
...
```

## Usage

### Startup

**Note:** Environment preloader no longer sources POSIX shell profile if environment from the context of `uwsm start` was successfully used.

*uwsm* can be started both by TTY and by a [display manager](https://wiki.archlinux.org/title/Display_manager "Display manager").

#### TTY

Add in your `~/.profile` file:

```
if uwsm check may-start && uwsm select; then
  exec uwsm start default
fi
```

If you want to always start the same *compositor*, then you can use instead in your `~/.profile` file:

```
if uwsm check may-start; then
  exec uwsm start compositor.desktop
fi
```

#### Display manager

You can create a custom session desktop entry which starts your compositor through *uwsm*:

```
/usr/share/wayland-sessions/my-compositor-uwsm.desktop
```
```
[Desktop Entry]
Name=My compositor (with UWSM)
Comment=My cool compositor, UWSM session

# either full command line with metadata and executable
Exec=uwsm start -N "My compositor" -D mycompositor:mylib -C "My cool compositor" -- my-compositor

# or a reference to another entry
Exec=uwsm start -- my-compositor.desktop

DesktopNames=mycompositor;mylib
Type=Application
```

### Session termination

If you want to terminate the current uwsm session, then you should use either `loginctl terminate-user ""` (terminates the entire user session) or `uwsm stop` (executes code after `uwsm start` or terminates user session, if it replaced the [login shell](https://wiki.archlinux.org/title/Login_shell "Login shell")).

**Note:** Do not use a compositor's native exit mechanism or kill its process directly. This will yank the compositor from under all the clients and interfere with ordered unit deactivation sequence.

## Tips and tricks

### Applications and autostart

By default uwsm launches compositors through a custom systemd service in `session.slice`. Many Wayland compositors allow you to start other applications that would then be launched inside the compositor service, which would uselessly consume compositor resources or even interfere with notification sockets.

To start applications as separate systemd scope units you can use `uwsm app`, which can launch both executables

```
uwsm app -- /my/program/path
```

and [desktop entries](https://wiki.archlinux.org/title/Desktop_entries "Desktop entries")

```
uwsm app -- myprogram.desktop
```

By default uwsm puts launched scope units in `app-graphical.slice` slice. If you want to put them in `background-graphical.slice` or `session-graphical.slice`, then you should respectively use options `-s b`, `-s s`:

```
uwsm app -s b -- background-app.desktop
```

Instead of `uwsm app` (which run as a Python script) you can instead use [app2unit-git](https://aur.archlinux.org/packages/app2unit-git/) <sup><small>AUR</small></sup>, which is generally faster since it is a shell script. You can use it as a drop-in replacement of `uwsm app` by setting the `APP2UNIT_SLICES` environment variable as follows:

```
APP2UNIT_SLICES='a=app-graphical.slice b=background-graphical.slice s=session-graphical.slice'
```

## See also

- [uwsm(1)](https://man.archlinux.org/man/uwsm.1)