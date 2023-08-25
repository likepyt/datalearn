# Задание для модуля 2

## Установка БД
Установка postgres и dbeaver cloud в контейнерах докер. 
Файл docker-compose:
```
version: "3.9"
      
services:
      
  postgres:
    image: postgres:14.8-alpine3.18
    container_name: postgres
    environment:
      POSTGRES_DB: "test"
      POSTGRES_USER: "test"
      POSTGRES_PASSWORD: "test"
      PGDATA: "/var/lib/postgresql/data/pgdata"
    volumes:
      - ./data_pg:/var/lib/postgresql/data
      - ./airflow_db_init:/docker-entrypoint-initdb.d
    ports:
      - "5432:5432" 
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "airflow"]
      interval: 10s
      retries: 5
      start_period: 5s
    restart: always
      
  dbeaver:
    image: dbeaver/cloudbeaver:23.1.1
    container_name: dbeaver
    ports:
      - "8090:8978"
    volumes:
      - ./data_dbeaver:/opt/cloudbeaver/workspace
```
## Загрузка данных в БД
Сохранение файла в формате csv.
![Alt text](https://github.com/likepyt/datalearn/blob/main/de101/module-02/save-csv.png)
Загрузка данных в базу при помощи инструмента импорта в dbeaver.
![Alt text](https://github.com/likepyt/datalearn/blob/main/de101/module-02/import-csv.png)
## SQL запросы

1. Overview (обзор ключевых метрик)
  - Total Sales 
      ```sql
      select 
	      sum(o.sales)
      from 
	      public.orders o
      ```
  - Total Profit
  - Profit Ratio
  - Profit per Order
  - Sales per Customer
  - Avg. Discount
  - Monthly Sales by Segment ( табличка и график)
  - Monthly Sales by Product Category (табличка и график)
 2. Product Dashboard (Продуктовые метрики)
  - Sales by Product Category over time (Продажи по категориям)
 3. Customer Analysis
  - Sales and Profit by Customer
  - Customer Ranking
  - Sales per region

## Нарисовать модель данных в SQLdbm

## Нарисовать графики в Google Sheets

## Нарисовать графики в KlipFolio
