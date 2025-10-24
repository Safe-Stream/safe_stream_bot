# Safe Stream VPN Bot

**Форк от [remnawave-tg-shop](https://github.com/machka-pasla/remnawave-tg-shop) с кастомизациями для Safe Stream VPN**

## 🚀 Особенности Safe Stream версии

- 🎨 **Брендинг Safe Stream VPN** - полная локализация и переработка интерфейса
- 📖 **Кнопка инструкций** - встроенное руководство по подключению VPN
- 📱 **Live счетчик устройств** - отображение подключенных устройств в реальном времени  
- 🔐 **Терминология "ключ активации"** вместо "ключ подключения"
- 🐳 **Docker конфигурация Safe Stream** - переименованные сервисы

## 📋 Изменения

Все модификации подробно документированы в файле [SAFE_STREAM_MODIFICATIONS.md](./SAFE_STREAM_MODIFICATIONS.md)

## 🔄 Синхронизация с upstream

```bash
# Получить обновления из оригинального репозитория
git fetch upstream
git merge upstream/main

# Разрешить конфликты если есть, затем
git push origin main
```

## 🛠️ Развертывание

```bash
# Клонировать репозиторий
git clone https://github.com/Safe-Stream/safe_stream_bot.git
cd safe_stream_bot

# Создать .env файл на основе .env.example
cp .env.example .env
# Отредактировать .env с вашими настройками

# Запустить с Docker
docker-compose up -d
```

## 📝 Лицензия

MIT License - см. [LICENSE](LICENSE)

---

**🔗 Upstream:** [machka-pasla/remnawave-tg-shop](https://github.com/machka-pasla/remnawave-tg-shop)