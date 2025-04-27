FROM python:3.11-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

RUN apk add --no-cache \
    gcc \
    musl-dev \
    libffi-dev \
    postgresql-dev \
    && rm -rf /var/cache/apk/*

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 5000

# Добавляем переменные окружения для Flask
ENV FLASK_APP=main.py
ENV FLASK_ENV=production

# Запускаем миграцию и потом приложение
CMD ["sh", "-c", "flask db upgrade && exec python main.py"]
