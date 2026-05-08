# dwexec

A small utility for running graphical applications as root on Wayland using a graphical authentication prompt.

`dwexec` was originally created for Sway after repeated crashes of `polkit-gnome-authentication-agent-1`. Instead of relying on a desktop-specific polkit authentication agent, `dwexec` provides a lightweight alternative based on:

- `sudo`
- `pkexec`
- `yad`

The project was written primarily for personal use on Debian, but it should work on most Linux distributions, Wayland compositors, and desktop environments.

I could have simply switched to another polkit agent such as `lxpolkit` (which worked through Xwayland), but I decided to create something lightweight myself instead. If someone else finds this useful, even better :).

---

# Features

- Graphical authentication prompt using YAD
- Run GUI applications as root on Wayland
- No persistent polkit GUI agent required
- Lightweight shell implementation
- Works well in minimalist environments

---

# Screenshot

![dwexec screenshot](https://github.com/DiogenesVX/dwexec/blob/main/dwexec.png)

---

# Installation

Make the scripts executable and install them system-wide:

```bash
chmod +x dwexec
chmod +x yadaskpass

sudo cp dwexec /usr/local/bin
sudo cp yadaskpass /usr/local/bin
```

---

# Dependencies

Make sure the following packages are installed:

```text
yad
sudo
coreutils
policykit-1
```

---

# Usage

Examples:

```bash
dwexec /usr/sbin/gparted
dwexec gedit /etc/sysctl.conf
dwexec mousepad /etc/fstab
```

---

# Important Note

If you recently used `sudo`, the authentication timestamp may still be active and the application may launch without showing a password prompt.

To test the authentication dialog properly, either:

```bash
sudo -k
```

or close and reopen the terminal before running `dwexec`.

---

# Creating a Desktop Launcher

You can modify existing `.desktop` launchers to always use `dwexec`.

Example for GParted:

```bash
cp /usr/share/applications/gparted.desktop \
~/.local/share/applications

gedit ~/.local/share/applications/gparted.desktop
```

Find:

```text
Exec=/usr/sbin/gparted %f
```

and change it to:

```text
Exec=dwexec /usr/sbin/gparted %f
```

Now launching GParted from the application menu will automatically use `dwexec`.

---

# License

MIT License

