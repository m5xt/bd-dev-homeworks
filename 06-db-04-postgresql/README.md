# Домашнее задание к занятию 4. «PostgreSQL»

## Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL, используя `psql`.

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:

- вывода списка БД,
- 
  \l+
- подключения к БД,
- 
  \conninfo
- вывода списка таблиц,
- 
  \dtS
- вывода описания содержимого таблиц,
- 
  \dS+
- выхода из psql
- 
  \q

## Задача 2

Используя `psql`, создайте БД `test_database`.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/virt-11/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.

Перейдите в управляющую консоль `psql` внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

**Приведите в ответе** команду, которую вы использовали для вычисления, и полученный результат.

![alt text](https://github.com/m5xt/bd-dev-homeworks/blob/main/06-db-04-postgresql/dz-2.png)



## Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам как успешному выпускнику курсов DevOps в Нетологии предложили
провести разбиение таблицы на 2: шардировать на orders_1 - price>499 и orders_2 - price<=499.

Предложите SQL-транзакцию для проведения этой операции.

CREATE TABLE orders_1 (CHECK (price > 499)) INHERITS (orders);

INSERT INTO orders_1 SELECT * FROM orders WHERE price > 499;

CREATE TABLE orders_2 (CHECK (price <= 499)) INHERITS (orders);

INSERT INTO orders_2 SELECT * FROM orders WHERE price <= 499;



CREATE TABLE orders_1 PARTITION OF orders FOR VALUES GREATER THAN ('499');

CREATE TABLE orders_2 PARTITION OF orders FOR VALUES FROM ('0') TO ('499');


Можно ли было изначально исключить ручное разбиение при проектировании таблицы orders?

Автоматическое разбиение было можно ностроить правилами для шардирования таблицы на этапе проектирвоаниея системы


## Задача 4

Используя утилиту `pg_dump`, создайте бекап БД `test_database`.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

CREATE TABLE public.orders (
 id integer NOT NULL,
 title character varying(80) NOT NULL UNIQUE );
