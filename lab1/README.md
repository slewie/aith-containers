# Лабораторная работа №1

## Плохие практики в Dockerfile.bad

### 1. Использование тега `latest`

**Плохо:**
```dockerfile
FROM jupyterhub/jupyterhub:latest
```

**Почему это плохо:**
- Сборки становятся невоспроизводимыми, сегодня соберётся одна версия, завтра другая
- Невозможно отследить, какая именно версия используется в проде

**Исправление в Dockerfile.good:**
```dockerfile
FROM jupyterhub/jupyterhub:3.1.1
```

### 2. Несколько команд RUN

**Плохо:**
```dockerfile
RUN apt update 
RUN apt -y install python3-dev
RUN apt -y install git
RUN apt -y install vim
RUN apt -y install nano
RUN apt -y install build-essential
RUN apt -y install curl
RUN apt -y install wget
```

**Почему это плохо:**
- Каждая команда `RUN` создаёт новый слой в образе, что увеличивает общий размер образа, замедляет процесс сборки и кэш слоёв используется неэффективно
- В данном случае создаётся 8 дополнительных слоёв вместо одного

**Исправление в Dockerfile.good:**
```dockerfile
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3-dev \
    git \
    vim \
    nano \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
```

### 3. Запуск от имени root

**Плохо:**
В `Dockerfile.bad` не указан пользователь, поэтому контейнер запускается от root.

**Почему это плохо:**
- Серьёзная уязвимость безопасности
- Процессы внутри контейнера могут случайно повредить систему

**Исправление в Dockerfile.good:**
```dockerfile
RUN ... \
    && addgroup --gid 1001 appgroup \
    && useradd --uid 1001 --gid 1001 --create-home --shell /bin/bash appuser \
    && chown -R appuser:appgroup /app

USER appuser
```
Создаётся непривилегированный пользователь, от имени которого запускается приложение.


## Плохие практики по использованию контейнеризации

1. Хранить данные внутри контейнера. Когда удаляется контейнер, данные удаляются.
2. Запускать контейнеры в привилегированном режиме. Это позволяет контейнеру иметь доступ к всем ресурсам хоста.
