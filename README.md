# 🧠 PHASE 1: Failed Arch + Hyprland + NVIDIA Setup

> [!NOTE]
> This repository documents a failed attempt at installing Arch Linux with Hyprland on a system using an NVIDIA GPU. The insights gained are invaluable for future installs and troubleshooting.

---

## 📘 Summary

Attempted to install **Arch Linux** with **Hyprland** (a tiling Wayland compositor) on a machine with an **NVIDIA GPU**. This setup led to persistent boot and display issues due to known incompatibilities between **Hyprland** and **proprietary NVIDIA drivers**. For more information regarding Hyprland with Nivida, please review the [`NVidia page on the Hyprland Wiki, here`](https://wiki.hypr.land/Nvidia/).

---

## 🧪 Commands Used

```bash
# Base install with NVIDIA and Hyprland
pacstrap /mnt base linux linux-firmware linux-headers neovim networkmanager grub efibootmgr sudo git hyprland nvidia nvidia-utils nvidia-settings

# Bootloader and initramfs
bootctl install
mkinitcpio -P

# Kernel flags and modules
echo "options nvidia_drm modeset=1" | sudo tee /etc/modprobe.d/nvidia-drm.conf
```

---

## 🪲 Errors Encountered

```text
- Black screen at boot
- Frozen or unresponsive shell after login
- No HDMI output (Wayland/NVIDIA conflict)
- System stuck in login loop
```

---

## 🖥️ Simulated Terminal Output

```text
[    0.000056] hyprland: GPU crashed on frame init
[    0.000201] wlroots: No compatible DRM device found
[    0.000302] wlroots: Failed to initialize renderer
[    0.000403] hyprland: Fatal error: unable to start display server
```

> [!WARNING]
> These errors indicate that `wlroots` could not initialize due to GPU/driver conflict. Wayland compositors like Hyprland are especially sensitive to these issues with NVIDIA hardware.

---

## 🛠️ Fixes Attempted

```bash
# Rebuild initramfs
mkinitcpio -P

# Set NVIDIA DRM kernel parameter
grub-mkconfig -o /boot/grub/grub.cfg

# Attempted Nouveau fallback (failed)
sudo pacman -Rns nvidia nvidia-utils nvidia-settings
sudo pacman -S xf86-video-nouveau
```

---

## 📛 Failed Alternatives

```bash
# Manually edited Hyprland config
vim ~/.config/hypr/hyprland.conf

# Enabled SDDM display manager
systemctl enable sddm

# Added kernel boot flags
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nvidia_drm.modeset=1"
```

---

## 💡 Lessons Learned

- 🚫 Hyprland and NVIDIA proprietary drivers do not play nicely without major configuration workarounds.
- 🕶️ GUI failures in Wayland offer limited visibility for debugging.
- ✅ Tiling window managers like Hyprland are best suited for **AMD** or **Intel integrated graphics**.

---

## ✅ Recommendations

> [!TIP]
> Save yourself hours of frustration by starting with more compatible setups when learning or testing on bare metal. I would recommend, if available, setting up a virtual-machine, and trial-error until a stable build is reached.

- Use **KDE/X11** or **GNOME/Wayland** with NVIDIA for improved compatibility.
- Never test experimental configs on production hardware.
- Always keep logs, configs, and recovery media backed up.
- Store critical configs and crash logs in version control (e.g., GitHub) for audit/recovery purposes.

---

[^1]: For a breakdown of the next steps taken after this failure, see the [`windows-iso-nuke-recovery`](https://github.com/sabrinaderose/windows-iso-nuke-recovery) repository.
