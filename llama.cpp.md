# llama.cpp

`llama.cpp` - это высокопроизводительная реализация модели LLaMA от Meta, написанная на C++ Георги Гергановым. `llama.cpp` позволяет запускать модели LLaMA на широком спектре устройств, от мощных серверов до мобильных устройств.

Сайт проекта:

- [GitHub](https://github.com/ggml-org/llama.cpp)
- [Официальный сайт](https://llama-cpp.com)

## Установка

llama.cpp зарегистрирован во множестве репозиториев, что облегчает его установку и использование на различных платформах.

Под Windows можно использовать `winget`:

```cmd
winget install llama.cpp
```

На macOS можно использовать `brew`:

```bash
brew install llama.cpp
```

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
# cmake -B build -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=OpenBLAS # (1)
# cmake -B build -DGGML_VULKAN=ON # (2)
cmake --build build --config Release
```

## Использование

### Запуск модели

После установки `llama.cpp`, можно запустить модель LLaMA, указав путь к файлу модели и передав текст для генерации. Например:

```bash
./llama-cli -m /path/to/model.bin -p "Привет, как дела?"
```

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
