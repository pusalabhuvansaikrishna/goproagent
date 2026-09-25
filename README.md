# GoPro Agent

A desktop app that connects to GoPro cameras over USB and exposes them to
your web UI (`http://localhost:8787`). It runs as a small window with
**Start** and **Stop** buttons and a live log -- no Python install
required, everything needed is bundled.

## Download

Both the Windows and Linux builds are here:
**https://drive.google.com/drive/folders/1DIORlvyFOwlYvLpvWXblpHC00hA5ZC3f?usp=sharing**

Download the one matching your machine:
- `GoProAgent-windows.zip` (or similarly named) -- for Windows
- `GoProAgent-linux.tar.gz` -- for Linux

If the names in the Drive folder differ from these, just match them to
your OS -- there should only be one of each.

---

## Windows

1. Download the Windows build from the link above and unzip it
   anywhere (Desktop, a Programs folder, wherever). Keep every file in
   the extracted folder together -- `GoProAgent.exe` needs the
   `_internal` folder sitting right next to it to run.
2. Double-click `GoProAgent.exe`.
3. Click **Start**. Windows Firewall may prompt for network access on
   first run -- allow it on **Private networks**, or camera discovery
   and live preview won't work.
4. Plug in the GoPro (USB Connection mode set to "GoPro Connect" in the
   camera's Preferences > Connections). Watch the log for it being
   discovered and connected.
5. Click **Stop** when done, or just close the window -- it shuts the
   agent down cleanly either way.

**Logs**: `C:\Users\<you>\.signlanguage-agent\agent.log`

**If it won't start / crashes silently**: there's no console window by
default, so errors aren't visible. Ask whoever built it for a "debug"
build (a version that keeps a console window open) if you need to see
what's failing.

---

## Linux

### Requirements
- Linux, **x86_64** (same CPU architecture as whatever machine built
  it -- this won't run on ARM, e.g. a Raspberry Pi, unless it was built
  there specifically).
- **A desktop environment** (GNOME, KDE, XFCE, etc.) -- this is a
  windowed app with Start/Stop buttons, so it needs a display. A fully
  headless server with no GUI won't work as-is.
- GoPro set to "GoPro Connect" USB mode, same as above.

### Install / run
1. Download `GoProAgent-linux.tar.gz` from the link above.
2. Extract it and make the binary executable (only needed once):
   ```bash
   tar -xzf GoProAgent-linux.tar.gz
   cd GoProAgent
   chmod +x GoProAgent
   ```
3. Run it:
   ```bash
   ./GoProAgent
   ```
4. Click **Start**, plug in the camera, watch the log for it being
   discovered and connected.
5. Click **Stop**, or close the window -- both shut it down cleanly.

**Logs**: `~/.signlanguage-agent/agent.log`
**Recordings/photos**: `~/.signlanguage-agent/recordings` and `~/.signlanguage-agent/photos`

### If the camera isn't found on Linux
Work through these in order:

1. **Check the camera enumerates at the OS level.** Plug it in, then
   run `ip a` -- a new network-style interface should appear. If
   nothing shows up, it's a driver/kernel issue outside the app; try a
   different USB port/cable, or check `dmesg | tail -30` right after
   plugging in.
2. **Check for a USB permissions problem.** If `ip a` shows the
   interface but the agent's log shows connect failures, try `sudo
   ./GoProAgent` once as a test. If that fixes it, the normal user
   needs adding to the right group, or a udev rule -- ask if you hit
   this.
3. **mDNS/multicast must be allowed on the machine.** The agent finds
   cameras via mDNS. Some locked-down networks or firewalls block
   multicast by default -- worth checking with whoever manages the
   network if nothing is found after a minute.
4. **Read the log panel.** A camera that's discovered but fails to
   fully connect logs why (e.g. a wired-control handshake failure)
   rather than failing silently.

### Live preview / ffmpeg
If `ffmpeg` isn't bundled inside the `GoProAgent` folder already, live
preview needs ffmpeg installed system-wide:
```bash
sudo apt install ffmpeg      # Debian/Ubuntu
sudo dnf install ffmpeg      # Fedora
```
Recording and downloading clips work fine either way -- only live
preview needs ffmpeg.

---

## Common to both platforms

- **Port**: the agent always listens on `http://localhost:8787` once
  started. Your web UI talks to it there.
- **Moving to another machine**: copy the *whole* extracted/unzipped
  folder, not just the `.exe`/binary -- it needs the files next to it.
- **Updating**: these are frozen snapshots built at a point in time.
  When the agent's code changes, a new build needs to be made and
  redownloaded from the Drive link above -- there's no auto-update.
- **Windows and Linux builds are separate and not interchangeable** --
  make sure you grab the one matching your OS.
