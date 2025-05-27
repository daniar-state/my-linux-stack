# Instructions for Migrating from Windows to Linux for Developers

## My System Configuration

- Motherboard: **Gigabyte B760M H DDR4 (Version F12) UEFI**
- Processor: **Intel(R) Core i7-12700KF (12th) Gen**
  - Core: 12
  - Threads: 20
  - Frequency: 3.6 GHz
  - L1 Cache: 256 KB
  - L2 Cache: 2 MB
  - L3 Cache: 25 MB
- Graphics Card: **NVIDIA GeForce RTX 4060Ti (16 Gb) (HAGS Supported)**
- RAM: **32 Gb (DDR4 Corsair) Min(2133MT/s) Max(3200MT/s)**
- Hard Drive: **SSD 1Tb (M.2 NVMe)**
- Hard Drive: **HDD 1Tb (SATA)**

## Brief Information About My Work

I have been primarily engaged for many years in _software development and programming_, specifically creating modern web applications, mobile applications, planning and building architectures. I mainly focus on _Backend development_ but also spend a considerable amount of time on _Frontend development_, in simpler terms **FullStack**.

### Technologies

- HTML/CSS/SCSS
- JavaScript/TypeScript/JSX/TSX
- NodeJS/NextJS/Vite/NestJS/FastAPI/Python/Django/Fastify/Express/RESTAPI/NPM/GO/React
- Github/Git/DockerCompose/Composer/Gitlab/Jira
- Replit/Cursor/VSCode/JetBrains/v0/ClaudeCode
- SFTP/FTP/SSH
- PostgreSQL/Prisma/MySQL/Kibana/ElasticSearch/Grafana/Prometheus/Logstash
- Jest/e2e/Supertest/Cypress

## Choosing Linux OS

For those who are not professionals in working with the console and Linux systems themselves, the best and fastest choice would be: **Ubuntu OS**. No other Linux systems will be covered in this guide (Arch, Mint, Fedora, Pop, Chrome, ...)

Therefore, I choose in this guide the most stable and fast option for migration.

### Ubuntu 24.04 LTS

Why this system is convenient and beneficial for me to migrate to:

- This is a Linux build that is very stable and optimized, particularly the **LTS** version has long-term stable support (until 2029)
- It has excellent compatibility with graphics cards, in my case with RTX 4060Ti
- This build has an extensive developer community, and you can find answers to any questions when needed
- Very easy transition from Windows and simple installation
- Stable GNOME 46 version
- This build also has good support for Intel 12th generation processors

## Installation Instructions

### Preparation and Image Building

You need to create a bootable **USB** for Ubuntu OS installation.

