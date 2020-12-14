# Main proxy

Главный docker образ на сервере для запуска нескольких контейнеров/приложений. 

## Структура

- `main_proxy`
  - `docker-compose.base.yml`
  - `docker-compose.staging.yml`
  - `docker-compose.production.yml`
  - `docker-compose.portainer.yml`
  - `env`
    - `staging`
      - `nginx-proxy-letsencrypt`
    - `production`
      - `nginx-proxy-letsencrypt`
  - `app1`
  - `app2`

## Как развернуть

1. Склонировать репозиторий
2. Изменить следующие значения
    - если нужен portainer, то в файле `docker-compose.portainer.yml`
      - `{{portainer_domain}}` - домен для входа в админ панель portainer
    - в файлах переменных окружений
      - `{{email}}` - email для регистрации сертификата letsencrypt

## Команды

### Префикс команды {{prefix}}

- `docker-compose -f docker-compose.base.yml -f docker-compose.staging.yml -f docker-compose.portainer.yml` - Запуск в staging окружении с portainer
- `docker-compose -f docker-compose.base.yml -f docker-compose.production.yml -f docker-compose.portainer.yml` - Запуск в production окружении с portainer
- `docker-compose -f docker-compose.base.yml -f docker-compose.staging.yml -f` - Запуск в staging окружении без portainer
- `docker-compose -f docker-compose.base.yml -f docker-compose.production.yml` - Запуск в production окружении без portainer

### Запуск

- `{{prefix}} up -d --build` - запуск контейнеров
- `{{prefix}} down` - остановка контейнеров

### Другие команды

- `{{prefix}} logs -f -t {{container_name}}` - просмотр логов
- `{{prefix}} exec {{container_name}} {{command}}` - выполнение команды
- `{{prefix}} build --no-cache {{container_name}}` - билд контейнера без кеша

## Известные кейсы

### Перегенерация сертификатов

1. `{{prefix}} down`
2. `docker volume rm main_proxy_vhost`
3. `docker volume rm main_proxy_certs`
4. `{{prefix}} up -d --build`
5. `{{prefix}} logs -f -t nginx-proxy-letsencrypt`
