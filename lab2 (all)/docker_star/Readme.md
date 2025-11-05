# lab-docker-star
`Афанасьев Дмитрий Михайлович`
`Сигаева Ксения Леонидовна`

Со звездочкой:
1) Написать “плохой” Docker compose файл, в котором есть не менее трех “bad practices” по их написанию
2) Написать “хороший” Docker compose файл, в котором эти плохие практики исправлены
3) В Readme описать каждую из плохих практик в плохом файле, почему она плохая и как в хорошем она была исправлена, как исправление повлияло на результат
4) После предыдущих пунктов в хорошем файле настроить сервисы так, чтобы контейнеры в рамках этого compose-проекта так же поднимались вместе, но не "видели" друг друга по сети. В отчете описать, как этого добились и кратко объяснить принцип такой изоляции

# bad_practices

---

```yaml

version: "3"

services:
  diarization-app:
    image: diarization-app:latest
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    environment:
      - FLASK_ENV=development
      - SECRET_KEY=mysecret123
    restart: unless-stopped


```

---

1. нестабильная версия
```yaml
image: diarization-app:latest
```

нужно: `diarization-app:1.0.0`

2. Проброс портов без ограничения

```yaml
ports:
  - "5000:5000"
```

нужно: `127.0.0.1:5000:5000` - только локально.


3. Переменные окружения в файле

```yaml
environment:
  - SECRET_KEY=mysecret123
```
нужно:  `.env`.

4. Версия docker-compose
```yml
version: "3"
```

```не рекомендуется использовать```

---


### Исправленный вариант

```yaml

services:
  diarization-app:
    image: diarization-app:1.0.0
    ports:
      - "127.0.0.1:5000:5000"
    volumes:
      - ./uploads:/app/uploads
      - ./output:/app/output
    env_file:
      - .env
    restart: unless-stopped
```


# изоляция

я взял свой небольшой проект для приложения для репетитора
в нем есть БД, бэк, фронт, nginx для веба

---

```yaml

services:
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: monitor
      POSTGRES_USER: monitor
      POSTGRES_PASSWORD: changeme
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - backend_net

  backend:
    build: ./backend
    depends_on:
      - db
    environment:
      DATABASE_URL: postgresql://monitor:changeme@db:5432/monitor
    volumes:
      - ./backend:/app
    expose:
      - "8000"
    networks:
      - backend_net

  frontend:
    build: ./frontend
    volumes:
      - ./frontend:/usr/share/nginx/html:ro
    expose:
      - "80"
    networks:
      - frontend_net

  nginx:
    image: nginx:stable
    ports:
      - "8081:80"
    volumes:
      - ./deploy/nginx.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - frontend
      - backend
    networks:
      - backend_net
      - frontend_net

volumes:
  db_data:

networks:
  backend_net:
  frontend_net:


```

---

контейнеры поднимаются вместе, но не имеют прямого доступа друг к другу по сети

созданы 2 сети

`backend_net` 

и 

`frontend_net`

дальше сервисы подключаются каждый к своей сети

то есть 

`БД` + `бэк` к `backend_net`

`фронт` к `frontend_net`

`nginx` к обоим (необходимо)


