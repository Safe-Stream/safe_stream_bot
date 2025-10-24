# Safe Stream Bot Modifications

## Полный список всех изменений для Safe Stream VPN бота

### 1. Локализация (Branding)

#### `locales/ru.json`
- Изменен приветственный текст на брендинг Safe Stream
- "Connection key" → "Activation key" (ключ активации)
- Обновлены все тексты подписки на VPN терминологию
- Добавлена кнопка "Инструкции" в интерфейс
- Переработаны тексты уведомлений о продлении подписки

**Ключевые строки для замены:**
```json
"main_menu_greeting": "Здравствуйте, {user_name}!\nДобро пожаловать в Safe Stream — ваш надёжный партнёр для защиты в цифровом мире. 🌍\nМы предлагаем высокоскоростной VPN с глобальной сетью серверов, надёжным шифрованием и полной анонимностью, чтобы обеспечить вашу безопасность и свободу в интернете. 🔒\nЕдинственный, кто должен иметь доступ к вашей коммуникации, это… Вы. 🛡️\nОформите подписку или начните с бесплатного пробного периода. 🚀\nВыберите следующий шаг: ⬇️",
"menu_instructions_button": "📖 Инструкция",
"instructions_text": "📖 <b>Инструкция по подключению Safe Stream VPN</b>\n\n🔗 <b>Шаг 1:</b> Получите ваш ключ активации в разделе \"🔐 Моя подписка\"\n\n📱 <b>Шаг 2:</b> Скачайте приложение для вашей платформы:\n• 📱 <b>Android:</b> <a href=\"https://play.google.com/store/apps/details?id=com.v2ray.ang\">V2rayNG</a>\n• 🍎 <b>iOS:</b> <a href=\"https://apps.apple.com/app/shadowrocket/id932747118\">Shadowrocket</a> (платное)\n• 💻 <b>Windows:</b> <a href=\"https://github.com/2dust/v2rayN/releases\">v2rayN</a>\n• 🐧 <b>Linux:</b> <a href=\"https://github.com/v2fly/v2ray-core\">V2Ray</a>\n• 🖥 <b>macOS:</b> <a href=\"https://github.com/yanue/V2rayU/releases\">V2rayU</a>\n\n⚙️ <b>Шаг 3:</b> Импортируйте ваш ключ активации в приложение\n• Скопируйте ключ из бота\n• Вставьте в приложение или отсканируйте QR-код\n• Подключитесь к серверу\n\n✅ <b>Готово!</b> Теперь ваше соединение защищено Safe Stream VPN\n\n💡 <b>Важно:</b> Один ключ можно использовать на 5 устройствах одновременно\n\n🆘 Нужна помощь? Обратитесь в поддержку!"
```

#### `locales/en.json`
- Аналогичные изменения для английского языка

### 2. Интерфейс (UI Layout)

#### `bot/keyboards/inline/user_keyboards.py`
**Строки 36-43**: Добавлена кнопка "Инструкции"
```python
# Кнопки "Инструкции" и "Рефералы" в одной строке
instructions_button = InlineKeyboardButton(
    text=_(key="menu_instructions_button"),
    callback_data="main_action:instructions")
referral_button = InlineKeyboardButton(
    text=_(key="menu_referral_inline"),
    callback_data="main_action:referral")

builder.row(instructions_button, referral_button)
```

#### `bot/handlers/user/start.py`
**Строки 661-666**: Обработчик кнопки "Инструкции"
```python
elif action == "instructions":
    await callback.answer()
    await callback.message.edit_text(
        text=_(key="instructions_text"),
        reply_markup=get_main_menu_keyboard(i18n),
    )
```

### 3. Функциональность

#### `bot/handlers/user/subscription/core.py`
**Строки 183-220**: Добавлен счетчик устройств в реальном времени
```python
# Получение количества подключенных устройств
current_devices_display = "?"
user_uuid = active.get("user_id")
devices_response = None
if user_uuid:
    try:
        devices_response = await panel_service.get_user_devices(user_uuid)
    except Exception:
        logging.exception("Failed to load devices for user %s", user_uuid)
if devices_response:
    devices_count: Optional[int] = None
    if isinstance(devices_response, dict):
        devices_list = devices_response.get("devices")
        if isinstance(devices_list, list):
            devices_count = len(devices_list)
        # ... дополнительная логика обработки
```

### 4. Docker Configuration

#### `docker-compose.yml`
- Все сервисы переименованы с префиксом `safe-stream-`:
  - `remnawave-bot` → `safe-stream-bot`
  - `remnawave-db` → `safe-stream-db`
  - `remnawave-network` → `safe-stream-network`
  - `remnawave-db-data` → `safe-stream-db-data`

### 5. Файлы для удаления

**Backup файлы локализации (созданы по ошибке на сервере):**
- `locales/ru.json.save`
- `locales/ru.json.save.1`
- `locales/ru.json.save.2`
- `locales/ru.json.save.3`
- `locales/ru.json.save.4`
- `locales/ru.json.save.5`

### 6. План применения изменений

1. **Клонировать форк**
2. **Применить Docker изменения** (docker-compose.yml)
3. **Применить UI изменения** (keyboards и handlers)
4. **Применить функциональные изменения** (subscription core)
5. **Применить локализацию** (ru.json и en.json)
6. **Удалить backup файлы**
7. **Коммит и push изменений**

### 7. Особенности кастомизации

- ✅ Сохранена полная совместимость с оригинальным API
- ✅ Добавлены только Safe Stream специфичные элементы
- ✅ Не затронута логика платежей и панели управления
- ✅ Все изменения обратимы и не конфликтуют с upstream

### 8. Upstream Synchronization

Настроен remote `upstream` для синхронизации с оригинальным репозиторием:
```bash
git remote add upstream https://github.com/machka-pasla/remnawave-tg-shop.git
```

Для обновления от upstream:
```bash
git fetch upstream
git merge upstream/main
```