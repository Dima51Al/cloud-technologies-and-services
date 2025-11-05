
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
