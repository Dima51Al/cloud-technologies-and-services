FROM python:3.12-slim

# системные deps для аудио/ffmpeg
RUN apt-get update && apt-get install -y \
    ffmpeg \
    libsndfile1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Устанавливаем совместимые CPU-колёса PyTorch + torchvision + torchaudio (и torchmetrics)
# Важно: делаем это ДО pip install -r requirements.txt, чтобы pip не перезаписал/не подтянул несовместимые бинарные пакеты.
RUN pip install --no-cache-dir --index-url https://download.pytorch.org/whl/cpu \
    "torch==2.2.2+cpu" "torchvision==0.17.2+cpu" "torchaudio==2.2.2+cpu" \
 && pip install --no-cache-dir torchmetrics==1.3.1

# Копируем requirements (убедись, что в requirements.txt нет строк с torch/torchvision/torchaudio)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Копирование моделей
COPY models/ ./models/

# Небольшая проверка (не фатальная — если пути другие, убери)
RUN ls -la /app/models/reverb-diarization-v2 || true

# Копирование остального проекта
COPY . .

RUN mkdir -p uploads output && chown -R 1000:1000 /app/uploads /app/output

EXPOSE 5000
CMD ["python", "app.py"]
