### README.md для проекта **Kittygram**

---

## Описание проекта

**Kittygram** — проект, где пользователи могут зарегистрироваться, делиться фотографиями своих любимых кошек вместе с описаниями их забавных историй и успехов, а также наслаждаться просмотром фотографий других пушистых друзей.

---

## Стек технологий

Для наглядности статуса сборки и тестирования используем GitHub Actions:

[![Main Kittygram workflow](https://github.com/alexproevolution/kittygram_final/actions/workflows/main.yml/badge.svg)](https://github.com/alexproevolution/kittygram_final/actions/workflows/main.yml)

---

## Автор

**Александр Садуха**

---

## Инструкция по запуску проекта

Чтобы начать работу с проектом Kittygram, выполните следующие шаги:

### Шаг 1. Клонируйте репозиторий и перейдите в папку проекта:

```bash
git clone git@github.com:alexproevolution/kittygram_final.git
```

Затем перейдите в корневую директорию проекта:

```bash
cd kittygram_final
```

### Шаг 2. Создайте файл `.env` для настройки окружения

Файл `.env` необходим для хранения важных конфигурационных переменных. Создайте этот файл вручную и заполните необходимыми значениями:

```bash
SECRET_KEY='Ваш секретный ключ Django'
ALLOWED_HOSTS='Имя вашего хостинга или IP адрес'
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_NAME=kittygram
DB_HOST=db_1
DB_PORT=5432
```

### Шаг 3. Запустите контейнеры Docker

Используйте команду ниже для старта приложения в режиме продакшена:

```bash
docker compose -f docker-compose.production.yml up
```

### Шаг 4. Выполните миграцию базы данных и соберите статические файлы

Эти команды применяются внутри контейнера Docker:

```bash
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /static/static/
```

### Шаг 5. Создание суперпользователя

Это позволит управлять контентом через админ-панель сайта:

```bash
docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```

Следуя подсказкам, введите электронную почту, логин и пароль администратора.

---

## Настройка автоматического тестирования и развертывания (CI/CD)

### Цель задания

Цель состоит в настройке процесса непрерывной интеграции и доставки (**CI/CD**) с использованием GitHub Actions и Docker.

### Тестирование проекта

#### Локальные тесты

Для локальных проверок создаётся специальный файл `tests.yml`, содержащий необходимую конфигурацию:

```yaml
repo_owner: ваш_логин_на_GitHub
kittygram_domain: https://полная_ссылка_на_проект_Kittygram
taski_domain: https://полная_ссылка_на_проект_Taski
dockerhub_username: ваш_логин_на_DockerHub
```

Для запуска локальных тестов создайте виртуальное окружение Python, установите необходимые пакеты из файла `backend/requirements.txt` и выполните команду:

```bash
pytest
```

---

## Чек-лист перед сдачей задания

Перед отправкой убедитесь, что выполнены следующие условия:

- Ваш проект **Taski** доступен по домену, указанному в файле `tests.yml`.
- Проект **Kittygram** доступен по адресу, указанному в `tests.yml`.
- После каждого пуша в ветку `main` автоматически запускаются тесты и выполняется деплой проекта **Kittygram**, после чего уведомление отправляется в Telegram.
- В корневом каталоге проекта присутствует файл `kittygram_workflow.yml`.

---
