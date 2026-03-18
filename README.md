# Тестовое задание - реализация RAG - системы

## Цель проекта
Проект предназначен для поиска ответов по PDF-документу с помощью RAG-пайплайна.

Что делает решение:
- рендерит страницы PDF как изображения;
- извлекает текст через OCR (`rus+eng`);
- разбивает текст на чанки;
- индексирует чанки в Qdrant;
- выполняет retrieval;
- передаёт найденный контекст в LLM для генерации краткого ответа с указанием страниц.

## Структура пайплайна
1. OCR страниц PDF через `PyMuPDF + Tesseract`
2. Разбиение текста через `RecursiveCharacterTextSplitter`
3. Индексация в локальный `Qdrant`
4. Многоязычный retrieval (`RU + EN`)
5. Генерация ответа через `ChatOpenAI`
6. Оценка качества ответа и retrieval методом llm-as-judge

## Требования
- Python 3.10+
- macOS с Homebrew
- `uv`
- Tesseract с языками `eng` и `rus`

## Установка `uv`
Если `uv` ещё не установлен:

```bash
brew install -qU uv
```

Проверка:

```bash
uv --version
```

## Клонирование и переход в проект
```bash
git clone <repo_url>
cd <repo_folder>
```

## Синхронизация окружения
В `pyproject.toml` указаны зависимости, установливаем так:

```bash
uv sync
```

Также желательно установить dev-зависимости:

```bash
uv sync --dev
```

## Установка системных зависимостей
Для OCR нужны Tesseract и языковые пакеты:

```bash
brew install -qU tesseract
brew install -qU tesseract-lang
```

Проверка доступных языков:

```bash
tesseract --list-langs
```

В списке должны быть `eng` и `rus`.

## Переменные окружения
Создай `.env` в корне проекта:

```env
OPENAI_API_KEY=your_openai_key
LANGSMITH_TRACING=true
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGSMITH_API_KEY=your_langsmith_key
LANGSMITH_PROJECT=rag_test
```

## Запуск
Запускаем Jupyter lab или notebook:

```bash
uv run jupyter lab
```

или

```bash
uv run jupyter notebook
```

## Первый запуск пайплайна
Обычно последовательность такая:
1. Открыть PDF
2. Выполнить OCR страниц
3. Разбить текст на чанки
4. Добавить чанки в Qdrant
5. Запустить retrieval и генерацию ответа

Важно: если коллекция Qdrant ещё пустая, перед запросами нужно выполнить индексацию документов.

## Полезные команды
Установить новую зависимость:

```bash
uv add <package_name>
```

Установить dev-зависимость:

```bash
uv add --dev <package_name>
```


## Краткое описание результата
На вход подаётся вопрос пользователя по документу.  
На выходе система возвращает:
- краткий ответ;
- ссылки на страницы документа.


