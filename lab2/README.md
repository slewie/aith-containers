# Лабораторная работа №2

## Описание docker-compose
- init (alpine) однократно создаёт маркер `/data/READY` в общем volume `app-data`.
- app собирается из `Dockerfile` в `${APP_IMAGE}`, монтирует `app-data`, пробрасывает `${APP_PORT}:8000`, зависит от init и cache, healthcheck на `/hub/health`.
- cache - `redis:7.2-alpine`, пробрасывает `${REDIS_PORT}:6379`, healthcheck `redis-cli ping`.
- Все сервисы в сети `appnet` (имя `${NETWORK_NAME}`), переменные берутся из `.env`.

## Ответы на вопросы

1. Да, можно. Надо указать для сервиса через deploy:resources, но работает это только через swarm, а в дефолтном compose игнорируется:
```yaml
deploy:
    resources:
        limits:
          cpus: '0.50'
          memory: 50M
        reservations:
          cpus: '0.25'
          memory: 20M
```

2. Надо просто передать имя сервиса в консоли:
`docker compose up -d [SERVICE...]`