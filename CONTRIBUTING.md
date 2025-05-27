# 🤝 Руководство по внесению вклада в проект
*Contributing Guide*

Спасибо за интерес к улучшению Ubuntu Developer Setup (my-linux-stack)! Мы ценим любой вклад в развитие проекта.

*Thank you for your interest in improving Ubuntu Developer Setup (my-linux-stack)! We appreciate any contribution to the project.*

## 📋 Способы внесения вклада
*Ways to contribute*

### 🐛 Сообщения об ошибках / Bug Reports
- Проверьте [существующие issues](../../issues), чтобы избежать дублирования
- Используйте шаблон для [сообщения об ошибке](.github/ISSUE_TEMPLATE/bug_report.md)
- Приложите как можно больше деталей: версия Ubuntu, железо, логи ошибок

*Check existing issues to avoid duplicates. Use the bug report template and provide as much detail as possible.*

### 💡 Предложения улучшений / Feature Requests
- Используйте шаблон для [предложения функций](.github/ISSUE_TEMPLATE/feature_request.md)
- Объясните, почему эта функция будет полезна другим пользователям
- Предложите возможную реализацию

### 📚 Улучшение документации / Documentation
- Исправления опечаток и грамматических ошибок
- Добавление новых разделов или примеров
- Переводы на другие языки
- Улучшение существующих инструкций

### 💻 Код / Code Contributions
- Исправления ошибок в скриптах
- Новые автоматизированные скрипты
- Оптимизация существующих решений
- Добавление поддержки нового железа/софта

## 🚀 Процесс внесения изменений
*Contribution Process*

### 1. Подготовка / Setup
```bash
# Создайте форк репозитория на GitHub
git clone https://github.com/YOUR_USERNAME/my-linux-stack.git
cd my-linux-stack

# Добавьте upstream remote
git remote add upstream https://github.com/daniar-state/my-linux-stack.git

# Создайте новую ветку для изменений
git checkout -b feature/your-feature-name
# или
git checkout -b fix/bug-description
```

### 2. Внесение изменений / Making Changes
```bash
# Убедитесь, что ваша ветка актуальна
git fetch upstream
git rebase upstream/main

# Внесите изменения и протестируйте их
# Добавьте файлы к коммиту
git add .

# Создайте коммит с понятным описанием
git commit -m "feat: добавил поддержку AMD GPU драйверов"
# или
git commit -m "fix: исправил ошибку в скрипте установки Docker"
```

### 3. Отправка изменений / Submitting Changes
```bash
# Отправьте изменения в ваш форк
git push origin feature/your-feature-name

# Создайте Pull Request на GitHub
```

## ✅ Требования к Pull Request
*Pull Request Requirements*

### 📝 Описание / Description
- **Четкое название**: кратко опишите, что изменяется
- **Подробное описание**: объясните, что и зачем изменяется
- **Тестирование**: опишите, как тестировались изменения
- **Скриншоты**: приложите скриншоты для UI изменений

### 🧪 Тестирование / Testing
- Протестируйте изменения на чистой установке Ubuntu 24.04 LTS
- Убедитесь, что скрипты работают без ошибок
- Проверьте, что документация корректна и понятна

### 📏 Стандарты кодирования / Coding Standards
- **Bash скрипты**: используйте `set -e` для обработки ошибок
- **Документация**: следуйте существующему стилю Markdown
- **Комментарии**: добавляйте комментарии на русском и английском языках
- **Структура**: следуйте существующей организации файлов

### 🔍 Checklist перед отправкой / Pre-submission Checklist
- [ ] Код протестирован на Ubuntu 24.04 LTS
- [ ] Документация обновлена при необходимости
- [ ] Коммиты имеют понятные сообщения
- [ ] Нет конфликтов с основной веткой
- [ ] Соблюдены стандарты кодирования

## 📝 Стиль коммитов / Commit Message Style

Используйте [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Типы коммитов / Commit Types:
- `feat`: новая функциональность
- `fix`: исправление ошибки
- `docs`: изменения в документации
- `style`: форматирование, пропущенные точки с запятой и т.д.
- `refactor`: рефакторинг кода
- `test`: добавление тестов
- `chore`: обновление задач сборки, настроек и т.д.

### Примеры / Examples:
```bash
feat(scripts): добавил автоустановку драйверов AMD
fix(docs): исправил битые ссылки в README
docs(install): обновил инструкции для NVIDIA
chore(git): обновил .gitignore для Python проектов
```

## 🎯 Приоритетные области для вклада
*Priority Areas for Contributions*

### 🔥 Высокий приоритет / High Priority
- 🐛 Исправления критических ошибок в скриптах установки
- 📚 Улучшение документации для новичков
- 🔧 Поддержка нового железа (AMD GPU, Intel ARC и т.д.)
- 🌐 Переводы инструкций на другие языки

### 📈 Средний приоритет / Medium Priority
- ⚡ Оптимизация существующих скриптов
- 🎨 Новые темы и настройки интерфейса
- 🔒 Улучшения безопасности
- 📦 Поддержка новых пакетов и инструментов

### 💡 Низкий приоритет / Low Priority
- 🎮 Настройки для игр
- 🖼️ Красивые скриншоты и демо
- 🔧 Экспериментальные функции
- 🎵 Дополнительные мультимедиа настройки

## 💬 Связь / Communication

### 🗨️ Обсуждения / Discussions
- Используйте [GitHub Discussions](../../discussions) для вопросов и идей
- [Issues](../../issues) для конкретных ошибок и предложений
- [Pull Requests](../../pulls) для готовых изменений

### 📧 Контакты / Contacts
- **Email**: enflyge@mail.ru
- **GitHub**: [@daniar-state](https://github.com/daniar-state)

## 🏆 Признание вклада / Recognition

Все участники будут:
- Добавлены в раздел Contributors
- Упомянуты в CHANGELOG.md
- Получат благодарность в релизных заметках

*All contributors will be added to the Contributors section, mentioned in CHANGELOG.md, and recognized in release notes.*

## ❓ Вопросы / Questions

Если у вас есть вопросы о том, как внести вклад, не стесняйтесь:
- Создать [Discussion](../../discussions)
- Связаться с мейнтейнером по email
- Задать вопрос в существующем Issue

*If you have questions about contributing, feel free to create a Discussion, contact the maintainer via email, or ask in an existing Issue.*

---

**Спасибо за ваш вклад в развитие проекта! 🎉**  
*Thank you for contributing to the project! 🎉*