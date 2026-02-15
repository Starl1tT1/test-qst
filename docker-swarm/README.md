# Docker Swarm Stack — Ubuntu Service

## Назначение

Развертывание сервиса в Docker Swarm со следующими параметрами:

- Образ: `ubuntu:22.04`
- Не менее 2 реплик
- Постоянная работа контейнера (`sleep infinity`)
- Подключение к сетям:
  - `db-postgres-net`
  - `ds-redis-net`
- Ограничение ресурсов: 1 CPU, 500MB RAM
- Rolling update без даунтайма
- Ограничение логов: 1 файл, 5MB
- Запуск только на нодах с label `SERVERTYPE=worker`
- Переменная окружения `HOSTNAME` содержит имя ноды
- Volume для логов

---

## Требования

- Docker Engine >= 20.x
- Инициализированный Docker Swarm
- Созданные overlay-сети:
  - `db-postgres-net`
  - `ds-redis-net`
- Ноды с label:

```bash
docker node update --label-add SERVERTYPE=worker <node_name>
```
## Инициализация
Создание сетей
```
docker network create --driver overlay db-postgres-net
docker network create --driver overlay ds-redis-net
```

Деплой docker-compose в swarm
```
docker stack deploy -c docker-stack.yml app-stack
```

Подключение к контейнеру и установка нужного ПО
```
docker exec -it <container_id> bash
apt update
apt install -y postgresql-client redis-tools netcat iputils-ping
```
Проверка postgresql 
```
nc -zv <postgres_host> 5432
psql -h <postgres_host> -U <user> -d <database>
```
Проверка redis
```
redis-cli -h <redis_host> ping
```