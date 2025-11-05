1. Использование

```dockerfile
FROM python:latest
```

нестабильно, поскольку версия может меняться.

---

2. Нет очистки кеша после установки системных пакетов .
   отсутсвует 
```
rm -rf /var/lib/apt/lists/*
```

---

3. Копирование всех файлов

```dockerfile
COPY . .
```

копирует мусор и секреты. нет  `.dockerignore` 

---

4. Установка зависимостей без `--no-cache-dir`

---

5. Shell-форма команды

```dockerfile
CMD python app.py
```

надо: `CMD ["python", "app.py"]`
