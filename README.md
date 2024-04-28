# taski-docker

## Краткое описание проекта:
Приложение для планирования своих задач.

Проект был размещен на сервере Ubuntu в трёх контейнерах при помощи Docker:
- nginx
- backend
- frontend
  
В файле docker-compose описывается система из контейнеров. Docker-compose-production описывает уже непосредственно вариант с работой через GitHub Actions и деплоем на сервере.

Реализован деплой на сервере после загрузки на GitHub при помощи GitHub Actions. Перед деплоем GitHub Actions делает следующее:
- тестирует backend код на соответствие PEP8
- тестирует frontend код
- загружает на DockerHub все контейнеры (c nginx, backend и frontend)
- с DockerHub осуществляет деплой на сервер
- направляет в Телеграм сообщение о совершенном деплое, уточняя:
  - кто сделал коммит
  - какое сообщение было у коммита
  - ссылку на коммит

## Как запустить проект:
1. Установить Docker, Docker Compose (для Windows - актуальный Docker Desktop).

2. Клонировать репозиторий с проектом на свой компьютер:
   ```git clone git@github.com:Nina2301/taski-docker.git```

3. Установить и активировать виртуальное окружение: 
```
python3.9 -m venv venv
. venv/bin/activate
```
В виртуальном окружении установить зависимости:
```
pip install -r requirements.txt
```

4. Создать .env файл с информацией:                                                       
- POSTGRES_USER= логин
- POSTGRES_PASSWORD= пароль
- POSTGRES_DB= имя БД
- DB_HOST= название хоста
- DB_PORT=5432

5. Выполнить сборку контейнеров: ```docker-compose up -d --build```
6. Выполнить миграции: ```docker-compose exec backend python manage.py migrate```
7. Создать суперпользователя: ```docker-compose exec backend python manage.py createsuperuser```
8. Собрать файлы статики: ```docker-compose exec backend python manage.py collectstatic```
9. Скопировать файлы статики в /backend_static/static/ backend-контейнера: ```docker compose exec backend cp -r /app/collected_static/. /backend_static/static/```

- 127.0.0.1:8000 Главная страница

