# Arch Linux: Windows 7 Theme for KDE Plamsa Desktop Environment

This repository documents how to deploy a Windows 7–style desktop on KDE Plasma 6 (Wayland) using:
* **[AeroThemePlasma](https://gitgud.io/wackyideas/aerothemeplasma.git)** for the full Windows 7 visual stack
* **SevenTasks / SevenStart** for taskbar & start menu behavior
* **kysguard6-git (AUR)** for Plasma 6 system monitoring compatibility
* Custom **KWin**, **Plasma**, and **theme components**

Warning: This modifies internal Plasma components.

---

## Prerequisites
* Arch Linux (tested on archinstall installation)
* KDE Plasma 6 (Wayland session)
* SDDM installed and working

---

## Step 1 — Install Required Dependencies

Install all required build tools and runtime dependencies:
```bash
sudo pacman -S git cmake extra-cmake-modules ninja curl unzip qt6-virtualkeyboard qt6-multimedia qt6-5compat qt6-wayland plasma-wayland-protocols plasma5support kvantum sddm sddm-kcm base-devel
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
yay -S kysguard6-git
```

## Step 4 — Clone AeroThemePlasma
```bash
git clone https://gitgud.io/wackyideas/aerothemeplasma.git
cd aerothemeplasma
```

## Step 5 — Compile Theme (Wayland)
```bash 
bash compile.sh --wayland --ninja
bash install_plasmoids.sh --ninja
bash install_kwin_components.sh
bash install_plasma_components.sh
bash install_misc_components.sh
```
When prompted, enter sudo password and install all fonts and custom branding, do not install plymouth thme (vista).

## Step 6 — Update Theme
```bash
cd /path/to/aerothemeplasma
git pull
```
Re-run:
```bash
bash compile.sh --wayland
bash install_plasmoids.sh
install_kwin_components.sh
bash install_plasma_components.sh
bash install_misc_components.sh
```

## Step 7
The theme should now be fully installed, now you must enable it.
* asdf (watch video to add these)
  
---

## Final Step: Freeze Plasma Updates 
* AeroThemePlasma depends on internal Plasma APIs.
* Future updates may break the theme stack.
* In order to prevent this, we must freeze updates.

--

## Step 1 — Edit pacman Configuration
```bash
sudo nano /etc/pacman.conf
```
Under [options], add:
```bash
IgnorePkg = plasma-desktop plasma-workspace libplasma kwin plasma5support systemsettings kde-cli-tools kdeplasma-addons breeze breeze-gtk plasma-integration plasma-nm plasma-pa plasma-systemmonitor plasma-browser-integration xdg-desktop-portal-kde sddm-kcm kdecoration aurorae layer-shell-qt libkscreen kscreenlocker kwayland kysguard6-git
```

## Step 2 — Verify Freeze
```bash
sudo pacman -Syu
```
You should see warnings such as:
```
warning: plasma-desktop: ignoring package upgrade
```
If you see this, Plasma is now frozen.

## Bypassing the Freeze (When You Intentionally Want to Update)
To temporarily update everything, including Plasma:
```bash
sudo pacman -Syu --ignore ""
```
This will temporarily override your IgnorePkg entries in /etc/pacman.conf.
