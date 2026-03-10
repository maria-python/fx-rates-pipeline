# Currency ETL Pipeline 

Проект реализует ETL-пайплайн для загрузки курсов валют из API Европейского центрального банка (ECB), трансформации данных и записи результата в ClickHouse с оркестрацией через Apache Airflow.

В рамках задания реализованы два DAG:
- `maintenance_currency_dag` — для загрузки данных за указанный период
- `integration_currency_dag` — для ежедневной загрузки данных за предыдущий день


## What the Project does

### Data Sourse
Используется API Европейского центрального банка (ECB) для получения курсов валют.

### Currency
В проекте обрабатываются данные для:
- USD
- EUR

### Result
После обработки данные приводятся к формату, подходящему для записи в таблицу `currency` в ClickHouse.

Каждая строка содержит:
- `id` — уникальный UUID
- `date` — дата курса
- `usd` — всегда `1.0`
- `euro` — значение EUR по отношению к USD
- `created` — время создания записи
- `updated` — `NULL`


## Pipeline Architecture

```
ECB API
   ↓
Airflow DAG: maintenance_currency_dag
   ↓
fetch → transform
   ↓
Airflow DAG: integration_currency_dag
   ↓
insert_batch()
   ↓
ClickHouse
```

## Tech Stack

- Python

- Apache Airflow

- ClickHouse

- Docker / Docker Compose

- Pandas

- Requests


## Project Structure

```
tech_task1
│
├── dags
│   ├── integration_dag.py
│   └── maintenance_dag.py
│
├── src
│   ├── __init__.py
│   ├── api
│   │   ├── __init__.py
│   │   └── ecb_client.py
│   ├── processing
│   │   ├── __init__.py
│   │   └── transform.py
│   └── db
│       ├── __init__.py
│       └── clickhouse_client.py
│
├── sql
│   └── currency_table.sql
│
├── screenshots
│   ├── integration.png
│   ├── maintenance.png
│   ├── airflow_dag_success.png
│   └── data_stored_in_clickhouse.png
│   └── data_stored_in_clickhouse2.png
│
├── docker-compose.yml
├── requirements.txt
├── test_ecb_rates.py
└── README.md
```

