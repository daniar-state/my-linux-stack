# 💡 Примеры использования
*Usage Examples*

Этот файл содержит практические примеры использования Ubuntu Developer Setup для различных сценариев разработки.

*This file contains practical examples of using Ubuntu Developer Setup for various development scenarios.*

## 🎯 Сценарии использования
*Use Cases*

### 👨‍💻 Для Frontend разработчика

```bash
# 1. Базовая установка системы
sudo apt update && sudo apt upgrade -y

# 2. Установка Node.js и инструментов
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs -y
npm install -g yarn pnpm typescript @angular/cli create-react-app

# 3. Установка браузеров для тестирования
sudo snap install firefox
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
sudo apt update && sudo apt install google-chrome-stable -y

# 4. VS Code с расширениями
sudo snap install code --classic
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-typescript-next
```

### 🔧 Для Backend разработчика

```bash
# 1. Установка языков программирования
# Python
sudo apt install python3 python3-pip python3-venv -y
curl -sSL https://install.python-poetry.org | python3 -

# Node.js
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs -y

# Go
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc

# 2. Базы данных
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql

# 3. Docker для контейнеризации
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker $USER

# 4. Инструменты мониторинга
sudo apt install htop nethogs iotop -y
```

### 🎨 Для FullStack разработчика

```bash
# Комбинация Frontend + Backend + DevOps инструменты

# 1. Все языки и runtime'ы
./scripts/install-development-tools.sh

# 2. Системы контроля версий
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# 3. Настройка рабочего пространства
mkdir -p ~/projects/{frontend,backend,fullstack,experiments}
mkdir -p ~/projects/fullstack/{ecommerce,blog,portfolio}

# 4. Инструменты API
sudo snap install postman
sudo snap install insomnia
```

## 🛠️ Примеры конфигураций
*Configuration Examples*

### ⚙️ Настройка .zshrc для разработчика

```bash
# ~/.zshrc - Конфигурация для разработчика
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="robbyrussell"

# Плагины для разработки
plugins=(
    git
    node
    npm
    docker
    python
    golang
    zsh-autosuggestions
    zsh-syntax-highlighting
    sudo
    web-search
)

source $ZSH/oh-my-zsh.sh

# Aliases для разработки
alias cls='clear'
alias ll='ls -alF'
alias la='ls -A'
alias ..='cd ..'
alias ...='cd ../..'

# Git shortcuts
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gl='git log --oneline'
alias gd='git diff'

# Development shortcuts
alias nr='npm run'
alias nrs='npm run start'
alias nrd='npm run dev'
alias nrb='npm run build'
alias ni='npm install'
alias py='python3'
alias pip='pip3'

# Docker shortcuts
alias dps='docker ps'
alias dpa='docker ps -a'
alias di='docker images'
alias dc='docker-compose'
alias dcu='docker-compose up'
alias dcd='docker-compose down'

# Project navigation
alias pf='cd ~/projects/frontend'
alias pb='cd ~/projects/backend'
alias pfs='cd ~/projects/fullstack'

# Custom paths
export PATH="$HOME/.local/bin:$PATH"
export PATH="$PATH:/usr/local/go/bin"
export GOPATH="$HOME/go"
export EDITOR="code"

# Node.js version manager
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

### 🔧 Настройка Git для команды

```bash
# Глобальные настройки Git
git config --global user.name "Daniar Developer"
git config --global user.email "enflyge@mail.ru"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"

# Полезные aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
git config --global alias.tree 'log --graph --pretty=oneline --abbrev-commit'

# Настройки для работы с GitHub
git config --global credential.helper store
git config --global pull.rebase false
```

## 📦 Примеры проектов
*Project Examples*

### 🌐 React + Node.js проект

```bash
# 1. Создание структуры проекта
mkdir -p ~/projects/fullstack/my-app
cd ~/projects/fullstack/my-app

# 2. Backend (Node.js + Express)
mkdir backend && cd backend
npm init -y
npm install express cors dotenv helmet morgan
npm install -D nodemon @types/node

# 3. Frontend (React + TypeScript)
cd ..
npx create-react-app frontend --template typescript
cd frontend
npm install axios @types/axios

