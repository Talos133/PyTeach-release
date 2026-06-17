# PyTeach — Installation & Setup

PyTeach is a self-contained desktop learning platform for Python test automation. It ships with everything bundled — Python runtime, all lesson content, a live code execution environment, and offline mock servers — so there is nothing to install beyond the app itself.

This document covers system requirements, installation steps per platform, any third-party dependencies, and a troubleshooting reference for common issues.

---

## System Requirements

| Platform | Minimum | Notes |
|---|---|---|
| **macOS** | macOS 13, Apple Silicon (M1 or later) | Intel Macs not currently supported |
| **Windows** | Windows 10 (64-bit) or Windows 11 | Installer coming soon |
| **Linux** | Ubuntu 22.04 LTS / Debian 12 or equivalent | `.deb` and `.AppImage` coming soon |

No Python installation is required on any platform. PyTeach bundles its own isolated Python runtime.

---

## Installation

### macOS

1. Download `PyTeach_<version>_aarch64.dmg` from the [Releases page](../releases/).
2. Open the `.dmg` file — a Finder window appears showing the PyTeach app.
3. Drag **PyTeach** into your **Applications** folder.
4. Eject the disk image.
5. **First launch only:** macOS Gatekeeper will block the app because it is not signed with an Apple Developer certificate. To open it:
   - Right-click (or Control-click) PyTeach in Applications.
   - Select **Open** from the context menu.
   - Click **Open** in the dialog that appears.
   - You will not need to do this again for subsequent launches.
6. PyTeach will open. Expect the first launch to take **8–12 seconds** while the bundled Python runtime extracts itself — this is normal (see [Troubleshooting](#troubleshooting--faq) below).

### Windows

> Installer coming in a future release. Check the [Releases page](../releases/) for updates.

### Linux

> `.deb` and `.AppImage` packages coming in a future release. Check the [Releases page](../releases/) for updates.
>
> If you are setting up a Linux environment ahead of release, see the [Linux: Narration](#linux-narration-text-to-speech) section below for the one optional dependency you may want to install in advance.

---

## Third-Party Requirements

PyTeach has **no required third-party dependencies** on macOS or Windows.

### Linux: Narration (Text-to-Speech)

PyTeach includes a lesson narration feature that reads lesson content aloud using the operating system's native voice engine. On macOS and Windows this works out of the box. On Linux, the voice engine requires **speech-dispatcher** and at least one voice package to be installed.

Without speech-dispatcher, the narration controls will appear in the app but the Play button will be disabled with a tooltip explaining why. All other features work normally.

**To enable narration on Linux (Ubuntu/Debian):**

```bash
sudo apt update
sudo apt install speech-dispatcher speech-dispatcher-espeak-ng
```

After installation, verify it is running:

```bash
spd-say "Hello from PyTeach"
```

If you hear audio, narration will work in PyTeach without any further configuration.

**Other distributions:**

| Distro family | Package manager command |
|---|---|
| Fedora / RHEL | `sudo dnf install speech-dispatcher` |
| Arch Linux | `sudo pacman -S speech-dispatcher` |
| openSUSE | `sudo zypper install speech-dispatcher` |

You may also want to install additional voice packs (e.g. `espeak-ng-data`, `festival`, or a higher-quality engine like `flite`) depending on what is available in your distribution's repositories.

---

## First Launch

PyTeach bundles a full Python runtime using PyInstaller. On the very first launch (and any launch after an OS cache clear), the runtime extracts itself to a temporary directory before the app becomes responsive. This is expected behaviour, not a hang.

**What you will see:**
- The app window appears within 1–2 seconds.
- A "Starting up…" message is shown on the profile picker screen.
- The app becomes fully interactive after **8–12 seconds** on a modern machine (M-series Mac; times may vary on other hardware).

Subsequent launches on the same machine are not meaningfully faster because the extraction destination is cleaned on each run — this is how PyInstaller's `--onefile` mode works.

---

## Troubleshooting / FAQ

### The app takes 8–12 seconds to become usable after opening

**This is normal.** PyTeach bundles the entire Python runtime and all dependencies into a single binary using PyInstaller. On every launch, the binary extracts ~60 MB of Python internals to a temporary directory before the backend server can start. Nothing is wrong with your installation.

If you are on a machine with a very slow disk or limited RAM and the app never becomes responsive after 30 seconds, try restarting the app.

---

### I see "Could not reach the app backend. Please restart PyTeach."

The app polls the backend server for up to 20 seconds on startup. If the backend has not responded within that window — typically because the machine is under heavy load — this error is shown.

**What to try:**
1. Click **Retry** if the button is available, or close and reopen the app.
2. If the error recurs consistently, check that nothing is blocking port **57391** on localhost (see firewall note below).
3. On macOS, check that PyTeach has not been quarantined by Gatekeeper. If it was opened by double-clicking instead of right-click → Open on first launch, the backend binary may be blocked. Delete the app from Applications, re-drag it from the DMG, and use the right-click → Open method.

---

### The backend (sidecar) never starts — app stays on "Starting up…" indefinitely

The PyTeach backend runs as a local server on **port 57391**. Security software can block this.

**Check the following:**
- A firewall rule is not blocking localhost connections on port 57391.
- Antivirus software is not quarantining the `pyteach-sidecar` binary inside the app bundle (`PyTeach.app/Contents/Resources/`).
- On Linux, confirm that the extracted binary has execute permissions (this can be lost on some filesystems).

---

### Quiz submit does nothing / the Submit button is greyed out after I select all answers

The Submit button is only enabled after every question in the quiz has a selected answer. If it is still unresponsive after confirming all answers are selected:

1. Check the bottom of the quiz panel for an error message ("Could not submit — please try again.").
2. This indicates the backend is not reachable. Restart the app and try again.
3. If the problem persists, make sure the sidecar is running (see above).

---

### Lesson narration is not working on macOS or Windows

On macOS and Windows, narration uses the built-in OS voice engine and requires no additional setup. If the Play button is disabled:

- Ensure you are running the app natively (not in a VM or remote desktop session where audio output is unavailable).
- Try clicking the Play button — on some macOS configurations the voices load after the first speak attempt. If the button remains disabled after the first click, the OS voice engine returned an error; restarting the app usually resolves this.

---

### Lesson narration is not working on Linux

The narration feature on Linux requires `speech-dispatcher`. If the Play button is disabled with the tooltip *"No voices available — see the Linux installation guide"*, follow the installation steps in [Linux: Narration](#linux-narration-text-to-speech) above.

---

### The Playwright browser exercises fail to launch a browser

Playwright exercises require Chromium (bundled) or WebKit (system, macOS only). If a browser fails to launch:

- Navigate to **Settings** (⚙️ in the top navigation bar) and confirm your browser preference is set correctly.
- On Linux, Playwright's Chromium may require additional system libraries. Run `playwright install-deps chromium` in a terminal to install them.
- WebKit is only available on macOS. On Windows and Linux, use Chromium.