- Download the image from the official website [Ubuntu 24.04 LTS](https://ubuntu.com/download/desktop)
- Install a tool for creating a bootable flash drive: [Rufus](https://rufus.ie/) or [Balena Etcher](https://etcher.balena.io/) or multi-boot flash drive [Ventoy](https://www.ventoy.net/en/download.html)
- During the bootable flash drive installation, make sure to select your system partition **UEFI** or **Legacy Boot**

### Installing Ubuntu 24.04 LTS

#### Loading the Installer

1. Restart your computer and hold the button to launch the disk selection section **F12** or **DELETE** (Laptops **F10** or **F9**).
2. Select your flash drive from the disk list to boot Live USB.
3. After the Ubuntu system loads, select `Try or Install Ubuntu`.
4. Choose your language: `Russian` or `English`.

#### Installer Configuration

1. Choose installation type: `Something else` (For manual disk partitioning)
2. SSD partitioning (dev/sdb) (In my case, the partitioning shown is):

```
/dev/sdb1  512MB   EFI System Partition   /boot/efi
/dev/sdb2  1Gb     ext4                   /boot
/dev/sdb3  100GB   ext4                   /
/dev/sdb4  400GB   ext4                   /home
/dev/sdb5  50GB    ext4                   /var
/dev/sdb6  100GB   ext4                   /opt
/dev/sdb7  20GB    ext4                   /tmp
/dev/sdb8  32GB    swap                   swap
```

3. HDD partitioning (dev/sda)

```
/dev/sda1   500GB   ext4    /home/projects
/dev/sda2   400GB   ext4    /home/data
/dev/sda3   100GB   ext4    /backup
```

##### Disk Partitioning Explanation (SSD and HDD)

```
SSD - Main System

/boot/efi   - 512MB   (FAT32, EFI bootloader)
/boot       - 1GB     (ext4, Linux kernels)
/           - 100GB   (ext4, system)
/home       - 400GB   (ext4, user files)
/var        - 50GB    (ext4, logs, caches)
/opt        - 100GB   (ext4, additional software)
/tmp        - 20GB    (ext4, temporary files)
swap        - 32GB    (equals RAM for hibernation)
Free        - ~297GB  (reserve for expansion)

HDD - My Data and Projects

/home/projects    - 500GB   (ext4, development projects)
/home/data        - 400GB   (ext4, personal data)
/backup           - 100GB   (ext4, backups)
```

#### Installation Completion

- Select your time zone
- Create a user and password
- Wait for installation completion and system restart

### System Configuration

#### Mandatory First Action

It's essential to update the system and packages that are already in the system:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget git nano htop neofetch -y
```

#### Installing NVIDIA Drivers

```bash
# First add NVIDIA repository
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:graphics-drivers/ppa -y
sudo apt update

# Install recommended driver
sudo ubuntu-drivers autoinstall

# If you need a specific driver version
sudo apt install nvidia-driver-545 -y

# Restart system to apply changes
sudo reboot
```

#### Driver Verification

```bash
nvidia-smi
glxinfo | grep "OpenGL renderer"
```

### Installing Necessary Development Tools (Via Console)

#### Git

```bash
git config --global user.name "daniar-state"
git config --global user.email "daniar@mail.ru"
git config --global init.defaultBranch main
```

#### Node.js and NPM

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - 
sudo apt install nodejs -y

# Check that everything is OK
node --version
npm --version

# Packages
npm install -g yarn pnpm typescript
```

#### Python

```bash
sudo apt install python3 python3-pip python3-venv python-is-python3 -y

# Poetry (dependency management)
curl -sSL https://install.python-poetry.org | python3 -

# Add to PATH (~/.bashrc)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

#### GO

```bash
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz

# Add to PATH
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
source ~/.bashrc
```

#### Flutter

```bash
sudo apt install clang cmake ninja-build pkg-config libgtk-3-dev -y

cd /opt
sudo git clone https://github.com/flutter/flutter.git -b stable
sudo chown -R $USER:$USER /opt/flutter

# Add to PATH
echo 'export PATH="$PATH:/opt/flutter/bin"' >> ~/.bashrc
source ~/.bashrc

flutter doctor
flutter config --enable-linux-desktop
```

#### Docker

```bash
sudo apt install apt-transport-https ca-certificates gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y

# Add user to docker group
sudo usermod -aG docker $USER

# Docker Compose
sudo apt install docker-compose -y
```

### GNOME Environment Configuration

#### GNOME Tweaks and Extensions

```bash
sudo apt install gnome-tweaks gnome-shell-extensions -y
sudo apt install chrome-gnome-shell -y
sudo apt install dconf-editor -y

# GUI extension installer
sudo apt install gnome-shell-extension-manager -y

# Theme tools
sudo apt install sassc optipng inkscape -y

# Useful extensions that add customizations to the system:
# - Dash to Dock - macOS dock
# - User Themes - custom themes
# - GSConnect (Android) - integration
# - Clipboard Indicator - clipboard manager
# - CPU Power Manager - CPU manager
# - Arc Menu - application menu
# - Blur my Shell - background blur
# - Just Perfection - precise interface tuning
# - Vitals - system monitoring in panel
# - Sound Input & Output Device Chooser - audio switching
# - Net speed Simplified - internet speed
```

#### Extension Configuration

##### Dash to Dock

```bash
# dconf
gsettings set org.gnome.shell.extensions.dash-to-dock dock-position BOTTOM
gsettings set org.gnome.shell.extensions.dash-to-dock extend-height false
gsettings set org.gnome.shell.extensions.dash-to-dock dock-fixed false
gsettings set org.gnome.shell.extensions.dash-to-dock autohide true
gsettings set org.gnome.shell.extensions.dash-to-dock intellihide-mode FOCUS_APPLICATION_WINDOWS
gsettings set org.gnome.shell.extensions.dash-to-dock transparency-mode FIXED
gsettings set org.gnome.shell.extensions.dash-to-dock background-opacity 0.8
```

##### GNOME Tweaks

```bash
# Apply theme
gsettings set org.gnome.desktop.interface gtk-theme "WhiteSur-Dark"
gsettings set org.gnome.desktop.wm.preferences theme "WhiteSur-Dark"
gsettings set org.gnome.shell.extensions.user-theme name "WhiteSur-Dark"

# Icons
gsettings set org.gnome.desktop.interface icon-theme "WhiteSur"

# Fonts
gsettings set org.gnome.desktop.interface font-name "Ubuntu 11"
gsettings set org.gnome.desktop.interface document-font-name "Ubuntu 11"
gsettings set org.gnome.desktop.interface monospace-font-name "JetBrains Mono 10"
gsettings set org.gnome.desktop.wm.preferences titlebar-font "Ubuntu Bold 11"

# Font antialiasing
gsettings set org.gnome.desktop.interface font-antialiasing "rgba"
gsettings set org.gnome.desktop.interface font-hinting "slight"
```

##### Panel and Workspace Configuration

```bash
# Show date in panel
gsettings set org.gnome.desktop.interface clock-show-date true
gsettings set org.gnome.desktop.interface clock-show-weekday true

# Workspace configuration
gsettings set org.gnome.mutter dynamic-workspaces false
gsettings set org.gnome.desktop.wm.preferences num-workspaces 4

# Animations (for smoothness)
gsettings set org.gnome.desktop.interface enable-animations true

# Center new windows
gsettings set org.gnome.mutter center-new-windows true
```

#### Themes and Icons

```bash
# Papirus icons
sudo add-apt-repository ppa:papirus/papirus -y
sudo apt update
sudo apt install papirus-icon-theme -y

# Arc theme
sudo apt install arc-theme -y
```

##### My Favorite WhiteSur Theme (macOS-like)

```bash
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git ~/Downloads/WhiteSur-gtk-theme
cd ~/Downloads/WhiteSur-gtk-theme

# Installation
./install.sh -l -c Dark -c Light -t default -N mojave -HD

# WhiteSur icons
git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git ~/Downloads/WhiteSur-icon-theme
cd ~/Downloads/WhiteSur-icon-theme
./install.sh -t default -a
```

##### A Few More Themes for GNOME

```bash
# Orchis (Material Design)
git clone https://github.com/vinceliuice/Orchis-theme.git ~/Downloads/Orchis-theme
cd ~/Downloads/Orchis-theme
./install.sh -t default -c dark -s standard

# Tela icons
git clone https://github.com/vinceliuice/Tela-icon-theme.git ~/Downloads/Tela-icon-theme
cd ~/Downloads/Tela-icon-theme
./install.sh -a
```

#### Fonts

```bash
# Noto fonts (Google)
sudo apt install fonts-noto fonts-noto-color-emoji -y

# Fira Code (for programming)
sudo apt install fonts-firacode -y

# Ubuntu fonts (updated)
sudo apt install fonts-ubuntu -y

# JetBrains Mono (excellent monospace)
wget https://download.jetbrains.com/fonts/JetBrainsMono-2.304.zip -O ~/Downloads/JetBrainsMono.zip
unzip ~/Downloads/JetBrainsMono.zip -d ~/Downloads/JetBrainsMono
sudo mkdir -p /usr/share/fonts/truetype/jetbrains
sudo cp ~/Downloads/JetBrainsMono/fonts/ttf/*.ttf /usr/share/fonts/truetype/jetbrains/
sudo fc-cache -f -v
```

### Useful Applications

#### Media

```bash
sudo apt install vlc gimp inkscape -y
sudo snap install blender --classic
sudo snap install figma-linux
```

#### Office

```bash
sudo snap install libreoffice
sudo snap install discord
sudo snap install telegram-desktop
```

#### System Utilities

```bash
sudo apt install htop btop neofetch tree fd-find ripgrep bat -y
sudo snap install code --classic
sudo snap install postman
```

### System Performance Optimization

#### Zsh and Oh My Zsh

```bash
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Useful plugins for ~/.zshrc:
# plugins=(git node npm docker python golang flutter zsh-autosuggestions zsh-syntax-highlighting)
# git clone https://github.com/zsh-users/zsh-autosuggestions
# git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# Change default shell
chsh -s $(which zsh)
```

##### .zshrc Configuration

```bash
# ~/.zshrc configuration
cat > ~/.zshrc << 'EOF'
# Oh My Zsh
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="robbyrussell"

# Useful plugins
plugins=(
    git
    node
    npm
    docker
    python
    golang
    flutter
    zsh-autosuggestions
    zsh-syntax-highlighting
    sudo
    web-search
    copypath
    copyfile
    copybuffer
    dirhistory
)

source $ZSH/oh-my-zsh.sh

# Development aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias cls='clear'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

# Git aliases
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log --oneline'
alias gd='git diff'
alias gb='git branch'
alias gco='git checkout'

# Development aliases
alias nr='npm run'
alias nrs='npm run start'
alias nrd='npm run dev'
alias nrb='npm run build'
alias ni='npm install'
alias py='python3'
alias pip='pip3'

# Docker aliases
alias dps='docker ps'
alias dpa='docker ps -a'
alias di='docker images'
alias dc='docker-compose'
alias dcu='docker-compose up'
alias dcd='docker-compose down'

# System aliases
alias update='sudo apt update && sudo apt upgrade'
alias install='sudo apt install'
alias search='apt search'
alias cleanup='sudo apt autoremove && sudo apt autoclean'

# Custom paths
export PATH="$HOME/.local/bin:$PATH"
export PATH="$PATH:/usr/local/go/bin"
export PATH="$PATH:/opt/flutter/bin"
export GOPATH="$HOME/go"
export EDITOR="code"

# Node.js
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
EOF
```

##### Custom GNOME Terminal

```bash
# Install Dracula color scheme
git clone https://github.com/dracula/gnome-terminal ~/Downloads/dracula-gnome-terminal
cd ~/Downloads/dracula-gnome-terminal
./install.sh

# Configure terminal profile
PROFILE=$(gsettings get org.gnome.Terminal.ProfilesList default | tr -d "'")
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ font 'JetBrains Mono 12'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ use-system-font false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ audible-bell false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ scrollback-unlimited true
```

#### Swappiness Configuration

```bash
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
```

#### Automatic HDD Mounting

```bash
# Find disk UUIDs
sudo blkid

# Add to /etc/fstab
sudo nano /etc/fstab
# UUID=your-uuid /home/projects ext4 defaults 0 2
# UUID=your-uuid /home/data ext4 defaults 0 2
# UUID=your-uuid /backup ext4 defaults 0 2
```

### Prepared Scripts for Automation

#### System Update Script

```bash
#!/bin/bash
# update-system.sh
echo "Updating Ubuntu LTS system..."
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
sudo snap refresh
flatpak update -y

echo "Updating npm..."
npm update -g

echo "Ubuntu updated!"
```

#### Project Backup Script

```bash
#!/bin/bash
# backup-projects.sh
DATE=$(date +%Y%m%d_%H%M%S)
tar -czf /backup/projects_backup_$DATE.tar.gz /home/projects
echo "Backup created: projects_backup_$DATE.tar.gz"
```

#### Automatic Backup Script

```bash
#!/bin/bash
# ~/.local/bin/backup-home.sh

# Configuration
BACKUP_DIR="/backup/home-backups"
DATE=$(date +%Y%m%d_%H%M%S)
SOURCE_DIR="$HOME"
EXCLUDE_FILE="$HOME/.backup-exclude"

# Create exclusion list
cat > $EXCLUDE_FILE << EOF
.cache/
.local/share/Trash/
.thumbnails/
node_modules/
.git/
__pycache__/
*.tmp
*.log
.npm/
.yarn/
Downloads/
EOF

# Create backup directory
sudo mkdir -p $BACKUP_DIR

# Create archive
tar --exclude-from=$EXCLUDE_FILE \
    -czf "$BACKUP_DIR/home-backup-$DATE.tar.gz" \
    -C $(dirname $SOURCE_DIR) $(basename $SOURCE_DIR)

echo "Backup created: $BACKUP_DIR/home-backup-$DATE.tar.gz"

# Remove old backups (older than 30 days)
find $BACKUP_DIR -name "home-backup-*.tar.gz" -mtime +30 -delete
```

#### Automation via cron

```bash
# Edit crontab
crontab -e

# Add tasks:
# Daily home directory backup at 2:00 AM
0 2 * * * /home/daniar/.local/bin/backup-home.sh

# Weekly system cleanup on Sunday at 3:00 AM
0 3 * * 0 /home/daniar/.local/bin/system-cleanup.sh
```

#### System Health Check Script

```bash
#!/bin/bash
# ~/.local/bin/system-health.sh

echo "=== SYSTEM STATUS REPORT ==="
echo "Date: $(date)"
echo

# Disk usage
echo "=== DISK USAGE ==="
df -h | grep -E '^/dev/'
echo

# RAM usage
echo "=== MEMORY USAGE ==="
free -h
echo

# CPU load
echo "=== CPU LOAD ==="
uptime
echo

# Temperature (if available)
if command -v sensors >/dev/null 2>&1; then
    echo "=== TEMPERATURE ==="
    sensors | grep -E "(Core|Package)"
    echo
fi

# Service status
echo "=== FAILED SERVICES ==="
systemctl --failed
echo

# Journal errors
echo "=== CRITICAL ERRORS (last 24 hours) ==="
journalctl --since "24 hours ago" --priority=0..3 --no-pager | tail -10
echo

# Security updates
echo "=== SECURITY UPDATES ==="
apt list --upgradable 2>/dev/null | grep -i security | wc -l
echo " security updates available"
```

#### Automatic Notifications

```bash
# Install mailutils for notifications
sudo apt install mailutils -y

# Add daily check to crontab
0 9 * * * /home/daniar/.local/bin/system-health.sh | mail -s "System Report $(hostname)" daniar@email.com
```

### Prepared System Configuration Files

#### ~/.bashrc or ~/.zshrc

```bash
# Aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias cls='clear'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'

# Development shortcuts
alias gc='git commit'
alias gs='git status'
alias gp='git push'
alias gl='git log --oneline'
alias nr='npm run'
alias nrs='npm run start'
alias nrb='npm run build'

# Docker shortcuts
alias dps='docker ps'
alias dpa='docker ps -a'
alias di='docker images'
alias dc='docker-compose'

# System shortcuts
alias update='sudo apt update && sudo apt upgrade'
alias install='sudo apt install'
alias search='apt search'

# Custom paths
export PATH="$HOME/.local/bin:$PATH"
export PATH="$PATH:/usr/local/go/bin"
export PATH="$PATH:/opt/flutter/bin"
export GOPATH="$HOME/go"
```

#### Git Aliases

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

### Security and Backup

#### Timeshift Configuration (Snapshots) [GUI]

```bash
sudo apt install timeshift -y

# Run setup
sudo timeshift --setup

# Or via GUI
timeshift-gtk
```

##### Timeshift Configuration and Commands

1. Snapshot type: RSYNC
2. Schedule:
   - Daily: 5 snapshots
   - Weekly: 3 snapshots
   - Monthly: 2 snapshots
3. Include in snapshots: System only without user data (/home)

Commands:

```bash
# Create manual snapshot
sudo timeshift --create --comments "Manual snapshot"

# View snapshots
sudo timeshift --list

# Restore from snapshot
sudo timeshift --restore --snapshot "2025-05-27_14-30-15"

# Delete snapshots
sudo timeshift --delete --snapshot "2025-05-27_10-15-30"
```

#### Emergency GRUB Bootloader Recovery

```bash
# Boot from Live USB
sudo mount /dev/sda1 /mnt # root
sudo mount /dev/sda2 /mnt/boot
sudo mount /dev/sda3 /mnt/boot/efi

# Chroot into system
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo chroot /mnt

# Reinstall GRUB
grub-install /dev/sda
update-grub

exit
sudo umount -R /mnt
reboot
```

#### Firewall Configuration

```bash
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 3000 # Node.js Dev
sudo ufw allow 8000 # Python Django
```

### Additional

#### SSH Key Configuration

```bash
ssh-keygen -t ed25519 -C "daniar@mail.ru"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub # Add to Github/Gitlab
```

#### Windows Virtual Machine (If Required) (KVM/QEMU)

##### Virtualization Check

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo # should be >0

lsmod | grep kvm
```

##### Installation

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y

sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER

sudo reboot
```

##### Network Bridge Configuration

```bash
# Performance optimization
sudo nano /etc/netplan/01-netcfg.yaml

network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: no
  bridges:
    br0:
      interfaces: [enp3s0]
      dhcp4: yes

sudo netplan apply
```

##### VM Creation

```bash
# Create folder for ISO
mkdir -p ~/VMs/iso
cd ~/VMs/iso

# Place Windows ISO in the folder
```

1. Now create VM using Virt-Manager GUI

- Name: Windows-11
- RAM: 8GB
- CPU: 6 Core
- Disk: 100GB (dynamic)
- Network: default NAT

2. Create VM via console

```bash
# Create disk for VM
sudo qemu-img create -f qcow2 ~/VMs/windows11.qcow2 100G

# Create VM
sudo virt-install \
  --name windows11-dev \
  --ram 8192 \
  --vcpus 6 \
  --disk path=~/VMs/windows11.qcow2,format=qcow2,bus=virtio \
  --cdrom ~/VMs/iso/windows11.iso \
  --network network=default \
  --graphics spice,listen=127.0.0.1 \
  --video qxl \
  --boot uefi \
  --os-variant win11
```

##### Windows VM Performance

Install VirtIO drivers:

```bash
wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso -O ~/VMs/iso/virtio-win.iso
```

VM configuration:

```bash
# Edit VM configuration
sudo virsh edit windows11-dev

# Add to <features> section:
<features>
  <acpi/>
  <apic/>
  <hyperv>
    <relaxed state='on'/>
    <vapic state='on'/>
    <spinlocks state='on' retries='8191'/>
    <vendor_id state='on' value='KVM Hv'/>
  </hyperv>
  <kvm>
    <hidden state='on'/>
  </kvm>
  <vmport state='off'/>
</features>

# In <cpu> section:
<cpu mode='host-passthrough' check='none' migratable='on'>
  <topology sockets='1' dies='1' cores='6' threads='1'/>
</cpu>
```

Performance tuning:

```bash
echo 'vm.nr_hugepages = 2048' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

File sharing between your system and VM:

```bash
sudo apt install samba -y
sudo mkdir -p /shared
sudo chmod 777 /shared
```