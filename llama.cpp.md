# llama.cpp

## Содержание

- [llama.cpp](#llamacpp)
  - [Содержание](#содержание)
  - [Установка](#установка)
    - [Windows](#windows)
    - [macOS](#macos)
    - [Linux](#linux)
  - [Использование](#использование)
    - [Существующие инструменты](#существующие-инструменты)
  - [Запуск модели](#запуск-модели)
    - [Локальный запуск модели](#локальный-запуск-модели)
      - [Полезные флаги запуска](#полезные-флаги-запуска)
    - [Hugging Face](#hugging-face)
    - [Запуск моделей из Ollama](#запуск-моделей-из-ollama)

`llama.cpp` - это высокопроизводительная реализация модели LLaMA от Meta, написанная на C++ Георги Гергановым. `llama.cpp` позволяет запускать модели LLaMA на широком спектре устройств, от мощных серверов до мобильных устройств.

Сайт проекта:

- [GitHub](https://github.com/ggml-org/llama.cpp)
- [Официальный сайт](https://llama-cpp.com)

## Установка

llama.cpp зарегистрирован во множестве репозиториев, что облегчает его установку и использование на различных платформах.

### Windows

Под Windows можно использовать `winget`:

```cmd
winget install llama.cpp
```

Любители MSYS2 могут установить `llama.cpp` через `pacman`:

```bash
pacman -S mingw-w64-x86_64-llama.cpp
# или pacman -S mingw-w64-clang-x86_64-llama.cpp
```

### macOS

На macOS можно использовать `brew`:

```bash
brew install llama.cpp
```

### Linux

Установка на Linux предполагает скачивание и компиляцию исходного кода:

```bash
sudo apt update
sudo apt install -y git build-essential cmake
# sudo apt install libopenblas-dev   # (1) Ubuntu/Debian for OpenBLAS support
# sudo apt install vulkan-tools libvulkan-dev  # (2) Ubuntu/Debian for Vulkan support
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
mkdir build
cmake -B build
# Для NVIDIA GPU (CUDA):
# cmake -B build -DGGML_CUDA=ON
# Для OpenBLAS:
# cmake -B build -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=OpenBLAS # (1)
# Для Vulkan:
# cmake -B build -DGGML_VULKAN=ON # (2)
cmake --build build --config Release
```

## Использование

### Существующие инструменты

- `llama-cli` - это командная строка для запуска моделей LLaMA, предоставляемая `llama.cpp`. Она позволяет пользователям легко запускать модели и генерировать текст, используя различные параметры и настройки.
- `llama-server` - это серверная реализация, которая позволяет запускать модели LLaMA в виде API, что может быть полезно для интеграции с другими приложениями и сервисами.
- `llama-quantize` - инструмент для квантования моделей, позволяющий сжимать веса (например, из F16 в Q4_K_M) для уменьшения потребления памяти и ускорения работы.
- `llama-convert-hf` - утилита для конвертации моделей из формата Hugging Face в формат GGUF.
- `llama-bench` - инструмент для замера производительности модели на конкретном оборудовании.
- `llama-tokenize` - позволяет анализировать процесс токенизации текста моделью.
- `llama-perplexity` - вычисляет перплексию модели для оценки её качества на определенном наборе данных.
- `llama-grammar-gen` - инструмент для создания GBNF-грамматик, ограничивающих вывод модели строго определенным форматом (например, JSON).

## Запуск модели

### Локальный запуск модели

После установки `llama.cpp`, можно запустить модель LLaMA, указав путь к файлу модели в формате **GGUF** и передав текст для генерации.

> [!IMPORTANT]
> Формат `.bin` является устаревшим. Для работы с современными версиями `llama.cpp` используйте только модели в формате `.gguf`.

Пример базового запуска:

```bash
./llama-cli -m /path/to/model.gguf -p "Привет, как дела?"
```

#### Полезные флаги запуска

Для оптимизации работы модели используйте следующие параметры:

- `-ngl <число>` (n-gpu-layers) — количество слоев, переносимых на GPU. Установите высокое значение (например, 32 или 64), чтобы максимально ускорить генерацию за счет видеокарты.
- `-c <число>` (ctx-size) — размер контекстного окна (количество токенов). По умолчанию может быть малым, увеличьте его для работы с длинными текстами.
- `-n <число>` (n-predict) — максимальное количество токенов в ответе.
- `--temp <число>` — температура (креативность). Чем ниже, тем более точным и предсказуемым будет ответ.

Пример продвинутого запуска с использованием GPU:

```bash
./llama-cli -m /path/to/model.gguf -ngl 32 -c 4096 -p "Напиши статью про ИИ"
```

### Hugging Face

`llama.cpp` поддерживает модели, загруженные с Hugging Face. Для этого необходимо сначала скачать модель в формате GGUF, а затем указать путь к ней при запуске `llama-cli`. Например:

```bash
# Скачиваем модель с Hugging Face
wget https://huggingface.co/ggml/llama-7b/resolve/main
# Запускаем модель через llama.cpp
./llama-cli -m /path/to/gguf/model.gguf -p "Привет, как дела?"
```

Можно скачать и запустить модели с Hugging Face, используя ключ `-hf`:

```bash
./llama-cli -hf ggml-org/gemma-3-1b-it-GGUF -p "Привет, как дела?"
```

В этом случае необходимо указать имя модели в формате `username/model-name`, и `llama.cpp` автоматически загрузит её с Hugging Face и запустит.

### Запуск моделей из Ollama

После установки `llama.cpp`, можно использовать его для запуска моделей, загруженных в Ollama. Для этого необходимо указать путь к модели и запустить `llama.cpp` с соответствующими параметрами. Чтобы запустить модель из Ollama через llama.cpp, нужно найти файл модели (blob) и указать путь к нему.

1. Определить путь к модели в Ollama. Для этого необходимо вывести содержимое modelfile для нужной модели:

```bash
$ ollama list
NAME                        ID              SIZE      MODIFIED
qwen3.5:397b-cloud          a7bf6f7891c3    -         11 hours ago
qwen3.5:9b                  6488c96fa5fa    6.6 GB    3 days ago
gemma3:27b-cloud            9e1580299085    -         4 days ago
deepseek-ocr:latest         0e7b018b8a22    6.7 GB    5 days ago
qwen3-vl:8b                 901cae732162    6.1 GB    6 days ago

$ ollama show deepseek-ocr:latest --modelfile
FROM ~/.ollama/models/blobs/sha256-3a18673ff291a1d8de94d490877127899356d33a18028d5f3945bf245c11b02c
...
```

Строка, начинающаяся с `FROM` указывает путь к файлу модели, который можно использовать с `llama.cpp`. Для этого необходимо скопировать этот путь и использовать его при запуске модели через `llama.cpp`. Например:

```bash
./llama-cli -m ~/.ollama/models/blobs/sha256-3a1867... -p "Привет!"
```
