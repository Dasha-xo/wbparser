# 🤖 Wildberries Feedback Bot

Автоматический бот для ответов на отзывы Wildberries с использованием AI и Telegram уведомлениями.

##  Возможности

- **Автоматические ответы** на отзывы Wildberries
- **AI-генерация** ответов (Yandex GPT, GigaChat или локальные шаблоны)
- **Telegram уведомления** о новых ответах и ежедневная статистика
- **Бесплатный хостинг** через GitHub Actions
- **Автозапуск** каждые 30 минут
- **Гибкая конфигурация** через переменные окружения

## Содержание

- [Быстрый старт](#Быстрый-старт)
- [Настройка](#настройка)
- [AI провайдеры](#ai-провайдеры)
- [Telegram уведомления](#telegram-уведомления)
- [Локальная разработка](#локальная-разработка)
- [Структура проекта](#структура-проекта)
- [Мониторинг](#мониторинг)
- [FAQ](#faq)

## Быстрый старт

### 1. Клонирование репозитория

```bash
git clone https://github.com/ваш-username/wb-parser.git
cd wb-parser
```
### 2. Настройка окружения

Создайте файл `.env` в корне проекта:
```bash
# Wildberries API
WB_API_KEY=your_wildberries_api_key_here
SUPPLIER_ID=your_supplier_id_here

# Настройки приложения
CHECK_INTERVAL=30
TEST_MODE=false

# AI Провайдер (free, russian, fallback)
AI_PROVIDER=free

# Yandex GPT (если используете)
YANDEX_API_KEY=your_yandex_api_key_here
YANDEX_FOLDER_ID=your_yandex_folder_id_here

# GigaChat (если используете)
GIGACHAT_API_KEY=your_gigachat_api_key_here

# Telegram уведомления
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
TELEGRAM_CHAT_ID=your_telegram_chat_id_here
```
### 3. Установка зависимостей

```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
```
или
```bash
venv\Scripts\activate     # Windows
pip install -r requirements.txt
```
### 4. Проверка работы

```bash
python check.py
python scripts/status.py
```
### 5. Запуск бота

```bash
python main.py
```
### Настройка
#### 1. Создайте репозиторий на GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/ваш-username/wb-bot.git
git push -u origin main
```
#### 2. Настройте Secrets в GitHub

Перейдите в ваш репозиторий на GitHub:\
***Settings → Secrets and variables → Actions → New repository secret***

Добавьте следующие secrets:


| Secret | Описание | Пример |
|--------|-----------|---------|
| `WB_API_KEY` | Ключ API Wildberries | `eyJhbGciOiJ...` |
| `SUPPLIER_ID` | ID поставщика | `123456` |
| `AI_PROVIDER` | Провайдер AI | `free` |
| `YANDEX_API_KEY` | Ключ Yandex Cloud | `AQVN...` |
| `YANDEX_FOLDER_ID` | Folder ID Yandex | `b1g...` |
| `TELEGRAM_BOT_TOKEN` | Токен Telegram бота | `123456:ABC-DEF...` |
| `TELEGRAM_CHAT_ID` | Ваш Chat ID в Telegram | `123456789` |

#### 3. Workflow автоматически запустится

После настройки secrets бот будет автоматически:

- ✅ Запускаться каждые 30 минут

- ✅ Отвечать на новые отзывы

- ✅ Отправлять уведомления в Telegram

### AI провайдеры

#### Бесплатные шаблоны (`AI_PROVIDER=free`)
- Использует предустановленные шаблоны ответов

- Не требует API ключей

- Подходит для тестирования

#### Российские AI (`AI_PROVIDER=russian`)
##### Yandex GPT:

- Требует `YANDEX_API_KEY` и `YANDEX_FOLDER_ID`

- Более качественные и уникальные ответы

##### GigaChat:

- Требует `GIGACHAT_API_KEY`

- Альтернатива Yandex GPT

#### Локальные шаблоны (`AI_PROVIDER=fallback`)
- Резервный вариант при недоступности API

- Простые шаблонные ответы

### Telegram уведомления
#### Создание Telegram бота
1. Найдите ***@BotFather*** в Telegram

2. Отправьте: `/newbot`

3. Введите имя: `Your Name Bot`

4. Скопируйте токен

#### Получение Chat ID
1. Найдите вашего бота в Telegram
2. Отправьте любое сообщение
3.Выполните команду:

```bash
curl https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates
```
Скопируйте `chat.id` из ответа

### Типы уведомлений
#### Мгновенные уведомления
Приходят сразу после ответа на отзыв:
```bash
text
📝 Новый ответ на отзыв

👤 Покупатель: Анна
⭐ Оценка: ⭐⭐⭐⭐⭐ (5/5)
🏷️ Товар: Футболка хлопковая

💬 Отзыв: Отличный товар! Качество...
🤖 Ответ бота: Большое спасибо за ваш отзыв...
```
#### Ежедневная статистика
Приходит каждый день в 9:00 UTC:
```bash
text
📊 Ежедневная статистика Wildberries Bot

📅 Дата: 21.10.2024
🔄 Проверок за день: 48
📝 Обработано отзывов: 5
📤 Отправлено ответов: 3
⭐ Текущая средняя оценка: 4.7

📈 Неотвеченных отзывов: 2
📋 Новых за сегодня: 1
```
#### Уведомления об ошибках
При проблемах с API или настройками

### Локальная разработка
#### Структура проекта
```bash
text
wb_parser/
├── .github/workflows/          # GitHub Actions
│   └── bot.yml                 # Основной workflow и ежедневные отчеты
├── src/
│   ├── api/                    # API клиенты
│   │   ├── wb_client.py        # Wildberries API
│   │   └── rate_limiter.py     # Ограничитель запросов
│   ├── ai/                     # AI генераторы
│   │   ├── generator.py        # Основной генератор
│   │   ├── free_generator.py   # Бесплатные шаблоны
│   │   ├── russian_generator.py # Российские AI
│   │   └── fallback_generator.py # Локальные шаблоны
│   ├── core/                   # Основная логика
│   │   ├── manager.py          # Менеджер ответов
│   │   └── processor.py        # Обработчик отзывов
│   ├── config/                 # Конфигурация
│   │   ├── settings.py         # Настройки приложения
│   │   └── constants.py        # Константы
│   └── utils/                  # Утилиты
│       ├── telegram_notifier.py # Telegram уведомления
│       └── logger.py           # Логирование
├── scripts/                    # Вспомогательные скрипты
│   ├── status.py              # Диагностика системы
│   ├── test_bot.py            # Тестирование функционала
│   └── daily_report.py        # Ежедневные отчеты
├── main.py                    # Точка входа
├── check.py                   # Быстрая проверка
└── requirements.txt           # Зависимости
```
### Полезные команды

Быстрая проверка
```bash
python check.py
```

Полная диагностика
```bash
python scripts/status.py
```
Тестирование всех функций
```bash
python scripts/test_bot.py
```
Проверка Yandex GPT
```bash
python scripts/check_yandexgpt.py
```
### Мониторинг
#### GitHub Actions
- Actions → Wildberries Feedback Bot - логи выполнения
- Actions → Daily Wildberries Report - ежедневные отчеты

#### Локальное логирование
Логи сохраняются в папку `logs/` при локальном запуске.

Статус бота
- ✅ Зеленый - бот работает нормально

- ⚠️ Желтый - есть предупреждения

- ❌ Красный - критические ошибки

### FAQ
**Бот не отвечает на отзывы**
- Проверьте `WB_API_KEY` в Secrets
- Убедитесь что `SUPPLIER_ID` правильный
- Проверьте логи в GitHub Actions

**Не приходят Telegram уведомления**
- Проверьте `TELEGRAM_BOT_TOKEN` и `TELEGRAM_CHAT_ID`
- Убедитесь что бот активирован (отправьте `/start`)
- Проверьте что Chat ID правильный

**Ошибка 401 Unauthorized**
- Обновите `WB_API_KEY` в ЛК Wildberries
- Проверьте права доступа токена

**AI не генерирует ответы**
- При `AI_PROVIDER=free` используются простые шаблоны
- Для Yandex GPT проверьте API ключ и Folder ID
- При ошибках автоматически переключается на fallback

**Как изменить частоту проверок**
В `.github/workflows/bot.yml` измените cron:

```bash
yaml
schedule:
  - cron: '*/15 * * * *'  # Каждые 15 минут
```
### Техническая поддержка
Если возникли проблемы:
- Проверьте логи в GitHub Actions
- Запустите диагностику:
```bash
python scripts/status.py
```
- Проверьте настройки в Secrets
- Создайте issue в репозитории

ons с уведомлениями в Telegram.
