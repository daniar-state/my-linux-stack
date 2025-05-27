# 📋 История изменений
*Changelog*

Все важные изменения в проекте будут документированы в этом файле.

*All notable changes to this project will be documented in this file.*

Формат основан на [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), и этот проект придерживается [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

*The format is based on Keep a Changelog, and this project adheres to Semantic Versioning.*

## [Unreleased] - В разработке
*Work in progress*

### 🚀 Планируется / Planned
- Поддержка AMD GPU драйверов
- Автоматизированные тесты для скриптов
- Поддержка Ubuntu 24.10
- Интеграция с Ansible для массового развертывания

### 🐛 Известные проблемы / Known Issues
- Конфликт с некоторыми пользовательскими настройками GNOME
- Медленная загрузка больших пакетов на слабом интернете

## [1.0.0] - 2025-05-27

### 🎉 Первый релиз / Initial Release

#### ✨ Добавлено / Added
- **📋 Полная инструкция по установке Ubuntu 24.04 LTS**
  - Детальная разметка дисков (SSD + HDD)
  - Поддержка UEFI системы
  - Настройка для современного железа (Intel 12th gen, RTX 4060Ti)

- **🛠️ Среда разработки**
  - Node.js LTS + npm/yarn/pnpm
  - Python 3 + pip + Poetry
  - Go 1.21.5
  - Flutter с поддержкой Linux desktop
  - Docker + Docker Compose

- **🎨 Кастомизация GNOME**
  - Тема WhiteSur (macOS-подобная)
  - Настройка Dash to Dock
  - Полезные расширения (GSConnect, Clipboard Indicator, etc.)
  - Кастомные шрифты (JetBrains Mono, Noto)

- **⚡ Автоматизированные скрипты**
  - Скрипт автоматической установки всех инструментов
  - Настройка Zsh + Oh My Zsh с плагинами
  - Автоматическое обновление системы
  - Скрипты резервного копирования

- **🔧 Оптимизация системы**
  - Настройка swappiness
  - Автомонтирование дисков
  - Настройка производительности
  - Конфигурация файрвола UFW

- **💾 Система бэкапов**
  - Инструкции по Timeshift
  - Создание образов через CloneZilla
  - Скрипты автоматического бэкапа проектов
  - Восстановление системы

- **🔒 Безопасность**
  - Настройка SSH ключей
  - Конфигурация UFW файрвола
  - Рекомендации по безопасности
  - Восстановление загрузчика GRUB

#### 📚 Документация / Documentation
- Подробный README на русском и английском языках
- Структурированные инструкции по установке
- Примеры конфигурационных файлов
- Руководство по устранению неполадок

#### 🎯 Поддерживаемые технологии / Supported Technologies
- **Frontend**: HTML5, CSS3, JavaScript/TypeScript, React, Next.js
- **Backend**: Node.js, Python, Go, Django, FastAPI, NestJS
- **DevOps**: Docker, Git, GitHub/GitLab, SSH
- **Databases**: PostgreSQL, MySQL, Elasticsearch
- **Tools**: VS Code, JetBrains, Grafana, Prometheus

#### 🖥️ Тестируемые конфигурации / Tested Configurations
- Intel Core i7-12700KF + RTX 4060Ti + 32GB RAM
- Ubuntu 24.04 LTS с GNOME 46
- UEFI загрузка с Secure Boot

## [0.9.0] - 2025-05-20 (Бета)

### ✨ Добавлено / Added
- Базовые скрипты установки
- Первоначальная документация
- Настройки GNOME по умолчанию

### 🐛 Исправлено / Fixed
- Ошибки в скрипте установки Docker
- Конфликты в настройках Git

### ⚠️ Устарело / Deprecated
- Старые скрипты для Ubuntu 22.04

## [0.5.0] - 2025-05-15 (Альфа)

### ✨ Добавлено / Added
- Начальная структура проекта
- Базовые инструкции по установке
- Скрипт настройки Node.js

---

## 🏷️ Типы изменений / Types of Changes

- **✨ Added** - для новых функций
- **🔄 Changed** - для изменений в существующей функциональности
- **⚠️ Deprecated** - для функций, которые скоро будут удалены
- **🗑️ Removed** - для удаленных функций
- **🐛 Fixed** - для исправления ошибок
- **🔒 Security** - для исправлений безопасности

## 📝 Формат версий / Version Format

Мы используем [Semantic Versioning](https://semver.org/):
- **MAJOR.MINOR.PATCH** (например, 1.2.3)
- **MAJOR**: значительные изменения, несовместимые с предыдущими версиями
- **MINOR**: новые функции, совместимые с предыдущими версиями
- **PATCH**: исправления ошибок, совместимые с предыдущими версиями

*We use Semantic Versioning: MAJOR version for incompatible changes, MINOR for backwards-compatible features, PATCH for backwards-compatible bug fixes.*

## 📅 График релизов / Release Schedule

- **🔄 Patch релизы**: По мере необходимости (исправления критических ошибок)
- **📈 Minor релизы**: Ежемесячно (новые функции, улучшения)
- **🚀 Major релизы**: Каждые 6-12 месяцев (значительные изменения)

*Patch releases as needed for critical fixes, minor releases monthly for features, major releases every 6-12 months for significant changes.*

## 🔗 Ссылки / Links

- [🏠 Главная страница проекта](../../)
- [📋 Все релизы](../../releases)
- [🐛 Сообщить об ошибке](../../issues/new?template=bug_report.md)
- [💡 Предложить улучшение](../../issues/new?template=feature_request.md)
- [🤝 Руководство по внесению вклада](CONTRIBUTING.md)

---

**📌 Примечание**: Даты в формате YYYY-MM-DD соответствуют стандарту ISO 8601.

*Note: Dates are in YYYY-MM-DD format following the ISO 8601 standard.*