# 🐧 Ubuntu Developer Setup

> **Полная инструкция по переходу с Windows на Ubuntu 24.04 LTS для разработчика**  
> *Complete guide for transitioning from Windows to Ubuntu 24.04 LTS for developers*

<div align="center">

![Ubuntu Version](https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![GNOME](https://img.shields.io/badge/GNOME-46-4A90E2?style=for-the-badge&logo=gnome&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Maintenance](https://img.shields.io/badge/Maintained-Yes-green?style=for-the-badge)

**[🇷🇺 Русский](#русский) | [🇺🇸 English](#english)**

</div>

---

## 🇷🇺 Русский

### 📋 Описание

Комплексное руководство по установке и настройке Ubuntu 24.04 LTS для FullStack разработчика. Включает детальные инструкции по установке системы, настройке среды разработки, кастомизации интерфейса и автоматизации процессов.

### ✨ Особенности

- 🎯 **Готовая конфигурация** для современного железа (Intel 12th gen, RTX 4060Ti)
- 🛠️ **Полная среда разработки** - Node.js, Python, Go, Flutter, Docker
- 🎨 **macOS-подобный интерфейс** с темой WhiteSur
- ⚡ **Автоматизированные скрипты** установки и настройки
- 💾 **Система бэкапов** и создания образов
- 🔧 **Оптимизация производительности** и безопасности

### 🔧 Поддерживаемые технологии

<div align="center">

| Frontend | Backend | DevOps | Databases |
|----------|---------|---------|-----------|
| ![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white) | ![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white) | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white) | ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=flat&logo=postgresql&logoColor=white) |
| ![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white) | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) | ![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white) | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white) |
| ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black) | ![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white) | ![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white) | ![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=elasticsearch&logoColor=white) |
| ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white) | ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white) | ![GitLab](https://img.shields.io/badge/GitLab-FC6D26?style=flat&logo=gitlab&logoColor=white) | ![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat&logo=prisma&logoColor=white) |
| ![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black) | ![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white) | ![SSH](https://img.shields.io/badge/SSH-4D4D4D?style=flat&logo=gnubash&logoColor=white) | |
| ![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=nextdotjs&logoColor=white) | ![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=flat&logo=nestjs&logoColor=white) | | |

</div>

### 🖥️ Системные требования

```yaml
Материнская плата: Gigabyte B760M H DDR4 (UEFI)
Процессор: Intel Core i7-12700KF (12 cores, 20 threads)
Видеокарта: NVIDIA GeForce RTX 4060Ti (16GB)
ОЗУ: 32GB DDR4 Corsair (3200MT/s)
Диски: 
  - SSD 1TB M.2 NVMe (система)
  - HDD 1TB SATA (данные)
```

### 📁 Структура проекта

```
Instructions/
├── 📄 INSTALL_RU(EN).md           # Основная инструкция по установке
├── 📄 BUILD_MY_OS_RU(EN).md       # Создание и развертка образов
```

### 🚀 Быстрый старт

1. **Скачайте репозиторий**
   ```bash
   git clone https://github.com/daniar-state/my-linux-stack.git
   cd my-linux-stack
   ```

2. **Установите Ubuntu 24.04 LTS** согласно инструкции в `Instructions/INSTALL_RU(EN).md`

### 📖 Разделы руководства

| Раздел | Описание | Файл |
|--------|----------|------|
| 🖥️ **Установка системы** | Подробная инструкция по установке Ubuntu с разметкой дисков | `INSTALL_RU(EN).md` |
| 🎨 **Кастомизация GNOME** | Настройка интерфейса, тем WhiteSur, расширений | `INSTALL_RU(EN).md` |
| 🛠️ **Среда разработки** | Установка всех необходимых инструментов разработчика | `INSTALL_RU(EN).md` |
| ⚡ **Оптимизация** | Настройка производительности и автоматизация | `INSTALL_RU(EN).md` |
| 💾 **Бэкап и образы** | Создание снимков системы и образов для быстрого восстановления | `BUILD_MY_OS_RU(EN).md` |

### 🎯 Готовая среда включает

- **Терминал:** Zsh + Oh My Zsh с полезными плагинами
- **Редакторы:** VS Code, Cursor, JetBrains
- **Контейнеризация:** Docker + Docker Compose
- **Системы контроля версий:** Git + GitHub/GitLab настройки
- **Мониторинг:** Grafana, Prometheus, Elasticsearch
- **Тестирование:** Jest, Cypress, Supertest
- **Безопасность:** UFW, SSH ключи, Timeshift

### 📸 Скриншоты

<details>
<summary>🖼️ Посмотреть интерфейс</summary>

> Здесь будут скриншоты настроенной системы с темой WhiteSur

</details>

### 🤝 Вклад в проект

Мы приветствуем ваши предложения! Пожалуйста:

1. Создайте форк репозитория
2. Создайте ветку для ваших изменений (`git checkout -b feature/amazing-feature`)
3. Зафиксируйте изменения (`git commit -m 'Add amazing feature'`)
4. Отправьте в ветку (`git push origin feature/amazing-feature`)
5. Создайте Pull Request

### 📝 Лицензия

Проект распространяется под лицензией Apache. Подробности в файле [LICENSE](LICENSE).

### 👨‍💻 Автор

**Данияр** - FullStack разработчик
- 📧 Email: enflyge@mail.ru
- 🐙 GitHub: [@daniar-state](https://github.com/daniar-state)

---

## 🇺🇸 English

### 📋 Description

A comprehensive guide for installing and configuring Ubuntu 24.04 LTS for FullStack developers. Includes detailed instructions for system installation, development environment setup, interface customization, and process automation.

### ✨ Features

- 🎯 **Ready-made configuration** for modern hardware (Intel 12th gen, RTX 4060Ti)
- 🛠️ **Complete development environment** - Node.js, Python, Go, Flutter, Docker
- 🎨 **macOS-like interface** with WhiteSur theme
- ⚡ **Automated installation scripts** and configuration
- 💾 **Backup system** and image creation
- 🔧 **Performance optimization** and security

### 🖥️ System Requirements

```yaml
Motherboard: Gigabyte B760M H DDR4 (UEFI)
Processor: Intel Core i7-12700KF (12 cores, 20 threads)
Graphics: NVIDIA GeForce RTX 4060Ti (16GB)
RAM: 32GB DDR4 Corsair (3200MT/s)
Storage: 
  - SSD 1TB M.2 NVMe (system)
  - HDD 1TB SATA (data)
```

### 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/daniar-state/my-linux-stack.git
   cd my-linux-stack
   ```

2. **Install Ubuntu 24.04 LTS** following instructions in `Instructions/INSTALL_RU(EN).md`

### 📖 Guide Sections

| Section | Description | File |
|---------|-------------|------|
| 🖥️ **System Installation** | Detailed Ubuntu installation with disk partitioning | `INSTALL_RU(EN).md` |
| 🎨 **GNOME Customization** | Interface setup, WhiteSur themes, extensions | `INSTALL_RU(EN).md` |
| 🛠️ **Development Environment** | Installation of all necessary developer tools | `INSTALL_RU(EN).md` |
| ⚡ **Optimization** | Performance tuning and automation | `INSTALL_RU(EN).md` |
| 💾 **Backup & Images** | System snapshots and images for quick recovery | `BUILD_MY_OS_RU(EN).md` |

### 🎯 Ready Environment Includes

- **Terminal:** Zsh + Oh My Zsh with useful plugins
- **Editors:** VS Code, Cursor, JetBrains
- **Containerization:** Docker + Docker Compose
- **Version Control:** Git + GitHub/GitLab configurations
- **Monitoring:** Grafana, Prometheus, Elasticsearch
- **Testing:** Jest, Cypress, Supertest
- **Security:** UFW, SSH keys, Timeshift

### 🤝 Contributing

We welcome your suggestions! Please:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Create a Pull Request

### 📝 License

This project is licensed under the Apache License. See [LICENSE](LICENSE) file for details.

### 👨‍💻 Author

**Daniar** - FullStack Developer
- 📧 Email: enflyge@mail.ru
- 🐙 GitHub: [@daniar-state](https://github.com/daniar-state)

---

<div align="center">

**⭐ Если проект оказался полезным, поставьте звездочку!**  
*If this project was helpful, please give it a star!*

</div>