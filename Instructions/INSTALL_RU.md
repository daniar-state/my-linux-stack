# Инструкция для перехода на Linux из Windows для разработчика

## Мои системная конфигурация

- Материнская плата: **Gigabyte B760M H DDR4 (Version F12) UEFI**
- Процессор: **Intel(R) Core i7-12700KF (12th) Gen**
  - Core: 12
  - Threads: 20
  - Frequency: 3.6 GHz
  - L1 Cache: 256 KB
  - L2 Cache: 2 MB
  - L3 Cache: 25 MB
- Видеокарта: **NVIDIA GeForce RTX 4060Ti (16 Gb) (HAGS Supported)**
- Оперативная память: **32 Gb (DDR4 Corsair) Min(2133MT/s) Max(3200MT/s)**
- Жесткий диск: **SSD 1Tb (M.2 NVMe)**
- Жесткий диск: **HDD 1Tb (SATA)**

## Небольшая информация о моих занятиях

В основном я занимаюсь уже много лет, _разработкой софта и программированием_ а именно созданием современных веб-приложений, мобильных приложений, планированием и построением архитектур. В основном занимаюсь _Backend разработкой_ но и относительно большую часть времени занимаюсь и _Frontend разработкой_, если проще **FullStack**.

### Технологии

- HTML/CSS/SCSS
- JavaScript/TypeScript/JSX/TSX
- NodeJS/NextJS/Vite/NestJS/FastAPI/Python/DJango/Fastify/Express/RESTAPI/NPM/GO/React
- Github/Git/DockerCompose/Composer/Gitlab/Jiro
- Replit/Cursor/VSCode/JetBrains/v0/ClaudeCode
- SFTP/FTP/SSH
- PostgreSQL/Prisma/MySQL/Kibana/ElasticSearch/Grafana/Prometheus/Logstash
- Jest/e2e/Supertest/Cypress

## Выбор ОС Linux

Для тех кто не профессионал в работе с консолью и самими Linux, то самый лучший выбор и быстрый будет: **Ubuntu OS**. Никаких других Linux-систем в данном руководстве не будет (Arch, Mint, Fedora, Pop, Chrome, ...)

Поэтому я выбираю в данном руководстве, самый стабильный и быстрый вариант для перехода.

### Ubuntu 24.04 LTS

Почему для меня удобна данная система и выгодна для перехода:

- Это сборка Linux, очень стабильная и оптимизированная, в частности версия **LTS** имеет долгосрочную стабильную поддержку (до 2029 года)
- Имеется отличная совместимость с видеокартами, в моем случае с RTX 4060Ti
- У данной сборки имеется обширное сообщество разработчиков, и можно найти ответы на любые вопросы при необходимости
- Очень легкий переход из Windows и простая установка
- Стабильная версия GNOME 46
- Так же у данной сборки хорошая поддержка процессоров Intel 12-го поколения

## Инструкция по установке

### Подготовка и сборка образа

Необходимо создать загрузочный **USB** для установки Ubuntu OS.

