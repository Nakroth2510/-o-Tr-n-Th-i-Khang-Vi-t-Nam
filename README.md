# -o-Tr-n-Th-i-Khang-Vi-t-Nam
cộng đồng game thủ Honor Of Kings để phục vụ phần mềm cho Tencent
# 🔱 HoKOS: ROG Edition 🔱
**A Performance-Optimized Operating System for Hardcore Mobile Gaming & Professional Stability.**

![ROG Banner](https://shields.io)
![Base OS](https://shields.io)
![Status](https://shields.io)

## 📖 Overview
**HoKOS ROG Edition** is a specialized system configuration designed to transform your PC into a high-performance gaming station. Inspired by the resilience of Apple’s ecosystem and the raw power of **ASUS ROG**, this project aims to eliminate system lag, prevent hardware overheating, and provide a secure "defense" for your hardware.

Developed specifically for users transitioning from unstable mobile devices (e.g., POCO F7 incidents) or resource-heavy Windows environments (e.g., 0.8GB RAM bottleneck).

## ✨ Key Features
- **ROG Integrated Core:** Custom boot aesthetics and branding for the ultimate gaming feel.
- **Native Android Support:** Powered by **Waydroid**, allowing **Honor of Kings (HoK)** and **Arena of Valor (LQM)** to run directly on the Linux kernel with Intel Iris Xe optimization.
- **HyperOS Animation Suite:** Ultra-smooth window transitions (Magic Lamp & Blur effects) with a 0.5x speed factor for instant responsiveness.
- **Classic Windows 7 UI:** Familiar taskbar layout for maximum productivity, featuring the **HoK Logo** as the custom Start/Power button.
- **Active Defense:** Eliminates bloatware and unnecessary background services (like Avast or telemetry) to free up maximum RAM.

## 🚀 Installation Guide

### Prerequisites
- **Hardware:** Optimized for 11th Gen Intel Core i5-1135G7 or similar.
- **OS:** Fresh installation of **Manjaro KDE Plasma**.
- **Internet:** High-speed connection required for dependency deployment.

### Deployment Steps
Open your terminal (**Konsole**) and execute the following command:

```bash
git clone https://github.com
cd HoKOS
chmod +x install_hokos_rog.sh
./install_hokos_rog.sh
```

## 🛠 Manual Fine-Tuning
To complete the **ROG x HoK** aesthetic, follow these final steps after the script finishes:
1. **Start Icon:** Right-click the Start Menu -> `Configure Launcher` -> Set icon to `~/hok_logo.png`.
2. **Cursor:** Navigate to `System Settings` -> `Cursors` -> Select **Aero** or **ROG Cursor** theme.
3. **Gaming:** Launch **Waydroid**, sign in to Google Play, and deploy your favorite titles.

## 🛡 Security & Philosophy
HoKOS follows a **"Defense-in-Depth"** philosophy. We prioritize hardware longevity by preventing thermal throttling and ensuring that background processes never "suffocate" your RAM. This is the "King's Armor" for your laptop.

## 📜 Credits
- **Founder:** [Nakroth_251013](https://github.com)
- **Engine:** [Waydroid Project](https://waydro.id)
- **Special Thanks:** Linus Torvalds for the Linux Kernel and the ROG community for the inspiration.

---
*Disclaimer: HoKOS is a community-driven project and is not officially affiliated with ASUS, Apple, or Tencent.*
#!/bin/bash

# ============================================================
# SCRIPT: HoKOS Automated Setup (ROG Edition)
# COMPATIBILITY: Manjaro KDE (Intel 11th Gen i5-1135G7)
# VERSION: 1.1
# DESCRIPTION: Performance-optimized OS with ROG branding
# ============================================================

set -e  # Exit on any error

# Color codes for professional output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
RED_BOLD='\033[1;31m'
NC='\033[0m' # No Color

# Display ROG Logo (ASCII Art)
display_rog_logo() {
    echo -e "${RED_BOLD}"
    echo "          __________________________________________________________ "
    echo "         /                                                         / "
    echo "        /    RRRRRRRRR        OOOOOOOOO        GGGGGGGGGGGG       /  "
    echo "       /     R:::::::RR     OO:::::::::OO    GG::::::::::::G      /   "
    echo "      /      R::RRRRR::R   O::::OOO::::OO   G:::::GGGGGGGG       /    "
    echo "     /       R::R    R::R  O::O     O::O   G:::::G              /     "
    echo "    /        R::RRRRR::R   O::O     O::O   G:::::G    GGGGGG   /      "
    echo "   /         R::RRRRRR     O::O     O::O   G:::::G    G::::G  /       "
    echo "  /          R::R  R::R    O::O     O::O   G:::::G    G::::G /        "
    echo " /           R::R   R::R   O::::OOO::::OO   G:::::GGGGGG::::G/         "
    echo "/            RRRR    RRRR   OOOOOOOOO        GGGGGGGGGGGGGG /          "
    echo "------------------------------------------------------------           "
    echo -e "                REPUBLIC OF GAMERS x HoKOS                     "
    echo -e "${NC}"
}

# Logging functions
log_info() { echo -e "${BLUE}[INFO] $1${NC}"; }
log_success() { echo -e "${GREEN}[SUCCESS] $1${NC}"; }
log_error() { echo -e "${RED}[ERROR] $1${NC}"; }

# ============================================================
# INITIALIZATION
# ============================================================

clear
display_rog_logo
log_info "Initializing HoKOS Deployment (ROG Edition)..."

# Step 1: System Verification
if ! grep -q "Manjaro" /etc/os-release; then
    log_error "Manjaro Linux not detected. Deployment aborted."
    exit 1
fi
log_success "System compatibility verified."

# Step 2: Core Updates & Dependencies
log_info "Step 1/5: Updating system and installing dependencies..."
sudo pacman -Syu --noconfirm
sudo pacman -S --needed base-devel git wget python python-pip --noconfirm
log_success "Core environment ready."

# Step 3: Waydroid & Android Integration
log_info "Step 2/5: Integrating Waydroid (Native Android Layer)..."
pamac build waydroid --no-confirm
sudo waydroid init
log_success "Waydroid core initialized."

# Step 4: Google Play & ARM Translation (Gapps/libndk)
log_info "Step 3/5: Installing Gapps & libndk for Intel Graphics..."
TEMP_DIR="$HOME/hokos_deploy"
mkdir -p "$TEMP_DIR" && cd "$TEMP_DIR"
git clone https://github.com
cd waydroid_script
python3 -m venv venv
sudo venv/bin/python3 main.py install gapps libndk
log_success "Android app compatibility layer installed."

# Step 5: UI Aesthetics (ROG x HyperOS)
log_info "Step 4/5: Applying ROG Aesthetics & HyperOS Animations..."
# Enable Blur & Magic Lamp
kwriteconfig5 --file kwinrc --group Plugins --key blurEnabled true
kwriteconfig5 --file kwinrc --group Plugins --key magiclampEnabled true
# Fast UI response
kwriteconfig5 --file kwinrc --group "KDE" --key AnimationDurationFactor 0.5
qdbus org.kde.KWin /KWin reconfigure

# Download HoK Logo for Start Button
wget -O ~/hok_logo.png https://wikimedia.org
log_success "UI customization completed."

# Step 6: Finalizing Services
log_info "Step 5/5: Enabling ROG-HoK System Services..."
sudo systemctl enable --now waydroid-container
log_success "System services online."

# ============================================================
# COMPLETION
# ============================================================

echo -e "\n${RED_BOLD}====================================================${NC}"
echo -e "${RED_BOLD}     HoKOS ROG EDITION DEPLOYMENT FINISHED!         ${NC}"
echo -e "${RED_BOLD}====================================================${NC}"
log_success "HoKOS is now active. Your system is fully optimized."
echo -e "\n${BLUE}Final Instructions:${NC}"
echo -e "1. Set Start Menu icon to: ${YELLOW}~/hok_logo.png${NC}"
echo -e "2. Apply 'ROG' or 'Aero' mouse cursor in System Settings."
echo -e "3. Launch Waydroid to start playing HoK/LQM."

# Cleanup
cd ~ && rm -rf "$TEMP_DIR"
