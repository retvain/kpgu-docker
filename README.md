## Для начала работы:
- клонировать проект в директорию projects
- настроить .env файл:
`cp .env-docker .env`
- запустить контейнеры
`docker-compose up -d`
- начало работы с проектом ведется из контейнера с kpgu-app
`docker-compose exec kpgu-app bash`
- настройка composer авторизации для работы с репозиториями Восхода
`composer config --global gitlab-oauth.git.voskhod-11.ru token` вместо `token` указать свой токен GitLab
- установить зависимости
`composer install`
