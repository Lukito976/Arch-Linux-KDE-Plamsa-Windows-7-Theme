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
sudo pacman -S git cmake extra-cmake-modules ninja curl unzip \
qt6-virtualkeyboard qt6-multimedia qt6-5compat qt6-wayland \
plasma-wayland-protocols plasma5support kvantum \
sddm sddm-kcm base-devel
```

---

Step 2 — Install yay (AUR Helper)

If yay is not installed:
```bash
git clone https://aur.archlinux.org/yay.git
```
```bash
cd yay
makepkg -si
```

---

Step 3 — Install Plasma 6 KSysGuard Fork

Plasma 6 removed legacy KSysGuard components. This fork restores compatibility.
```bash
yay -S kysguard6-git
```

⸻

Step 4 — Clone AeroThemePlasma

git clone https://gitgud.io/wackyideas/aerothemeplasma.git
cd aerothemeplasma


⸻

Step 5 — Compile Theme (Wayland)

bash compile.sh --wayland --ninja


⸻

Step 6 — Install Theme Components

Run all install scripts:

bash install_plasmoids.sh --ninja
bash install_kwin_components.sh
bash install_plasma_components.sh
bash install_misc_components.sh


⸻

Step 7 — Install Fonts & Branding

Install:
	•	All included Windows-style fonts
	•	Custom branding assets

⚠️ Do not install the Vista Plymouth theme unless explicitly desired.

⸻

Required Compatibility Patch (Plasma 6.5.x)

If SevenTasks fails with:

Cannot assign to non-existent property "windowTitle"

Apply this fix:

sed -i 's/windowTitle/title/g' \
~/.local/share/plasma/plasmoids/io.gitgud.wackyideas.seventasks/contents/ui/Task.qml

Restart Plasma:

kquitapp6 plasmashell
plasmashell --replace &

This resolves the Plasma 6 Task API change.

⸻

Updating AeroThemePlasma

To update the theme:

cd /path/to/aerothemeplasma
git pull

Re-run:

bash compile.sh --wayland
bash install_plasmoids.sh
bash install_plasma_components.sh
bash install_misc_components.sh

install_kwin_components.sh usually does not need to be re-run unless KWin changes.

⸻

Freeze Plasma Updates (Strongly Recommended)

AeroThemePlasma depends on internal Plasma APIs.
Future updates may break the theme stack.

⸻

Step 1 — Edit pacman Configuration

sudo nano /etc/pacman.conf

Under [options], add:

IgnorePkg = plasma-desktop plasma-workspace libplasma kwin plasma5support systemsettings kde-cli-tools kdeplasma-addons breeze breeze-gtk plasma-integration plasma-nm plasma-pa plasma-systemmonitor plasma-browser-integration xdg-desktop-portal-kde sddm-kcm kdecoration aurorae layer-shell-qt libkscreen kscreenlocker kwayland kysguard6-git


⸻

Step 2 — Verify Freeze

sudo pacman -Syu

You should see warnings such as:

warning: plasma-desktop: ignoring package upgrade

Plasma is now frozen.

⸻

Bypassing the Freeze (When You Intentionally Want to Update)

To temporarily update everything, including Plasma:

sudo pacman -Syu --ignore ""

After updating, you may need to reapply the SevenTasks compatibility patch.

⸻

Recovery

If Plasma fails to load:
	1.	Switch to TTY (Ctrl + Alt + F3)
	2.	Remove custom plasmoids:

rm -rf ~/.local/share/plasma/plasmoids/io.gitgud.wackyideas*


	3.	Reset panel layout:

mv ~/.config/plasma-org.kde.plasma.desktop-appletsrc \
   ~/.config/plasma-org.kde.plasma.desktop-appletsrc.bak



Log back in.

⸻

Final Result

You now have:
	•	Windows 7–style Plasma 6 (Wayland)
	•	SevenTasks compatibility with Plasma 6.5.x
	•	KSysGuard restored
	•	Frozen update protection
	•	Reproducible installation workflow

⸻

If you’d like, I can also generate:
	•	A full uninstall guide
	•	A system snapshot / rollback guide
	•	Or a fully automated install script version
