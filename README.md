# Criage - Высокопроизводительный пакетный менеджер

Criage - это современный пакетный менеджер, написанный на Go, обеспечивающий быструю установку, обновление и управление пакетами с поддержкой различных форматов сжатия.

## Возможности

### Основные функции
- 🚀 **Высокая производительность** - использование быстрых алгоритмов сжатия (Zstandard, LZ4)
- 📦 **Множественные форматы архивов** - поддержка TAR.ZST, TAR.LZ4, TAR.XZ, TAR.GZ, ZIP
- 🔧 **Управление зависимостями** - автоматическое разрешение и установка зависимостей
- 🌐 **Множественные репозитории** - поддержка нескольких источников пакетов
- 🎯 **Кроссплатформенность** - поддержка Linux, macOS, Windows
- ⚡ **Параллельные операции** - многопоточная обработка для ускорения

### Управление пакетами
- Установка и удаление пакетов
- Обновление до последних версий  
- Поиск пакетов в репозиториях
- Просмотр информации о пакетах
- Глобальная и локальная установка

### Разработка пакетов
- Создание новых пакетов из шаблонов
- Сборка пакетов с настраиваемыми скриптами
- Публикация в репозитории
- Хуки жизненного цикла (pre/post install/remove)
- Манифесты сборки

## Установка

### Из исходников

```bash
git clone https://github.com/your-org/criage.git
cd criage
go build -o criage
sudo mv criage /usr/local/bin/
```

### Проверка установки

```bash
criage --version
```

## Использование

### Основные команды

#### Установка пакетов
```bash
# Установить пакет
criage install package-name

# Установить определенную версию
criage install package-name --version 1.2.3

# Глобальная установка
criage install package-name --global

# Установка с dev зависимостями
criage install package-name --dev
```

#### Удаление пакетов
```bash
# Удалить пакет
criage uninstall package-name

# Полное удаление с конфигурацией
criage uninstall package-name --purge
```

#### Обновление пакетов
```bash
# Обновить конкретный пакет
criage update package-name

# Обновить все пакеты
criage update --all
```

#### Поиск и информация
```bash
# Найти пакеты
criage search keyword

# Показать установленные пакеты
criage list

# Показать только устаревшие пакеты
criage list --outdated

# Подробная информация о пакете
criage info package-name
```

### Разработка пакетов

#### Создание нового пакета
```bash
# Создать пакет из базового шаблона
criage create my-package --author "Your Name" --description "Package description"
```

#### Сборка пакета
```bash
# Собрать с настройками по умолчанию
criage build

# Указать формат и уровень сжатия
criage build --format tar.zst --compression 6 --output my-package-1.0.0.tar.zst
```

#### Публикация пакета
```bash
# Опубликовать в репозитории
criage publish --registry https://packages.example.com --token YOUR_TOKEN
```

### Конфигурация

#### Просмотр настроек
```bash
# Показать все настройки
criage config list

# Получить значение конкретной настройки
criage config get cache_path
```

#### Изменение настроек
```bash
# Изменить путь кеша
criage config set cache_path /custom/cache/path

# Изменить уровень сжатия по умолчанию
criage config set compression.level 6

# Изменить количество параллельных потоков
criage config set parallel 8
```

## Структура проекта

```
criage/
├── main.go              # Основная точка входа
├── commands.go          # Реализация CLI команд
├── go.mod               # Go модуль
├── go.sum               # Зависимости
└── pkg/                 # Основные пакеты
    ├── types.go         # Структуры данных
    ├── archive.go       # Работа с архивами
    ├── config.go        # Управление конфигурацией
    ├── package_manager.go        # Основная логика пакетного менеджера
    └── package_manager_helpers.go # Вспомогательные функции
```

## Форматы файлов

### Манифест пакета (criage.yaml)
```yaml
name: my-package
version: 1.0.0
description: Example package
author: Your Name
license: MIT
homepage: https://github.com/user/my-package
repository: https://github.com/user/my-package

keywords:
  - utility
  - tool

dependencies:
  some-lib: ^1.0.0

dev_dependencies:
  test-framework: ^2.0.0

scripts:
  build: make build
  test: make test
  install: make install

files:
  - "bin/*"
  - "lib/*"
  - "README.md"

exclude:
  - "*.log"
  - ".git"
  - "node_modules"

arch:
  - amd64
  - arm64

os:
  - linux  
  - darwin
  - windows

hooks:
  pre_install:
    - echo "Installing package..."
  post_install:
    - echo "Package installed successfully"
```

### Конфигурация сборки (build.json)
```json
{
  "name": "my-package",
  "version": "1.0.0",
  "build_script": "make build",
  "build_env": {
    "CGO_ENABLED": "0",
    "GOOS": "linux"
  },
  "output_dir": "./dist",
  "include_files": ["bin/*", "lib/*"],
  "exclude_files": ["*.log", "test/*"],
  "compression": {
    "format": "tar.zst",
    "level": 3
  },
  "targets": [
    {"os": "linux", "arch": "amd64"},
    {"os": "linux", "arch": "arm64"},
    {"os": "darwin", "arch": "amd64"},
    {"os": "windows", "arch": "amd64"}
  ]
}
```

## Производительность

Criage оптимизирован для максимальной производительности:

- **Zstandard сжатие** - до 3x быстрее чем gzip при лучшем сжатии
- **LZ4 сжатие** - экстремально быстрое сжатие/распаковка
- **Параллельная обработка** - использование всех доступных CPU ядер
- **Умное кеширование** - избежание повторных загрузок
- **Эффективное разрешение зависимостей** - минимизация сетевых запросов

## Сравнение форматов сжатия

| Формат | Скорость сжатия | Скорость распаковки | Размер | Использование |
|--------|----------------|---------------------|--------|---------------|
| tar.zst | Средняя | Очень быстрая | Отличное | По умолчанию |
| tar.lz4 | Очень быстрая | Очень быстрая | Среднее | Быстрые операции |
| tar.xz | Медленная | Средняя | Отличное | Минимальный размер |
| tar.gz | Средняя | Средняя | Хорошее | Совместимость |
| zip | Средняя | Быстрая | Хорошее | Windows совместимость |

## Разработка

### Требования
- Go 1.21 или выше
- Git

### Сборка из исходников
```bash
git clone https://github.com/your-org/criage.git
cd criage
go mod tidy
go build -o criage
```

### Запуск тестов
```bash
go test ./...
```

### Форматирование кода
```bash
go fmt ./...
```

## Лицензия

MIT License - см. файл [LICENSE](LICENSE) для подробностей.

## Вклад в проект

1. Форкните репозиторий
2. Создайте ветку для новой функции (`git checkout -b feature/amazing-feature`)
3. Зафиксируйте изменения (`git commit -m 'Add amazing feature'`)
4. Отправьте в ветку (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

## Поддержка

- 📧 Email: support@criage.io
- 💬 Чат: https://discord.gg/criage
- 🐛 Баги: https://github.com/your-org/criage/issues
- 📖 Документация: https://docs.criage.io