- Скачайте образ с оффициального сайта [Ubuntu 24.04 LTS](https://ubuntu.com/download/desktop)
- Установите инструмент для создания загрузочной флешки: [Rufus](https://rufus.ie/ru/) или [Balena Etcher](https://etcher.balena.io/) или мультизагрузочную флешку [Ventoy](https://www.ventoy.net/en/download.html)
- В ходе установки загрузочной флешки обязательно выберите свой раздел системы **UEFI** или **Legacy Boot**

### Установка Ubuntu 24.04 LTS

#### Загрузка установщика

1. Перезапустите свой компьютер, и держите кнопку для запуска раздела выбора дисков **F12** или **DELETE** (Ноутбуки **F10** или **F9**).
2. Выберите в разделе со списком дисков, свою флешку для загрузки Live USB.
3. После загрузки системы Ubuntu, выберите `Try or Install Ubuntu`.
4. Выберите свой язык: `Русский` или `English`.

#### Настройка установщика

1. Выберите тип установки: `Другой вариант` (Для ручной разметки дисков)
2. Разметка SSD (dev/sdb) (В моем случае показывается разметка):

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

3. Разметка HDD (dev/sda)

```
/dev/sda1   500GB   ext4    /home/projects
/dev/sda2   400GB   ext4    /home/data
/dev/sda3   100GB   ext4    /backup
```

##### Пояснение по разметки дисков (SSD и HDD)

```
SSD - Основная система

/boot/efi   - 512MB   (FAT32, EFI загрузчик)
/boot       - 1GB     (ext4, ядра Linux)
/           - 100GB   (ext4, система)
/home       - 400GB   (ext4, пользовательские файлы)
/var        - 50GB    (ext4, логи, кеши)
/opt        - 100GB   (ext4, дополнительное ПО)
/tmp        - 20GB    (ext4, временные файлы)
swap        - 32GB    (равен RAM для гибернации)
Свободное   - ~297GB  (резерв для расширения)

HDD - Мои данные и проекты

/home/projects    - 500GB   (ext4, проекты разработки)
/home/data        - 400GB   (ext4, личные данные)
/backup           - 100GB   (ext4, резервные копии)
```

#### Завершение установки

- Выберите свой часовой пояс
- Создайте пользователя и пароль
- Ожидание завершения установки и перезагрузка системы

### Настройка системы

#### Обязательное первое действие

Обязательно необходимо выполнить обновление системы и пакетов которые уже есть в системе:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl whet git nano htop neofetch -y
```

#### Установка драйверов NVIDIA

```bash
# Добавить для начала репозиторий NVIDIA
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:graphics-drivers/ppa -y
sudo apt update

# Установка рекомендуемого драйвера
sudo ubuntu-drivers autoinstall

# Если необходимо определенную версию драйвера
sudo apt install nvidia-driver-545 -y

# Перезагрузить систему чтоб применить изменения
sudo reboot
```

#### Проверка драйверов

```bash
nvidia-smi
glxinfo | grep "OpenGL renderer"
```

### Установка необходимов инструментов разработки (Через консоль)

#### Git

```bash
git config --global user.name "daniar-state"
git config --global user.email "daniar@mail.ru"
git config --global init.defaultBranch main
```

#### Node.js и NPM

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - 
sudo apt install nodejs -y

# Проверить что все ок
node --version
npm --version

# Пакеты
npm install -g yarn pnpm typescript
```

#### Python

```bash
sudo apt install python3 python3-pip python3-venv python-is-python3 -y

# Poetry (управление зависимостями)
curl -sSL https://install.python-poetry.org | python3 -

# Добавление в PATH (~/.bashrc)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

#### GO

```bash
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz

# Добавление в PATH
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

# Добавление в PATH
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

# Добавление пользователя в группу docker
sudo usermod -aG docker $USER

# Docker Compose
sudo apt install docker-compose -y
```

### Настройка среды GNOME

#### GNOME Tweaks и расширения

```bash
sudo apt install gnome-tweaks gnome-shell-extensions -y
sudo apt install chrome-gnome-shell -y
sudo apt install dconf-editor -y

# Установщик расширение GUI
sudo apt install gnome-shell-extension-manager -y

# Инструменты для тем
sudo apt install sassc optipng inkspace -y

# Полезные расширения которые добавляют кастомизации в систему:
# - Dash to Dock - macOS док
# - User Themes - кастомные темы
# - GSConnect (Android) - интеграция
# - Clipboard Indicator - буфер обмена
# - CPU Power Manager - менеджер CPU
# - Arc Menu - меню приложений
# - Blur my Shell - размытие фона
# - Just Perfection - точная настройка интерфейса
# - Vitals - мониторинг системы в панели
# - Sound Input & Output Device Chooser - переключение аудио
# - Net speed Simplified - скорость интернета
```

#### Конфигурация расширений

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
# Применение темы
gsettings set org.gnome.desktop.interface gtk-theme "WhiteSur-Dark"
gsettings set org.gnome.desktop.wm.preferences theme "WhiteSur-Dark"
gsettings set org.gnome.shell.extensions.user-theme name "WhiteSur-Dark"

# Иконки
gsettings set org.gnome.desktop.interface icon-theme "WhiteSur"

# Шрифты
gsettings set org.gnome.desktop.interface font-name "Ubuntu 11"
gsettings set org.gnome.desktop.interface document-font-name "Ubuntu 11"
gsettings set org.gnome.desktop.interface monospace-font-name "JetBrains Mono 10"
gsettings set org.gnome.desktop.wm.preferences titlebar-font "Ubuntu Bold 11"

# Сглаживание шрифтов
gsettings set org.gnome.desktop.interface font-antialiasing "rgba"
gsettings set org.gnome.desktop.interface font-hinting "slight"
```

##### Настройка панели и рабочих столов

```
# Показать дату в панели
gsettings set org.gnome.desktop.interface clock-show-date true
gsettings set org.gnome.desktop.interface clock-show-weekday true

# Настройка рабочих столов
gsettings set org.gnome.mutter dynamic-workspaces false
gsettings set org.gnome.desktop.wm.preferences num-workspaces 4

# Анимации (для плавности)
gsettings set org.gnome.desktop.interface enable-animations true

# Центрирование новых окон
gsettings set org.gnome.mutter center-new-windows true
```

#### Темы и иконки

```bash
# Иконки Papirus
sudo add-apt-repository ppa:papirus/papirus -y
sudo apt update
sudo apt install papirus-icon-theme -y

# Тема Arc
sudo apt install arc-theme -y
```

##### Любимая моя тема WhiteSur (macOS-подобная)

```bash
git clone https://github/com/vinceliuice/WhiteSur-gtk-theme.git ~/Downloads/WhiteSur-gtk-theme
cd ~/Downloads/WhiteSur-gtk-theme

# Установка
./install.sh -l -c Dark -c Light -t default -N mojave -HD

# Иконки WhiteSur
git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git ~/Downloads/WhiteSur-icon-theme
cd ~/Downloads/WhiteSur-icon-theme
./install.sh -t default -a
```

##### Еще пару тем для GNOME

```bash
# Orchis (Material Design)
git clone https://github.com/vinceliuice/Orchis-theme.git ~/Downloads/Orchis-theme
cd ~/Downloads/Orchis-theme
./install.sh -t default -c dark -s standard

# Tela иконки
git clone https://github.com/vinceliuice/Tela-icon-theme.git ~/Downloads/Tela-icon-theme
cd ~/Downloads/Tela-icon-theme
./install.sh -a
```

#### Шрифты

```bash
# Noto шрифты (Google)
sudo apt install fonts-noto fonts-noto-color-emoji -y

# Fira Code (для программирования)
sudo apt install fonts-firacode -y

# Ubuntu шрифты (обновленные)
sudo apt install fonts-ubuntu -y

# JetBrains Mono (отличный моноширинный)
wget https://download.jetbrains.com/fonts/JetBrainsMono-2.304.zip -O ~/Downloads/JetBrainsMono.zip
unzip ~/Downloads/JetBrainsMono.zip -d ~/Downloads/JetBrainsMono
sudo mkdir -p /usr/share/fonts/truetype/jetbrains
sudo cp ~/Downloads/JetBrainsMono/fonts/ttf/*.ttf /usr/share/fonts/truetype/jetbrains/
sudo fc-cache -f -v
```

### Полезные приложения

#### Медиа

```bash
sudo apt install vlc gimp inkspace -y
sudo snap install blender --classic
sudo snap install figma-linux
```

#### Офис

```bash
sudo snap install libreoffice
sudo snap install discord
sudo snap install telegram-desktop
```

#### Системные утилиты

```bash
sudo apt install htop btop neofetch tree fd-find ripgrep bat -y
sudo snap install code --classic
sudo snap install postman
```

### Оптимизация производительности системы

#### Zsh и Oh My Zsh

```bash
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Полезные плагины для ~/.zshrc:
# plugins=(git node npm docker python golang flutter zsh-autosuggestions zsh-syntax-highlighting)
# git clone https://github.com/zsh-users/zsh-autosuggestions
# git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# Изменение оболочки по умолчанию
chsh -s $(which zsh)
```

##### Настройка .zshrc

```bash
# ~/.zshrc конфигурация
cat > ~/.zshrc << 'EOF'
# Oh My Zsh
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="robbyrussell"

# Полезные плагины
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

# Aliases для разработки
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

##### Кастом GNOME Terminal

```bash
# Установка цветовой схемы Dracula
git clone https://github.com/dracula/gnome-terminal ~/Downloads/dracula-gnome-terminal
cd ~/Downloads/dracula-gnome-terminal
./install.sh

# Настройка профиля терминала
PROFILE=$(gsettings get org.gnome.Terminal.ProfilesList default | tr -d "'")
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ font 'JetBrains Mono 12'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ use-system-font false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ audible-bell false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$PROFILE/ scrollback-unlimited true
```

#### Настройка swappiness

```bash
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
```

#### Автоматическое монтирование HDD

```bash
# Найди UUID дисков
sudo blkid

# Добавь в /etc/fstab
sudo nano /etc/fstab
# UUID=твой-uuid /home/projects ext4 defaults 0 2
# UUID=твой-uuid /home/data ext4 defaults 0 2
# UUID=твой-uuid /backup ext4 defaults 0 2
```

### Заготовленные скрипты для автоматизации

#### Скрипт для обновления системы

```bash
#!/bin/bash
# update-system.sh
echo "Обновление системы Ubuntu LTS..."
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
sudo snap refresh
flatpak update -y

echo "Обновление npm..."
npm update -g

echo "Ubuntu актуализирована!"
```

#### Скрипт резервного копирования ваших проектов

```bash
#!/bin/bash
# backup-projects.sh
DATE=$(date +%Y%m%d_%H%M%S)
tar -czf /backup/projects_backup_$DATE.tar.gz /home/projects
echo "Backup создан: projects_backup_$DATE.tar.gz"
```

#### Скрипт автоматического бэкапа

```bash
#!/bin/bash
# ~/.local/bin/backup-home.sh

# Конфиг
BACKUP_DIR="/backup/home-backups"
DATE=$(date +%Y%m%d_%H%M%S)
SOURCE_DIR="$HOME"
EXCLUDE_FILE="$HOME/.backup-exclude"

# Создание списка исключений
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

# Создание директории бэкапа
sudo mkdir -p $BACKUP_DIR

# Создание архива
tar --exclude-from=$EXCLUDE_FILE \
    -czf "$BACKUP_DIR/home-backup-$DATE.tar.gz" \
    -C $(dirname $SOURCE_DIR) $(basename $SOURCE_DIR)

echo "Backup создан: $BACKUP_DIR/home-backup-$DATE.tar.gz"

# Удаление старых бэкапов (старше 30 дней)
find $BACKUP_DIR -name "home-backup-*.tar.gz" -mtime +30 -delete
```

#### Автоматизация через cron

```bash
# Редактирование crontab
crontab -e

# Добавление задач:
# Ежедневный бэкап домашней директории в 2:00
0 2 * * * /home/daniar/.local/bin/backup-home.sh

# Еженедельная очистка системы в воскресенье в 3:00
0 3 * * 0 /home/daniar/.local/bin/system-cleanup.sh
```

#### Скрипт проверки здоровья системы

```bash
#!/bin/bash
# ~/.local/bin/system-health.sh

echo "=== ОТЧЕТ О СОСТОЯНИИ СИСТЕМЫ ==="
echo "Дата: $(date)"
echo

# Использование диска
echo "=== ИСПОЛЬЗОВАНИЕ ДИСКОВ ==="
df -h | grep -E '^/dev/'
echo

# Использование RAM
echo "=== ИСПОЛЬЗОВАНИЕ ПАМЯТИ ==="
free -h
echo

# Загрузка CPU
echo "=== ЗАГРУЗКА CPU ==="
uptime
echo

# Температура (если доступно)
if command -v sensors >/dev/null 2>&1; then
    echo "=== ТЕМПЕРАТУРА ==="
    sensors | grep -E "(Core|Package)"
    echo
fi

# Состояние сервисов
echo "=== НЕУДАЧНЫЕ СЕРВИСЫ ==="
systemctl --failed
echo

# Ошибки в журнале
echo "=== КРИТИЧЕСКИЕ ОШИБКИ (последние 24 часа) ==="
journalctl --since "24 hours ago" --priority=0..3 --no-pager | tail -10
echo

# Обновления безопасности
echo "=== ОБНОВЛЕНИЯ БЕЗОПАСНОСТИ ==="
apt list --upgradable 2>/dev/null | grep -i security | wc -l
echo " доступно обновлений безопасности"
```

#### Автоматическое уведомление

```bash
# Установка mailutils для уведомлений
sudo apt install mailutils -y

# Добавление в crontab ежедневной проверки
0 9 * * * /home/daniar/.local/bin/system-health.sh | mail -s "Отчет системы $(hostname)" daniar@email.com
```

### Заготовленные конфигурационные файлы системы

#### ~/.bashrc  или ~/.zshrc

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

#### Git aliases

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

### Безопасность и резервное копирование

#### Настройка Timeshift (снимки) [GUI]

```bash
sudo apt install timeshift -y

# Запуск настройки
sudo timeshift --setup

# Или через GUI
timeshift-gtk
```

##### Конфигурация и команды Timeshift

1. Тип снимков: RSYNC
2. Расписание:
   - Ежедневно: 5 снимков
   - Еженедельно: 3 снимка
   - Ежемесячно: 2 снимка
3. Включите в снимки: Только систему без пользовательских данных (/home)

Команды:

```bash
# Создание снимка вручную
sudo timeshift --create --comments "Ручной снимок"

# Просмотр
sudo timeshift --list

# Восстановление из снимка
sudo timeshift --restore --snapshot "2025-05-27_14-30-15"

# Удаление снимков
sudo timeshift --delete -snapshot "2025-05-27_10-15-30"
```

#### Экстренное восстановление загрузчика GRUB

```bash
# Загрузка с Live USB
sudo mount /dev/sda1 /mnt # корневой
sudo mount /dev/sda2 /mnt/boot
sudo mount /dev/sda3 /mnt/boot/efi

# Chroot в систему
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo chroot /mnt

# Переустановка
grub-install /dev/sda
update-grub

exit
sudo umount -R /mnt
reboot
```

#### Настройка файрвола

```bash
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 3000 # Node.js Dev
sudo ufw allow 8000 # Python Django
```

### Дополнительно

#### Настройка SSH ключей

```bash
ssh-keygen -t ed25519 -C "daniar@mail.ru"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub # В Github/Gitlab
```

#### Виртуальная машина с Windows (Если требуется) (KVM/QEMU)

##### Проверка виртуализации

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo # >0

lsmod | grep kvm
```

##### Установка

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y

sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER

sudo reboot
```

##### Настройка сетевого моста

```bash
# Оптимизация производительности
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

##### Создание VM

```bash
# Создать папку для ISO
mkdir -p ~/VMs/iso
cd ~/VMs/iso

# Положить в папку iso образ windows
```

1. Теперь создадим VM используя Virt-Manager GUI

- Имя: Windows-11
- RAM: 8GB
- CPU: 6 Core
- Диск: 100GB (динамический)
- Сеть: default NAT

2. Создание VM через консоль

```bash
# Создание диска для VM
sudo qemu-img create -f qcow2 ~/VMs/windows11.qcow2 100G

# Создание VM
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

##### Производительность Windows VM

Установка VirtIO драйверов:

```bash
wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso -O ~/VMs/iso/virtio-win.iso
```

Настройка самой VM:

```bash
# Редактирование конфигурации VM
sudo virsh edit windows11-dev

# Добавить в секцию <features>:
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

# В секции <cpu>:
<cpu mode='host-passthrough' check='none' migratable='on'>
  <topology sockets='1' dies='1' cores='6' threads='1'/>
</cpu>
```

Настройка производительности:

```bash
echo 'vm.nr_hugepages = 2048' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

Шаринг файлов между своей системой и виртуалкой:

```bash
sudo apt install samba -y
sudo mkdir -p /shared
sudo chmod 777 /shared
```