# 4. Database (PostgreSQL)
sudo -u postgres createdb myapp_db

# 5. Docker configuration
cat > docker-compose.yml << EOF
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "3001:3001"
    environment:
      - NODE_ENV=development
      - DB_HOST=db
    depends_on:
      - db

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=myapp_db
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=mypassword
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
EOF
```

### 🐍 Python + Django проект

```bash
# 1. Создание виртуального окружения
mkdir -p ~/projects/backend/django-api
cd ~/projects/backend/django-api
python3 -m venv venv
source venv/bin/activate

# 2. Установка Django и зависимостей
pip install django djangorestframework python-decouple
pip install psycopg2-binary django-cors-headers

# 3. Создание проекта
django-admin startproject myapi .
cd myapi
python manage.py startapp api

# 4. Настройка settings.py
cat >> settings.py << EOF
# API settings
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ]
}

CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://127.0.0.1:3000",
]
EOF

# 5. Requirements file
pip freeze > requirements.txt
```

## 🔄 Автоматизированные workflows
*Automated Workflows*

### 📋 Ежедневная разработка

```bash
#!/bin/bash
# daily-dev-setup.sh - Ежедневная настройка рабочей среды

echo "🌅 Настройка рабочего дня разработчика..."

# 1. Обновление системы
sudo apt update && sudo apt list --upgradable

# 2. Запуск необходимых сервисов
sudo systemctl start postgresql
sudo systemctl start docker
sudo systemctl start nginx

# 3. Проверка статуса проектов
cd ~/projects
for project in */; do
    if [ -d "$project/.git" ]; then
        echo "📁 Проверка проекта: $project"
        cd "$project"
        git status --porcelain
        cd ..
    fi
done

# 4. Очистка временных файлов
docker system prune -f
npm cache clean --force

echo "✅ Рабочая среда готова!"
```

### 🔄 Еженедельное обслуживание

```bash
#!/bin/bash
# weekly-maintenance.sh - Еженедельное обслуживание

echo "🔧 Еженедельное обслуживание системы..."

# 1. Полное обновление системы
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
sudo apt autoclean

# 2. Обновление snap пакетов
sudo snap refresh

# 3. Обновление Node.js пакетов
npm update -g

# 4. Обновление Python пакетов
pip3 list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip3 install -U

# 5. Docker очистка
docker system prune -a -f
docker volume prune -f

# 6. Очистка логов
sudo journalctl --vacuum-time=7d

