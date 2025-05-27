# Creating and Deploying Ready-Made System Images

Here I'll show you how to create an image of your system that we've already set up and prepared, so that if you need to reinstall the system or install it on another PC, you can do it in a couple of clicks with everything fully configured.

This allows you, after completely customizing your system, to create an image that can be used when needed, without spending time again to configure the system for yourself.

## The Simplest Way: Clonezilla

### Creating System Image

#### Preparing to Create Our Image

```bash
# System cleanup
sudo apt autoremove -y
sudo apt autoclean
sudo apt clean

# Clear temporary files
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*
sudo rm -rf /var/log/*.log
sudo rm -rf /home/*/.cache/*

# Clear history
history -c
history -w
```

#### Creating USB

1. First download from the official website [Clonezilla Live ISO](https://clonezilla.org/downloads.php)
2. Create bootable USB via dd: `sudo dd if=clonezilla-live-x.x.x-amd64.iso of=/dev/sdX bs=4M status=progress`

#### Image Creation Process:

1. Boot from CloneZilla USB
2. Select mode: device-image
3. Save location: External HDD/SSD
4. Compression: -z9p (max)
5. Option `-fsck-src-part` - filesystem check
6. Option `-rescue` - ignore errors
7. Option `-batch` - automatic mode
8. Select our disk: sda1

CloneZilla example command:

```bash
ocs-sr -q2 -c -j2 -z9p -i 0 -fsck-src-part -rescue -batch savedisk my-ubuntu-golden-image nvme0n1
```

### Image Restoration

1. Boot CloneZilla USB
2. Mode: image-device
3. Select our image
4. Target disk: sda1
5. Confirm

After image restoration:

```bash
sudo update-grub

sudo ubuntu-drivers autoinstall

sudo update-initramfs -u
```

---

## Automation Using Scripts (Advanced Approach)

### Create dotfiles Repository

#### Repository Structure:

```bash
mkdir -p ~/my-ubuntu-setup
cd ~/my-ubuntu-setup

mkdir -p {configs,scripts,themes,fonts}

git init
```

#### Main Installation Script:

```bash
#!/bin/bash
# install.sh

set -e 

echo "Setting up My System..."

# Check privileges
if [[ $EUID -eq 0 ]]; then
   echo "❌ Don't run as root! Use sudo when needed."
   exit 1
fi

install_packages() {
  echo "Installing base packages"
  sudo apt update
  sudo apt upgrade -y

  # tools
  sudo apt install -y \
    curl wget git nano htop tree fd-find ripgrep bat \
    build-essential software-properties-common \
    gnome-tweaks gnome-shell-extensions chrome-gnome-shell \
    dconf-editor timeshift zsh fonts-firacode \
    python3 python3-pip python3-venv python-is-python3
  
  echo "Base packages installed!"
}

install_development_tools() {
  echo "Installing development tools..."

  # Node
  curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
  sudo apt install -y nodejs

  # Docker
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
  sudo usermod -aG docker $USER

  echo "Development tools installed"
}

install_themes_and_fonts() {
  echo "Installing themes and fonts"

  # JetBrains Mono
  wget https://download.jetbrains.com/fonts/JetBrainsMono-2.304.zip -O /tmp/JetBrainsMono.zip
  unzip /tmp/JetBrainsMono.zip -d /tmp/JetBrainsMono
  sudo mkdir -p /usr/share/fonts/truetype/jetbrains
  sudo cp /tmp/JetBrainsMono/fonts/ttf/*.ttf /usr/share/fonts/truetype/jetbrains/
  sudo fc-cache -f -v

  # WhiteSur theme
  git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git /tmp/WhiteSur-gtk-theme
  cd /tmp/WhiteSur-gtk-theme
  ./install.sh -l -c Dark -c Light -t default -N mojave -HD
    
  # WhiteSur icons
  git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git /tmp/WhiteSur-icon-theme
  cd /tmp/WhiteSur-icon-theme
  ./install.sh -t default -a

  echo "Themes and fonts installed"
}

configure_gnome() {
  echo "Configuring GNOME..."

  # Apply themes
  gsettings set org.gnome.desktop.interface gtk-theme "WhiteSur-Dark"
  gsettings set org.gnome.desktop.interface icon-theme "WhiteSur"
  gsettings set org.gnome.desktop.interface font-name "Ubuntu 11"
  gsettings set org.gnome.desktop.interface monospace-font-name "JetBrains Mono 10"
    
  # Interface settings
  gsettings set org.gnome.desktop.interface clock-show-date true
  gsettings set org.gnome.desktop.interface clock-show-weekday true
  gsettings set org.gnome.desktop.interface show-battery-percentage true
  gsettings set org.gnome.mutter center-new-windows true
  
  echo "GNOME configured"
}

setup_zsh() {
  echo "Setting up Zsh..."

  # Oh My Zsh
  sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
  
  # Plugins
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  
  # Configuration
  cp configs/.zshrc ~/.zshrc
  
  # Change shell
  chsh -s $(which zsh)

  echo "Zsh configured"
}

create_directories() {
  echo "📁 Creating working directories..."
    
  mkdir -p ~/projects/{web,mobile,ai,scripts,learning,archive}
  mkdir -p ~/.config/custom-scripts
  mkdir -p ~/.local/bin
  
  echo "✅ Directories created"
}

main() {
  install_packages
  install_development_tools
  install_themes_and_fonts
  configure_gnome
  setup_zsh
  create_directories
  
  echo "🎉 Setup completed!"
  echo "🔄 Reboot to apply all changes"
  echo "📝 Logs saved to ~/setup.log"
}

main 2>&1 | tee ~/setup.log
```

#### Saving Configurations

```bash
# configs/.zshrc
cp ~/.zshrc configs/.zshrc

# configs/gnome-settings.sh
cat > configs/gnome-settings.sh << 'EOF'
#!/bin/bash
# Export GNOME settings
dconf dump /org/gnome/ > configs/gnome-settings.dconf
EOF

# Create settings restoration script
cat > configs/restore-gnome-settings.sh << 'EOF'
#!/bin/bash
# Restore GNOME settings
dconf load /org/gnome/ < configs/gnome-settings.dconf
EOF
```

### Adding Changes to Github

```bash
git add .
git commit -m "Initial Ubuntu config"

git remote add origin https://github.com/username/my-ubuntu-setup.git
git push -u origin main
```

## Automation

1. Configure system using the main methods indicated above
2. Create dotfiles repo with configs
3. Create image in clonezilla

```bash
#!/bin/bash
# ultimate-backup.sh - Create complete backup

echo "🔄 Creating ultimate system backup..."

# 1. Cleanup before backup
sudo apt autoremove -y && sudo apt autoclean
sudo rm -rf /tmp/* /var/tmp/*
history -c

# 2. Export settings
mkdir -p ~/system-backup/{configs,lists}

# List of installed packages
dpkg --get-selections > ~/system-backup/lists/packages.list
flatpak list --app --columns=application > ~/system-backup/lists/flatpak.list
snap list > ~/system-backup/lists/snap.list

# GNOME settings
dconf dump / > ~/system-backup/configs/dconf-settings.ini

# Copy important configs
cp ~/.zshrc ~/system-backup/configs/
cp ~/.gitconfig ~/system-backup/configs/
cp -r ~/.ssh ~/system-backup/configs/ 2>/dev/null || true

# 3. Create configuration archive
tar -czf ~/system-backup-$(date +%Y%m%d).tar.gz ~/system-backup/

echo "✅ Backup ready: ~/system-backup-$(date +%Y%m%d).tar.gz"
echo "🔧 Now create Clonezilla image for complete restoration"
```

### Restoration

```bash
#!/bin/bash
# restore-system.sh - Restore from backup

# 1. Restore packages
sudo apt update
sudo dpkg --set-selections < packages.list
sudo apt-get dselect-upgrade -y

# 2. Restore settings
dconf load / < dconf-settings.ini

# 3. Restore configs
cp configs/.zshrc ~/
cp configs/.gitconfig ~/
cp -r configs/.ssh ~/

echo "✅ System restored from backup!"
```