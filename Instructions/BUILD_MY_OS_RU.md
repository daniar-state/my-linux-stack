# Создание и развертка готового образа

Здесь я покажу сразу, как создать образ своей системы, которые мы уже сделали, подготовили для себя, чтобы в случае если нам необходимо будет переустановить систему, или установить на другой ПК, чтобы сделать это в пару кликов, и сразу полностью настроенную.

Это позволяет после полной настройки системы под себя, создать образ, который можно будет использовать если это потребуется, не тратя снова время для настройки системы под себя.

## Самый простой способ Clonezilla

### Создание образа системы

#### Подготовка к созданию нашего образа

```bash
# Очистка системы
sudo apt autoremove -y
sudo apt autoclean
sudo apt clean

# Очистка временных файлов
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*
sudo rm -rf /var/log/*.log
sudo rm -rf /home/*/.cache/*

# Очистка истории
history -c
history -w
```

#### Создание USB

1. Для начала скачай с официального сайта [Clonezilla Live ISO](https://clonezilla.org/downloads.php)
2. Создай загрузочный USB через dd: `sudo dd if=clonezilla-live-x.x.x-amd64.iso of=/dev/sdX bs=4M status=progress`

#### Процесс создания образа:

1. Загрузиться с CloneZilla USB
2. Выбрать режим: device-image
3. Место сохранения: Внешний HDD/SSD
4. Сжатие: -z9p (макс)
5. Опция `-fsck-src-part` - проверка файловой системы
6. Опция `-rescue` - игнор ошибок
7. Опция `-batch` - автоматический режим
8. Выбор нашего диска: sda1

Пример команды CloneZilla:

```bash
ocs-sr -q2 -c -j2 -z9p -i 0 -fsck-src-part -rescue -batch savedisk my-ubuntu-golden-image nvme0n1
```

### Восстановление образа

1. Загрузка CloneZilla USB
2. Режим: image-device
3. Выбрать наш образ
4. Целевой диск: sda1
5. Подтвердить

После восстановления образа:

```bash
sudo update-grub

sudo ubuntu-drivers autoinstall

sudo update-initramfs -u
```

___

## Автоматизация используя скрипты (Тяжелый подход)

### Создать репозиторий dotfiles

#### Структура репозитория:

```bash
mkdir -p ~/my-ubuntu-setup
cd ~/my-ubuntu-setup

mkdir -p {configs, scripts, themes, fonts}

git init
```

#### Главный скрипт установки:

```bash
#!/bin/bash
# install.sh

set -e 

echo "Настройка My System..."

# Проверяем права
if [[ $EUID -eq 0 ]]; then
   echo "❌ Не запускай от root! Используй sudo когда нужно."
   exit 1
fi

install_packages() {
  echo "Установка базовых пакетов"
  sudo apt update
  sudo apt upgrade -y

  # инструменты
  sudo apt install -y \
    curl wget git nano htop tree fd-find ripgrep bat \
    build-essential software-properties-common \
    gnome-tweaks gnome-shell-extensions chrome-gnome-shell \
    dconf-editor timeshift zsh fonts-firacode \
    python3 python3-pip python3-venv python-is-python3
  
  echo "Базовые пакеты установлены!"
}

install_development_tools() {
  echo "Установка инструментов для разработки..."

  # Node
  curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
  sudo apt install -y nodejs

  # Docker
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
  sudo usermod -aG docker $USER

  echo "Инструменты для разработки установлены"
}

install_themes_and_fonts() {
  echo "Установка тем и шрифтов"

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

  echo "Темы и шрифты установлены"
}

configure_gnome() {
  echo "Настройка GNOME..."

  # Применить темы
  gsettings set org.gnome.desktop.interface gtk-theme "WhiteSur-Dark"
  gsettings set org.gnome.desktop.interface icon-theme "WhiteSur"
  gsettings set org.gnome.desktop.interface font-name "Ubuntu 11"
  gsettings set org.gnome.desktop.interface monospace-font-name "JetBrains Mono 10"
    
  # Настройки интерфейса
  gsettings set org.gnome.desktop.interface clock-show-date true
  gsettings set org.gnome.desktop.interface clock-show-weekday true
  gsettings set org.gnome.desktop.interface show-battery-percentage true
  gsettings set org.gnome.mutter center-new-windows true
  
  echo "GNOME настроен"
}

setup_zsh() {
  echo "Настройка Zsh..."

  # Oh My Zsh
  sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
  
  # Плагины
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/pluginszsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/pluginszsh-syntax-highlighting
  
  # Конфигурация
  cp configs/.zshrc ~/.zshrc
  
  # Смена оболочки
  chsh -s $(which zsh)

  echo "Zsh настроен"
}

create_directories() {
  echo "📁 Создание рабочих директорий..."
    
  mkdir -p ~/projects/{web,mobile,ai,scripts,learning,archive}
  mkdir -p ~/.config/custom-scripts
  mkdir -p ~/.local/bin
  
  echo "✅ Директории созданы"
}

main() {
  install_packages
  install_development_tools
  install_themes_and_fonts
  configure_gnome
  setup_zsh
  create_directories
  
  echo "🎉 Настройка завершена!"
  echo "🔄 Перезагрузись для применения всех изменений"
  echo "📝 Логи сохранены в ~/setup.log"
}

main 2>&1 | tee ~/setup.log
```

#### Сохранение конфигураций

```bash
# configs/.zshrc
cp ~/.zshrc configs/.zshrc

# configs/gnome-settings.sh
cat > configs/gnome-settings.sh << 'EOF'
#!/bin/bash
# Экспорт настроек GNOME
dconf dump /org/gnome/ > configs/gnome-settings.dconf
EOF

# Создание скрипта восстановления настроек
cat > configs/restore-gnome-settings.sh << 'EOF'
#!/bin/bash
# Восстановление настроек GNOME
dconf load /org/gnome/ < configs/gnome-settings.dconf
EOF
```

### Добавление изменений в Github

```bash
git add .
git commit -m "Initial Ubuntu config"

git remote add origin https://github/com/username/my-ubuntu-setup.git
git push -u origin main
```

## Автоматизация

1. Настроить систему по основным способам указанным выше
2. Создать dotfiles репо с конфигами
3. Сделать образ в clonezilla

```bash
#!/bin/bash
# ultimate-backup.sh - Создание полного бэкапа

echo "🔄 Создание ultimate backup системы..."

# 1. Очистка перед бэкапом
sudo apt autoremove -y && sudo apt autoclean
sudo rm -rf /tmp/* /var/tmp/*
history -c

# 2. Экспорт настроек
mkdir -p ~/system-backup/{configs,lists}

# Список установленных пакетов
dpkg --get-selections > ~/system-backup/lists/packages.list
flatpak list --app --columns=application > ~/system-backup/lists/flatpak.list
snap list > ~/system-backup/lists/snap.list

# Настройки GNOME
dconf dump / > ~/system-backup/configs/dconf-settings.ini

# Копирование важных конфигов
cp ~/.zshrc ~/system-backup/configs/
cp ~/.gitconfig ~/system-backup/configs/
cp -r ~/.ssh ~/system-backup/configs/ 2>/dev/null || true

# 3. Создание архива конфигураций
tar -czf ~/system-backup-$(date +%Y%m%d).tar.gz ~/system-backup/

echo "✅ Бэкап готов: ~/system-backup-$(date +%Y%m%d).tar.gz"
echo "🔧 Теперь создай Clonezilla образ для полного восстановления"
```

### Восстановление

```bash
#!/bin/bash
# restore-system.sh - Восстановление из бэкапа

# 1. Восстановление пакетов
sudo apt update
sudo dpkg --set-selections < packages.list
sudo apt-get dselect-upgrade -y

# 2. Восстановление настроек
dconf load / < dconf-settings.ini

# 3. Восстановление конфигов
cp configs/.zshrc ~/
cp configs/.gitconfig ~/
cp -r configs/.ssh ~/

echo "✅ Система восстановлена из бэкапа!"
```