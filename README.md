# Arch Linux: Windows 7 Theme for KDE Plasma Desktop Environment

This repository documents how to deploy a Windows 7–style desktop on KDE Plasma 6 (Wayland) using:
* **[AeroThemePlasma](https://gitgud.io/aeroshell/atp/aerothemeplasma)** for the full Windows 7 visual stack
* **SevenTasks / SevenStart** for taskbar & start menu behavior
* **ksysguard6-git (AUR)** for Plasma 6 system monitoring compatibility
* Custom **KWin**, **Plasma**, and **theme components**

> Warning: This modifies internal Plasma components.

---

## Prerequisites
* Arch Linux (tested on archinstall installation)
  → *I use arch btw*
* KDE Plasma 6 (Wayland session)
* SDDM installed and working

---

## Step 1 — Install Required Dependencies

Install all required build tools and runtime dependencies:
```bash
sudo pacman -S git cmake extra-cmake-modules ninja curl unzip qt6-virtualkeyboard qt6-multimedia qt6-5compat qt6-wayland plasma-wayland-protocols plasma5support kvantum sddm sddm-kcm base-devel plasma-nm plasma-pa plasma-workspace plasma-desktop kwin-x11 plasma-x11-session
```

## Step 2 — Install yay (AUR Helper)

If yay is not installed:
```bash
git clone https://aur.archlinux.org/yay.git
```
```bash
cd yay
makepkg -si
```

## Step 3 — Install Plasma 6 KSysGuard Fork

Plasma 6 removed legacy KSysGuard components. This fork restores compatibility.
```bash
yay -S ksysguard6-git
```

## Step 4 — Clone AeroThemePlasma

```bash
git clone https://gitgud.io/wackyideas/aerothemeplasma.git aerothemeplasma
cd aerothemeplasma
```

## Step 5 — Install from Source

Run the install script. Use `--ninja` to reduce build times, and `--skip-x11` to skip building X11 components if not needed:

```bash
bash install.sh --ninja
```

> Do **NOT** run the install script with `sudo`, `doas`, or as root. The script will ask for admin privileges itself when needed.

It's highly recommended to keep the generated build files after installation — uninstalling AeroThemePlasma can be done using `sudo make uninstall` or `sudo ninja uninstall` in each build directory.

## Step 6 — Enable Theme Components

The theme should now be fully installed. Enable its components via:

**System Settings → Appearance & Style → Colors & Themes:**
* Global Theme: Windows 7 Style
* System Sounds: Windows 7
* Cursors: Windows 7
* Login Screen (SDDM): Windows 7
* Plasma Style: Windows 7
* Splash Screen: Windows 7

---

## Updating AeroThemePlasma

Pull the latest changes and re-run the install script — it will automatically pull changes for all cloned repositories and rebuild them:

```bash
cd /path/to/aerothemeplasma
git pull
bash install.sh --ninja
```

If something needs to be rebuilt completely, delete the repository folder causing the build error and re-run the install script. Note that after a full system upgrade, KWin effects and `libplasma` modifications may stop working — re-running the install script is required to restore them.
