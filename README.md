# Arch Linux + Hyprland + NVIDIA Failure Analysis

**Author:** [sabrinaderose](https://github.com/sabrinaderose)  
**Date:** June 10–12, 2025  
**Repository:** https://github.com/sabrinaderose/hyprland-nvidia-failure-analysis  
**Category:** Linux Desktop Troubleshooting | GPU Debugging | Bootloader Recovery  
**Certifications Aligned:** Indirect support for Linux+, Cloud+, CCNA

---

## 🧠 Project Objective

This project aimed to test whether the **Hyprland Wayland compositor** could run reliably on **Arch Linux** with an **NVIDIA RTX 3060** using proprietary drivers. The goal was to simulate a modern, bare-metal, tiling Wayland workflow — a growing trend in advanced Linux setups.

Despite extensive tuning and documentation review, the system failed to produce a usable GUI. However, this repo serves as a comprehensive diagnostic log and post-mortem analysis. The outcome contributed to critical Linux administration experience: **bootloader tuning, GPU driver troubleshooting, initramfs rebuilding, and hard-learned recovery techniques.**

---

## 🛠️ Hardware & Test Environment

### Primary Test System
- **CPU:** AMD Ryzen 7 5700G  
- **GPU:** NVIDIA GeForce RTX 3060 (12GB VRAM)  
- **RAM:** 32GB DDR4  
- **Storage:** 2x 1TB SSDs (UEFI Boot)  
- **Bootloaders:** GRUB, systemd-boot (used in parallel during testing)  
- **Network:** Wi-Fi  

### Recovery Device
- **Lenovo IdeaPad 3 (Ryzen 5 5500U, 8GB RAM)**  
- Used to reimage ISOs and prepare recovery USBs

---

## 🧪 Software Stack

- **OS:** Arch Linux (Rolling, June 2025)  
- **Kernel:** 6.9.x.arch1-1  
- **Display Stack:** Hyprland + WLRoots (Wayland)  
- **Drivers:** `nvidia`, `nvidia-utils` (proprietary)  
- **Supporting Tools:** `pacstrap`, `mkinitcpio`, `neovim`, `grub`, `systemd-boot`, `networkmanager`, SDDM

---

## 🧭 Objectives & Skills Practiced

### Test Focus
- Assess Hyprland’s compatibility with NVIDIA proprietary drivers under a real installation
- Gain exposure to wlroots-based Wayland compositors and multi-stage Linux boot troubleshooting

### Real-World Job Relevance
- GPU hardware and driver diagnostics  
- Kernel flag and initramfs tuning  
- Logging and recovery from soft-bricked systems  
- Compositor configuration and fallback strategy  
- Multi-boot system experimentation

---

## 📉 Failure Breakdown

### 🚫 What Went Wrong
- Hyprland consistently failed to launch, with:
  - Blank screens or black HDMI signal
  - Crashes in `wlroots` renderer
  - DRM/KMS initialization errors
  - EGL context failures

### 🔧 What Was Attempted
- Kernel boot parameter tuning (`nvidia_drm.modeset=1`, etc.)  
- GRUB → systemd-boot migration  
- Initramfs rebuilt with alternate hooks  
- Nouveau driver fallback (produced visual artifacts, still failed)  
- TTY diagnostics with `journalctl`, SDDM override, and log scraping  
- `hyprland.conf` stripped down to minimal config

### 📂 Key Logs
| File / Path                      | Purpose                          |
|----------------------------------|----------------------------------|
| `/etc/mkinitcpio.conf`           | Initramfs hook configuration     |
| `/boot/grub/grub.cfg`            | GRUB bootloader config snapshot  |
| `~/.config/hypr/hyprland.conf`   | Minimal Hyprland config          |
| `journalctl -b -1`               | Boot error tracking              |

---

## 🧠 Lessons Learned

- **Hyprland is not stable** on NVIDIA proprietary drivers without significant patching
- DRM/KMS setup for NVIDIA remains fragile on Wayland
- Systemd-boot vs GRUB behavior varies significantly in initramfs injection
- GUI crashes often leave **no logs** if the compositor dies early
- In critical deployments, test in a **VM first**, especially for experimental setups

---

## 🧾 STAR Format Summary (Interview-Ready)

**Situation:** Attempted to build a modern Wayland desktop (Hyprland) on Arch Linux using an NVIDIA GPU  
**Task:** Diagnose and resolve GUI startup failures, crashes, and boot instability under Wayland  
**Action:** Rotated bootloaders, rebuilt initramfs, tested kernel flags, configured Hyprland, and performed multi-layer recovery from soft-bricked states  
**Result:** GUI environment remained non-functional; transitioned to KDE Plasma after deep diagnostic review, gaining valuable Linux GPU administration skills

---

## 📌 Outcome

- **Status:** ❌ Failure (GUI never launched successfully)  
- **Next Step Taken:** Full system wipe → KDE Plasma installation for stability  
- **Skills Gained:**
  - Init system debugging  
  - Proprietary GPU driver troubleshooting  
  - Compositor analysis (Wayland vs X11)
  - Bootloader and kernel parameter tuning

---

## 📚 References

- [Arch Wiki – Hyprland & NVIDIA Setup](https://wiki.archlinux.org/title/Hyprland)  
- [Hyprland GitHub – Known issues and config examples](https://github.com/hyprwm/Hyprland)  
- Reddit threads on NVIDIA & Wayland failure modes  
- YouTube: Linux experimenters showing varied success with Hyprland + NVIDIA  
- StackExchange discussions on DRM/DRI and initramfs boot flags

---

## 🧠 Final Note

This was not a polished lab — it was a crash-course in failure, resilience, and **real-world Linux troubleshooting**. As the Linux desktop ecosystem evolves, especially around Wayland and proprietary GPU support, this repo stands as both a warning and a resource for anyone attempting similar setups in production.

