# Ollama

Ollama - это инструмент для управления и взаимодействия с языковыми моделями (LLM). По факту является оберткой над `llama.cpp`.

## Установка

Для установки Ollama, следуйте инструкциям на официальном сайте: [Ollama Installation Guide](https://ollama.com/docs/installation).

Установка под Linux:

```bash
curl -sSL https://ollama.com/install.sh | sh
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