# 7. Проверка дискового пространства
df -h
du -sh ~/projects/*

echo "✅ Обслуживание завершено!"
```

## 🎨 Примеры кастомизации GNOME
*GNOME Customization Examples*

### 🌟 Применение темы WhiteSur

```bash
#!/bin/bash
# apply-whitesur-theme.sh

echo "🎨 Применение темы WhiteSur..."

# 1. Установка темы
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git /tmp/WhiteSur-gtk-theme
cd /tmp/WhiteSur-gtk-theme
./install.sh -l -c Dark -c Light -t default -N mojave -HD

# 2. Установка иконок
git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git /tmp/WhiteSur-icon-theme
cd /tmp/WhiteSur-icon-theme
./install.sh -t default -a

# 3. Применение настроек
gsettings set org.gnome.desktop.interface gtk-theme "WhiteSur-Dark"
gsettings set org.gnome.desktop.wm.preferences theme "WhiteSur-Dark"
gsettings set org.gnome.shell.extensions.user-theme name "WhiteSur-Dark"
gsettings set org.gnome.desktop.interface icon-theme "WhiteSur"

# 4. Настройка шрифтов
gsettings set org.gnome.desktop.interface font-name "Ubuntu 11"
gsettings set org.gnome.desktop.interface monospace-font-name "JetBrains Mono 10"

echo "✅ Тема WhiteSur применена!"
```

### 🔧 Настройка расширений GNOME

```bash
#!/bin/bash
# setup-gnome-extensions.sh

echo "🔧 Настройка расширений GNOME..."

# 1. Установка необходимых пакетов
sudo apt install gnome-shell-extensions gnome-shell-extension-manager -y

# 2. Включение расширений
gnome-extensions enable dash-to-dock@micxgx.gmail.com
gnome-extensions enable user-theme@gnome-shell-extensions.gcampax.github.com
gnome-extensions enable clipboard-indicator@tudmotu.com

# 3. Настройка Dash to Dock
gsettings set org.gnome.shell.extensions.dash-to-dock dock-position BOTTOM
gsettings set org.gnome.shell.extensions.dash-to-dock extend-height false
gsettings set org.gnome.shell.extensions.dash-to-dock dock-fixed false
gsettings set org.gnome.shell.extensions.dash-to-dock autohide true

echo "✅ Расширения GNOME настроены!"
```

## 📊 Примеры мониторинга
*Monitoring Examples*

### 🔍 Скрипт мониторинга системы

```bash
#!/bin/bash
# system-monitor.sh - Мониторинг системы разработчика

echo "📊 === СИСТЕМА РАЗРАБОТЧИКА ==="
echo "Дата: $(date)"
echo

# Использование CPU
echo "💻 CPU:"
top -bn1 | grep "Cpu(s)" | awk '{print $2 $3}' | sed 's/%us,/% пользователь, /' | sed 's/%sy/% система/'

# Использование памяти
echo
echo "🧠 Память:"
free -h | awk 'NR==2{printf "Использовано: %s/%s (%.1f%%)\n", $3,$2,$3*100/$2}'

# Диски
echo
echo "💾 Диски:"
df -h | grep -E '^/dev/' | awk '{print $1 ": " $3 "/" $2 " (" $5 ")"}'

# Активные процессы разработки
echo
echo "🔄 Процессы разработки:"
ps aux | grep -E '(node|python|go|docker|code)' | grep -v grep | wc -l | xargs echo "Активных процессов:"

# Сетевые подключения
echo
echo "🌐 Сеть:"
ss -tuln | grep -E ':(3000|8000|8080|5000)' | wc -l | xargs echo "Открытых портов разработки:"

# Git статус проектов
echo
echo "📁 Git проекты:"
find ~/projects -name ".git" -type d | wc -l | xargs echo "Всего Git репозиториев:"

echo
echo "✅ Мониторинг завершен"
```

## 🚀 Шаблоны проектов
*Project Templates*

### 📝 README template для нового проекта

```markdown
# 🚀 Название проекта

> Краткое описание проекта

## 📋 Описание

Подробное описание того, что делает проект.

## 🛠️ Технологии

- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL
- Styling: Tailwind CSS

## 🚀 Быстрый старт

### Требования
- Node.js 18+
- PostgreSQL 15+
- npm или yarn

### Установка
\`\`\`bash
# Клонирование репозитория
git clone https://github.com/username/project.git
cd project

# Установка зависимостей
npm install

# Настройка окружения
cp .env.example .env

# Запуск разработки
npm run dev
\`\`\`

## 📁 Структура проекта

\`\`\`
project/
├── frontend/          # React приложение
├── backend/           # Node.js API
├── database/          # SQL схемы
├── docs/              # Документация
└── docker/            # Docker конфигурация
\`\`\`

## 🤝 Вклад в проект

1. Fork проект
2. Создайте ветку (`git checkout -b feature/amazing-feature`)
3. Commit изменения (`git commit -m 'Add amazing feature'`)
4. Push в ветку (`git push origin feature/amazing-feature`)
5. Создайте Pull Request

## 📄 Лицензия

MIT License - см. [LICENSE](LICENSE)
```

---

## 🔗 Полезные ссылки
*Useful Links*

- 📖 **[README.md](README.md)** - Главная документация
- 📋 **[Инструкции](Instructions/)** - Пошаговые руководства  
- 🤝 **[Contributing](CONTRIBUTING.md)** - Как внести вклад
- 🆘 **[Support](SUPPORT.md)** - Получение помощи
- 👥 **[Authors](AUTHORS.md)** - Авторы и контрибьюторы

---

**💡 Совет**: Адаптируйте эти примеры под свои потребности и рабочий процесс!

*Tip: Adapt these examples to your needs and workflow!*