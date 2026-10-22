# DWUT 10 - Darko's Windows Utility Tool

One app for tuning, cleaning, repairing and tidying up a Windows 10 / 11 PC. DWUT 10 is an Electron app (the UI) on top of a Python
backend (all the real logic, PowerShell scripts included). Electron talks to the backend over a JSON pipe (`backend/server.py`).
Windows only, 64-bit / 32-bit / ARM64. Most features need Administrator.

## Install / run
* **One-liner** (PowerShell, any window):
  `irm https://raw.githubusercontent.com/supportpantheon-sys/Darkos-Windows-Utility-Tool-DWUT/main/bootstrap.ps1 | iex`
* **From this folder:** double-click `run.bat` (or right-click `bootstrap.ps1` > Run with PowerShell). It elevates itself, installs what is
  missing (Python 3.10+, the pip packages, the Electron runtime with a SHA-256 check), makes a desktop shortcut and launches DWUT.
  Run it again any time to repair or update an install. Everything lives in `%LOCALAPPDATA%\DWUT`.
* **Dev:** `npm install`, `npm start` (needs Python 3.10+ and `pip install -r requirements.txt`).
* **Lost internet after a crash while a VPN / proxy was on?** Run `Repair-Internet.bat`, or use the red "Restore Internet" button.

## What's inside

**Sidebar pages:** Dashboard, Optimizer, Gaming, Debloat, Proxy Center, VPN, Network Monitor, USB Flasher, VM Builder, Tools, Cleaner, Settings.

### DWUT 1 CLICK BUTTON (Dashboard, tray)
The whole tune-up in one run, in a safe order: restore point, debloat, clean + optimize every drive, free RAM / CPU / GPU, tweaks,
TBOK-style system optimizations, Windows Recall off (Windows 11), extra safe cleanups, DISM, SFC, High CPU Power Config repair,
every app on the high-performance GPU, power mode, visual effects, and a network stack reset last. You only decide five things, each in a
popup that looks like the real page: apps to remove, tweaks, system optimizations (55 individual switches), power mode, visual effects.
Live step list and log, "Stop after current step", and a tray notification when it finishes. Takes about 20 - 60 minutes (mostly DISM and SFC).
Xbox apps and services are never touched. Service start modes are saved to `%LOCALAPPDATA%\DWUT\Backups` before they change.

### TBOK (CORPORATE AND BUSINESS) (Dashboard, tray)
Runs The Beard of Knowledge's TBOK Windows Performance Optimizer v1.6.2, bundled unmodified in `backend/vendor/tbok`, in its own console window.
A warning popup explains who it is for first, and a credits popup + notification links to the author's repository. Credit and thanks to
TheBeardOfKnowledge: https://github.com/TheBeardofKnowledge/TBOK-Win11Optimizer

### Optimizer
Repairs (DISM / SFC and more), Config Tools (Command Prompt, PowerShell, Group Policy, Local Security, Performance Options with a
"DWUT recommended" preset), Updates (Windows Update and WinGet app updates where you tick exactly which apps), Performance Plans
(Ultimate and Extreme Performance with verified activation).

### Gaming
Game Booster: closes background apps you choose (an exclusion popup opens first), power plan, process priority, auto-detects the running game
(130+ known games). FPS Unlocker for Roblox (every install, incl. Store, Bloxstrap, Fishstrap, Voidstrap), Fortnite and Minecraft Java by
editing config files only - nothing touches game memory.

### Debloat
Pick the apps and features to remove, with a restore point first.

### VPN and Proxy Center
* **VPN:** "Automatic" carries the whole PC through a full-system tunnel with a kill switch and DNS protection; choosing a country uses the
  shared-server pool. Change country while connected. Everything it changes is recorded in `%LOCALAPPDATA%\DWUT\vpn_state.json` and undone
  on disconnect, on exit and on the next start after a crash.
* **Proxy Center:** checker (ping, jitter, YouTube, Discord, real speed per proxy), a local failover relay (HTTP / SOCKS4 / SOCKS5), and a
  proxy-based VPN mode with kill switch, DNS-leak, IPv6 and WebRTC protection. Free proxies are free-proxy speed.

### Network Monitor
Live per-app network use.

### USB Flasher and VM Builder
* **USB Flasher:** flash a stick (pick drive, pick OS, automatic download + checksum + write + verify), Ventoy multi-boot, restore a stick to
  normal, back up to `.img`, ISO library. Windows 10/11 (official ISO), Ubuntu, Mint, Fedora, Debian, Arch, openSUSE, Alpine, Kali, Parrot,
  Tails, Hiren's PE and more, or your own ISO. Only USB disks are shown and nothing is erased until the download is verified.
* **VM Builder:** pick drive, OS, specs. VirtualBox is installed silently and the VM created (unattended install where the ISO supports it);
  VMware gets the ISO plus a step-by-step guide.

