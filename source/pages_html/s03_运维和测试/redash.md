docker pull redash/redash

https://redash.io/help/open-source/dev-guide/docker



https://hub.docker.com/r/redash/redash

https://www.cnblogs.com/rongfengliang/p/9901464.html

```yaml
version: '2'
x-redash-service: &redash-service
  image: redash/redash:7.0.0.b18042
  depends_on:
    - postgres
    - redis
  env_file: /opt/redash/env
  restart: always
services:
  server:
    <<: *redash-service
    command: server
    ports:
      - "5000:5000"
    environment:
      REDASH_WEB_WORKERS: 4
  scheduler:
    <<: *redash-service
    command: scheduler
    environment:
      QUEUES: "celery"
      WORKERS_COUNT: 1
  scheduled_worker:
    <<: *redash-service
    command: worker
    environment:
      QUEUES: "scheduled_queries,schemas"
      WORKERS_COUNT: 1
  adhoc_worker:
    <<: *redash-service
    command: worker
    environment:
      QUEUES: "queries"
      WORKERS_COUNT: 2
  redis:
    image: redis:5.0-alpine
    restart: always
  postgres:
    image: postgres:9.5-alpine
    env_file: /opt/redash/env
    volumes:
      - /opt/redash/postgres-data:/var/lib/postgresql/data
    restart: always
  nginx:
    image: redash/nginx:latest
    ports:
      - "80:80"
    depends_on:
      - server
    links:
      - server:redash
restart: always
```



## 二、docker-compose安装

#### 1，下载docker-compose

```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

#### 2，授权

```
$ sudo chmod +x /usr/local/bin/docker-compose
```

#### 3，查看版本信息

```
$ docker-compose --version
显示出版本信息，即安装成功。
```

https://ywnz.com/linuxysjk/4254.html

```yaml
# This is an example configuration for Docker Compose. Make sure to atleast update
# the cookie secret & postgres database password.
#
# Some other recommendations:
# 1. To persist Postgres data, assign it a volume host location.
# 2. Split the worker service to adhoc workers and scheduled queries workers.
version: '2'
services:
 server:
   image: redash/redash:latest
   command: server
   depends_on:
     - postgres
     - redis
   ports:
     - "5000:5000"
   environment:
     PYTHONUNBUFFERED: 0
     REDASH_LOG_LEVEL: "INFO"
     REDASH_REDIS_URL: "redis://redis:6379/0"
     REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
     REDASH_COOKIE_SECRET: veryverysecret
     REDASH_WEB_WORKERS: 4
   restart: always
 worker:
   image: redash/redash:latest
   command: scheduler
   environment:
     PYTHONUNBUFFERED: 0
     REDASH_LOG_LEVEL: "INFO"
     REDASH_REDIS_URL: "redis://redis:6379/0"
     REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
     QUEUES: "queries,scheduled_queries,celery"
     WORKERS_COUNT: 2
   restart: always
 redis:
   image: redis:3.0-alpine
   restart: always
 postgres:
   image: postgres:9.5.6-alpine
   # volumes:
   #   - /opt/postgres-data:/var/lib/postgresql/data
   restart: always
 nginx:
   image: redash/nginx:latest
   ports:
     - "80:80"
   depends_on:
     - server
   links:
     - server:redash
   restart: always
```

