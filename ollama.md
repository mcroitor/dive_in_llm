# Ollama

Ollama - это инструмент для управления и взаимодействия с языковыми моделями (LLM). По факту является оберткой над `llama.cpp`.

## Установка

Для установки Ollama, следуйте инструкциям на официальном сайте: [Ollama Installation Guide](https://ollama.com/docs/installation).

Установка под Linux:

```bash
curl -sSL https://ollama.com/install.sh | sh
```

## Ollama Docker

Ollama также поддерживает использование через Docker. Вы можете запустить Ollama в контейнере, используя следующий команду:

```bash
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

В этом случае Ollama использует CPU для работы, скачиваемые модели будут сохраняться в томе `ollama`, который монтируется в папку `/root/.ollama` внутри контейнера.

Естественным образом хочется использовать Ollama с GPU, что возможно при запуске контейнера с флагом `--gpus all`:

```bash
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

Поэтому, удобнее пользоваться файлом `docker-compose.yml`, который уже содержит все необходимые настройки для запуска Ollama с поддержкой Nvidia GPU:

```yaml
services:
  ollama:
    image: ollama/ollama
    container_name: ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - count: all
            - capabilities: [gpu]
```

Можно указать конкретную GPU по ее ID:

```yaml
services:
  ollama:
    image: ollama/ollama
    container_name: ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]
              options:
                device: "0" # Укажите ID вашей GPU
```

При запуске контейнера через WSL2 на Windows можно воспользоваться подключением устройства GPU через `/dev/dri`:

```yaml
services:
  ollama:
    image: ollama/ollama
    container_name: ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama:/root/.ollama
    devices:
      - /dev/dri:/dev/dri # Проборос всех Intel GPU устройств в контейнер
```

## Использование

После установки, вы можете начать использовать Ollama для управления своими языковыми моделями. Вот несколько основных команд:

- `ollama pull <model>` - загрузить модель.
- `ollama run <model>` - запустить модель.
- `ollama list` - показать список доступных моделей.
- `ollama stop <model>` - остановить модель.
- `ollama logs <model>` - просмотреть логи модели.
- `ollama remove <model>` - удалить модель.

Модели можно скачать с официального репозитория Ollama - [Ollama Model Repository](https://ollama.com/search).

## Интересные модели

- `qwen3.5` - модель от Alibaba общего назначения.
- `deepseek-ocr` - модель для распознавания текста на изображениях.
