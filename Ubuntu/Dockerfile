FROM ubuntu:24.04

# Установка базовых зависимостей
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-pip \
    python3-venv \
    curl \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Настройка рабочей директории
WORKDIR /app

# Аргумент для версии (можно переопределить при сборке)
ARG VERSION=0.1.1

# Загрузка и распаковка нужной версии проекта
RUN if [ "$VERSION" = "0.1.0" ]; then \
        URL="https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.0.tar.gz" && \
        HASH="3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444"; \
    elif [ "$VERSION" = "0.1.1" ]; then \
        URL="https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.1.tar.gz" && \
        HASH="30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b"; \
    elif [ "$VERSION" = "main" ]; then \
        URL="https://github.com/MrDave/StaticJinjaPlus/archive/refs/heads/main.tar.gz" && \
        HASH="9adccb8fe17a40252df1a3acdea7edef4633b4ecaa8ba2dd5e0270f87ae43eab"; \
    else \
        echo "Unsupported version: $VERSION" && exit 1; \
    fi && \
    curl -L "$URL" -o SJP.tar.gz && \
    echo "$HASH  SJP.tar.gz" | sha256sum -c - && \
    tar xzf SJP.tar.gz --strip-components=1 && \
    rm SJP.tar.gz

# Создание и активация виртуального окружения
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Установка зависимостей в виртуальном окружении
RUN pip3 install --no-cache-dir --upgrade pip && \
    pip3 install --no-cache-dir -r requirements.txt

# Очистка ненужных пакетов
RUN apt-get purge -y --auto-remove build-essential curl && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Команда запуска
CMD ["python", "main.py"]