### Cleaner and Tools
Disk Cleanup (every event-log channel, temp files, per-file freed-space numbers), Deep Clean (downloads and runs malware scanners, then removes
everything it installed; "Protect programs" popup and DWUT's own processes are always protected), registry cleaner, startup manager and a
tool launcher. "Check Tool Links" reports which download links are alive.

### Tools > Health
A real health scan of the whole PC, with a score bar per component (full and green = healthy, short and red = problem) and one overall score.
Nothing is simulated: every reading comes from Windows (WMI / CIM), the drives' own SMART data, the Windows event log (last 30 days),
NVIDIA's NVML, and LibreHardwareMonitor sensors when that is running. Checked: **CPU** (temperature, load, clock, hardware-error reports,
firmware throttling), **memory** (use, commit, paging, out-of-memory events, last Windows Memory Diagnostic, RAM speed), **every drive**
(Windows health, SMART failure prediction, SSD wear, temperature, uncorrectable errors, disk-error events, free space), **GPU** (temperature,
thermal throttling, driver crashes, VRAM, PCIe link), **battery** (wear, cycles), **cooling** (thermal zones, throttle limit, fans),
**network** (per adapter: link speed, error/drop rate, router and internet loss / latency / jitter, DNS, Wi-Fi signal),
**motherboard and firmware** (board and BIOS identity, UEFI / Secure Boot / TPM, CPU virtualization, memory slots, power-rail voltages),
**drivers and devices** (Device Manager errors, unsigned and old drivers) and **Windows** (unexpected shutdowns, blue screens, DISM
component-store check, antivirus, firewall, update age, boot time). Every card shows who made the part and its exact model (CPU, each RAM stick with its part number, each drive with its firmware, GPU with its
PCI ID and card maker, motherboard and BIOS, network adapters, battery, monitors, audio, OS), and a "Your hardware" list sums it all up. A brand that
had to be worked out from a part number or PCI ID says so next to it; serial numbers and MAC addresses are never collected. A reading the PC cannot give (sensor not exposed, needs Administrator)
shows "Not available" with the reason and is left out of the score - it is never guessed. "Copy report" puts the full report on the clipboard.
Motherboard / BIOS detection lives in its own module (`backend/modules/health/motherboard.py`, RPC `health.motherboard`) so a BIOS category can reuse it.

### Built-in windows and tray
* **DWUT Task Manager** (Ctrl+Shift+Esc) and **Run** window (Win+R): DWUT-themed replacements, can be turned off in Settings.
* **Command Prompt / PowerShell windows** wrapped around the real shell (ConPTY), with an "open as administrator?" choice.
* **Tray menu:** Open DWUT, Task Manager, Command Prompt, PowerShell, 1 CLICK, TBOK, Quick Optimize / Clean / SFC + DISM / Game Boost,
  Network Monitor, VPN controls, Exit. Closing the window hides DWUT to the tray; **Exit** ends DWUT completely.
* **Personalization:** themes, music-reactive backgrounds (live beat detection from what your speakers play), particles.
* One shared sampler feeds CPU, RAM, GPU, disk activity, network and temperatures, so every page shows the same numbers.

## Exiting
Tray > Exit, the Task Manager's End Task, or closing the console DWUT was started from all shut down the backend, terminal helpers and VPN /
proxy changes. If an earlier run ever gets stuck, the next launch detects it after about 8 seconds, ends it and starts fresh. Tools DWUT
launched for you (the TBOK window, DISM / SFC runs, Windows tools) are left running on purpose.

## Requirements
Windows 10 (1809+) or 11, Python 3.10+, internet for installing and for downloads. Python packages are in `requirements.txt` (psutil, pywin32,
requests, wmi, nvidia-ml-py, PySocks required; pycaw, numpy, PyAudioWPatch, pywinpty optional and installed automatically when used).

## Honest limits
* The VPN's proxy-based modes only cover apps that follow Windows proxy settings (browsers, Discord, Store apps, most clients), not games or apps
  using raw UDP; the kill switch still blocks leaks, so those apps will not work while it is on.
* FPS Unlocker only offers games that have a config-file cap. Valorant, Apex and similar have none, so they are not offered.
* Parts of DWUT were tested with Windows APIs simulated. Real-hardware behaviour (powercfg, DISM, UAC, tool downloads) can differ, so if
  something misbehaves check `%APPDATA%\DWUT\logs` and `%LOCALAPPDATA%\DWUT\logs`.

## Version history
Full per-round notes are in `CHANGES-V9.md`. Short version:
* **v10:** Efficiency pass: the Task Manager backend worker is demand-driven and hidden / minimised windows stop their timers and animation loops, so idle CPU / GPU use is much lower. Theme-aware visual polish layer and animation engine across every page, redesigned Health tab. Refined darker gradient aesthetics with deep contrast tones, tactile button-styled sliders, enlarged high-legibility console toggle switches, fixed transparency control, interactive mascot Game Booster shortcut, dynamic telemetry alerts with themed disk status, and enhanced game overlay with multi-key shortcuts (Ctrl+Shift+O / F4).
* **v9:** Tools > Health scan, DWUT 1 CLICK BUTTON, TBOK button, system optimizations popup, DWUT Task Manager + Run window, terminal windows, shared performance
  sampler, new beat detection, tray VPN fixes, clean-exit fix.
* **v6:** VPN as its own category, VM Builder, 130+ games in the Game Booster, Deep Clean protection popup.
* **v5:** USB Flasher, VPN location changer, rewritten power plans, update toast.
* **v4:** Proxy VPN mode with kill switch.  **v3:** proxy failover relay.  **v1 - v2:** Electron rewrite of the original CustomTkinter app,
  Disk Cleanup, Game Booster, FPS Unlocker and Deep Clean fixes.

---
Copyright © 2026 Darko / Darko.Dev
