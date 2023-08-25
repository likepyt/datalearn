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
    ```sql
    select 
	sum(o.profit)
    from 
	public.orders o
    ```
  - Profit Ratio
    ```sql
    select 
	sum(o.sales) / sum(o.profit)
    from 
	public.orders o
    ```
  - Profit per Order
    ```sql
    select 
	sum(o.profit) / count(distinct o.order_id)
    from 
	public.orders o
    ```
  - Sales per Customer
    ```sql
    select
    	  o.customer_name
    	, sum(o.sales)
    from 
	public.orders o
    group by
    	customer_name
    ```
  - Avg. Discount
    ```sql
    select 
    	avg(o.discount)
    from 
	public.orders o
    ```    
  - Monthly Sales by Segment
    ```sql
    select
	  extract(year from o.order_date)
	, extract(month from o.order_date)
	, o.segment 
	, sum(o.sales)
     from 
	public.orders o
     group by
	  extract(year from o.order_date)
	, extract(month from o.order_date)
	, o.segment
    ``` 
  - Monthly Sales by Product Category
    ```sql
    select
	  extract(year from o.order_date)
	, extract(month from o.order_date)
	, o.category
	, sum(o.sales)
    from 
	public.orders o
    group by
	  extract(year from o.order_date)
	, extract(month from o.order_date)
	, o.category
    ```
    
 2. Product Dashboard
  - Sales by Product Category over time
    ```sql
    select
	  o.category
	, sum(o.sales)
    from 
	public.orders o
    group by
	o.category
    ```
 3. Customer Analysis
  - Sales and Profit by Customer
    ```sql
    select
	  o.customer_name
	, sum(o.sales)
    	, sum(o.profit)
     from 
	public.orders o
     group by
	  o.customer_name
    ```
  - Customer Ranking
    ```sql
    select
	  o.customer_name
	, sum(o.sales)
	, row_number() over(order by sum(o.sales) desc)
     from 
	public.orders o
     group by 
	o.customer_name
     order by 
	sum(o.sales) desc
    ```
  - Sales per region
    ```sql
    select
	  o.region
	, sum(o.sales)
     from 
	public.orders o
     group by 
	o.region
    ```
    
## Нарисовать модель данных в SQLdbm

Концептуальная модель данных:
![Alt text](https://github.com/likepyt/datalearn/blob/main/de101/module-02/concept-model.png)

Физическая модель данных:
![Alt text](https://github.com/likepyt/datalearn/blob/main/de101/module-02/phyz-model.png)

## Нарисовать графики в Google Sheets

![Alt text](https://github.com/likepyt/datalearn/blob/main/de101/module-02/dashboard.png)
