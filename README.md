```markdown
# Weather Microservice

Микросервис для получения прогноза погоды с веб-интерфейсом, хранением истории запросов и визуализацией статистики.

---

## 🧩 Функционал

### Backend (FastAPI)
- **Текущая погода и прогноз** — на сегодня / завтра с разбивкой по частям суток (утро, день, вечер, ночь).
- **История запросов** — сохранение каждого запроса пользователя с полным ответом API.
- **Статистика** — общее количество запросов и топ‑3 самых частых города.
- **Кэширование** — in-memory кэш с TTL 5 минут для снижения нагрузки на внешний API.
- **Ретраи** — автоматические повторные попытки при сбоях внешнего API (3 попытки с экспоненциальной задержкой).
- **Асинхронность** — неблокирующая обработка запросов.

### Frontend (Streamlit)
- **Аутентификация** — по числовому ID (валидация > 0).
- **Поиск погоды** — выбор периода (сейчас / сегодня / завтра) и части суток.
- **История запросов** — последние 10 запросов с датами.
- **Статистика** — столбчатая диаграмма топ‑3 городов (Altair) и скачивание статистики в `.txt`.
- **Управление состоянием** — вкладки, очистка истории, обратная связь.

---

## 🛠️ Стек технологий

| Компонент | Технологии |
|-----------|------------|
| **Backend** | Python 3.11, FastAPI, asyncpg, SQLAlchemy (Alembic), Pydantic, httpx, geopy, tenacity |
| **Frontend** | Python 3.11, Streamlit, Altair, Pandas, httpx |
| **Database** | PostgreSQL 15 |
| **Инфраструктура** | Docker, Docker Compose, GitHub Actions (CI/CD), VPS |

---

## 🚀 Запуск

### Через Docker (рекомендуется)
```bash
docker-compose up --build
```

После запуска:
- Backend: `http://localhost:8000`
- Frontend: `http://localhost:8501`
- Swagger-документация: `http://localhost:8000/docs`

### Без Docker (локально)
1. Установи зависимости:
```bash
pip install -r backend/requirements.txt
pip install -r frontend/requirements.txt
```

2. Настрой `.env` (см. `.env.example`).

3. Запусти:
```bash
python run.py
```

Или вручную в двух терминалах:
```bash
# Терминал 1
uvicorn backend.app.main:app --reload

# Терминал 2
streamlit run frontend/dashboard.py
```

---

## 🔐 Переменные окружения

Создай файл `.env` в корне проекта:

```env
# Яндекс.Погода
YANDEX_ACCESS_KEY=ваш_ключ

# PostgreSQL
DB_HOST=localhost        # в Docker — postgres
DB_PORT=5432
DB_USER=ваш_пользователь
DB_PASSWORD=ваш_пароль
DB_NAME=queries
```

---

## 📁 Структура проекта

```
.
├── backend/
│   ├── app/
│   │   ├── main.py          # Эндпоинты FastAPI
│   │   ├── forecaster.py    # Логика запросов к API и кэширование
│   │   ├── db.py            # Работа с PostgreSQL (asyncpg)
│   │   └── utils.py         # Парсинг, форматирование, эмодзи
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/
│   ├── dashboard.py         # Главный интерфейс Streamlit
│   ├── api_client.py        # Клиент для запросов к бэкенду
│   ├── Dockerfile
│   └── requirements.txt
├── docker-compose.yml
├── .env.example
├── .gitignore
├── run.py                   # Универсальный запуск
├── run_windows.bat
├── run_ubuntu.sh
└── README.md
```

---

## 🧪 Тестирование

```bash
pytest test_backend.py -v
```

Покрыты:
- `/weather` (успешный ответ)
- `/history` (возврат последних запросов)
- `/stats` (статистика с топ-городами)

---

## 🌍 Деплой

При желании проект может быть легко задеплоен на вашем VPS

---

## 📌 Планы по развитию

- Redis для кэширования вместо in-memory словаря
- Расширение тестового покрытия
- Добавление прогноза на неделю

---

**Автор:** Сафронов Владимир  
