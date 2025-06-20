# Отчет по лабораторной работе №1

**Задание на 15 баллов:** 

Необходимо реализовать функционал пользователя в разрабатываемом приложении. Функционал включает в себя:
- Авторизацию и регистрацию
- Генерацию JWT-токенов
- Аутентификацию по JWT-токену
- Хэширование паролей
- Дополнительные АПИ-методы для получения информации о пользователе, списка пользователей и смене пароля

## Описание реализованного функционала

### Функционал пользователя:
1. **Авторизация и регистрация.**  
    Реализованы эндпоинты:
      - `POST /auth/register` – регистрация нового пользователя.
      - `POST /auth/login` – авторизация пользователя с передачей email и пароля через форму.
  
2. **Генерация и проверка JWT-токенов.**  
  Функция `create_access_token` (в `security.py`) генерирует JWT-токен на основе данных пользователя. Функция `decode_token` проверяет и расшифровывает токен. Эти функции используются для аутентификации по JWT.

3. **Аутентификация по JWT-токену.**  
  Функция `get_current_user` (в `security.py`) извлекает текущего пользователя из токена, который передаётся через заголовок `Authorization` в формате `Bearer <токен>`.

4. **Хэширование паролей.**  
  Реализованы функции `hash_password` и `verify_password` для безопасного хранения и проверки паролей.

5. **Дополнительные API-методы.**  
  Реализован метод `GET /users/me` (в `main.py`), который возвращает информацию о текущем пользователе, основываясь на JWT-токене. В файле `app/routers/user.py` также реализованы эндпоинты для смены пароля и получения списка пользователей.

## Что было сделано

1. **Настройка проекта и установка зависимостей.**  
      - Создан виртуальный окружение.
      - Установлены пакеты: FastAPI, uvicorn, SQLModel, Alembic, python-dotenv, python-multipart, jose (для JWT), mkdocs (для документации) и mkdocs-material.

2. **Конфигурация подключения к базе данных.**  
      - Файл `.env` содержит параметры подключения к PostgreSQL:
     ```
     DB_URL=postgresql://postgres:123@localhost:5433/timemanager_db
     SECRET_KEY=...
     ALGORITHM=HS256
     ACCESS_TOKEN_EXPIRE_MINUTES=30
     ```
      - Файл `db.py` загружает переменные окружения и создаёт подключение к базе через SQLModel.

3. **Реализация моделей данных.**  
      - В файле `models.py` определены модели:
          - **User** – содержит поля: id, username, email, hashed_password, role.
          - **Task** – для задач с полями: title, description, deadline, priority, is_completed, user_id.
          - Дополнительные модели: TimeLog, UserTaskLink, DailySchedule.

4. **Реализация функционала аутентификации и регистрации.**  
      - В файле `app/routers/auth.py` реализованы эндпоинты:
          - `POST /auth/register` – регистрирует пользователя, хэшируя пароль с помощью `hash_password`.
          - `POST /auth/login` – принимает данные из формы (email и password) и возвращает JWT-токен, если данные верны.
          - Функции для создания, декодирования JWT и проверки пароля реализованы в `app/utils/security.py`.

5. **Реализация API-метода для получения информации о текущем пользователе.**  
      - В файле `main.py` реализован эндпоинт `GET /users/me`, который возвращает username пользователя, извлечённого из JWT-токена через функцию `get_current_user`.
      - В файле `user.py` реализованы эндпоинты для смены пароля `PUT /users/change-password` и получения списка пользователей `GET /users`.

6. **Настройка OpenAPI и Swagger UI.**  
      - В файле `main.py` добавлена функция `custom_openapi` для переопределения OpenAPI-схемы, чтобы в Swagger UI отображалась авторизация через Bearer-токен, а не через логин и пароль.

7. **Применение миграций с помощью Alembic.**  
      - Создан файл миграции (`d7bb2f7f0785_add_user_model.py`), который добавляет поле `role` в таблицу `user`.
      - Выполнена команда `alembic upgrade head` для применения миграций.



