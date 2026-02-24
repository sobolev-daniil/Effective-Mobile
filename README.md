# Тестовое задание Effective Mobile

## Описание проекта
Простое веб-приложение, развернутое в Docker-контейнерах:
- **Backend**: Python HTTP-сервер (http.server) отвечает текстом "Hello from Effective Mobile!"
- **Nginx**: Reverse proxy, принимает запросы на порт 80 и перенаправляет их на backend

## Запуск проекта

### Предварительные требования
- Установленные Docker и Docker Compose
- Git (для клонирования репозитория)

### Шаги для запуска

1. **Клонируйте репозиторий**
   ```bash
   git clone https://github.com/sobolev-daniil/Effective-Mobile.git
   cd Effective-Mobile
2. **Запустите проект**
   ```bash
   docker-compose up -d
3. **Проверьте, что все контейнеры запустились**
   ```bash
   docker-compose ps

### Проверка работоспособности

1. **Основная проверка**
   ```bash
   curl http://localhost
2. **Проверка изоляции backend**
   ```bash
   curl http://localhost:8080
3. **Просмотр логов**
   ```bash
   docker-compose logs -f
   
## Архитектура проекта (кратко)

Сервисы запущены в отдельных Docker-контейнерах и взаимодействуют через внутреннюю сеть. Nginx принимает запросы с хоста и проксирует их на backend, который недоступен напрямую извне.

### Схема взаимодействия

Хост (порт 80) → Nginx (контейнер) → Backend (контейнер, порт 8080)

## Использованные технологии

- **Docker** и **Docker Compose** — контейнеризация и оркестрация
- **Python 3.11** (alpine) — backend на встроенном HTTP-сервере
- **Nginx** (alpine) — reverse proxy

## Безопасность

- **Non-root пользователь** — backend запускается от `appuser`, не от root
- **Read-only** — корневая файловая система backend в режиме только для чтения
- **No-new-privileges** — запрещено повышение привилегий в контейнере
- **Read-only конфиги** — nginx.conf монтируется в режиме `:ro` (только чтение)
- **Минимальные образы** — alpine-версии Python и nginx
- **Скрытие версий** — `server_tokens off` в nginx
- **Сетевая изоляция** — отдельная bridge-сеть `effective-network`
- **Минимум портов** — backend доступен только внутри Docker-сети