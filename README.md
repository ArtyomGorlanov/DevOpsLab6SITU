# DevOps СИТУ Лабораторная работа №6: Развертывание многоконтейнерных приложений и системы мониторинга

## Описание

Цель лабораторной работы - развернуть два многоконтейнерных приложения с использованием **Docker Compose**:

- стек **Flask + Redis** - веб-приложение со счетчиком посещений;
- стек **Prometheus + Grafana** - система мониторинга.

Дополнительно настроен сбор метрик и контроль доступности веб-приложения с помощью **Prometheus** и **Blackbox Exporter**.

> Работа выполнялась на двух серверах:
>
> - **Linux Debian** - стек Flask + Redis;
> - **Ubuntu Server 24.04** - стек Prometheus + Grafana.

## Структура проекта

### Сервер Debian (Flask + Redis)

```text
.
├── app.py
├── compose.yml
├── Dockerfile
└── requirements.txt
```

### Сервер Ubuntu 24.04 (Prometheus + Grafana)

```text
.
├── compose.yml
├── grafana
│   └── datasource.yml
└── prometheus
    └── prometheus.yml
```

## Flask + Redis

### Состав стека

- Flask
- Redis

### Функциональность

Веб-приложение:

- отвечает по HTTP;
- хранит количество посещений в Redis;
- увеличивает счетчик при каждом обращении.

Пример ответа:

```text
Hello World! I have been seen 638 times.
```

### `compose.yml`

Docker Compose запускает два контейнера:

- `web` - Flask-приложение;
- `redis` - база данных Redis.

Порт приложения:

```text
8000 → 5000
```

### Dockerfile

Dockerfile выполняет следующие действия:

1. Использует образ Python 3.10 Alpine.
2. Устанавливает необходимые зависимости.
3. Копирует исходный код приложения.
4. Запускает Flask-сервер.

## Prometheus + Grafana

### Состав стека

- Prometheus
- Grafana
- Blackbox Exporter

### `compose.yml`

Docker Compose запускает три контейнера:

- Prometheus;
- Grafana;
- Blackbox Exporter.

Доступные сервисы:

| Сервис | Порт |
|--------|-----:|
| Grafana | 3000 |
| Prometheus | 9090 |
| Blackbox Exporter | 9115 |

### Конфигурация Prometheus

Настроен сбор следующих метрик:

- собственные метрики Prometheus;
- проверка доступности Flask-приложения через Blackbox Exporter;
- мониторинг внешнего HTTP-ресурса;
- сбор метрик Flask-приложения по адресу `/metrics`.

### Конфигурация Grafana

Grafana автоматически подключает Prometheus как источник данных через механизм Provisioning.

## Запуск

### Flask + Redis

Перейти в каталог проекта и выполнить:

```bash
docker compose up -d
```

### Prometheus + Grafana

Перейти в каталог проекта и выполнить:

```bash
docker compose up -d
```

## Результат

После успешного развертывания:

- запущено веб-приложение **Flask + Redis**;
- приложение доступно по порту **8000**;
- счетчик посещений хранится в Redis и увеличивается при каждом обращении;
- развернут сервер мониторинга **Prometheus**;
- настроен **Grafana** с автоматически подключенным источником данных Prometheus;
- через **Blackbox Exporter** выполняется проверка доступности HTTP-сервисов;
- Prometheus регулярно собирает метрики приложения и системы мониторинга.

## Используемые технологии

- Docker
- Docker Compose
- Python
- Flask
- Redis
- Prometheus
- Grafana
- Blackbox Exporter
