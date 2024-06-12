# Домашнее задание к занятию "`Базы данных`" - `Сайфиев Денис`


### Задание 1.

Легенда
Заказчик передал вам файл в формате Excel , в котором сформирован отчёт.

На основе этой отчёта необходимо выполнить следующее задание.

Запишите не менее семи таблиц, из которых состоит база данных:

какие данные хранятся в этих таблицах;
Какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
Приняв решение к следующему мнению:

Сотрудники (

идентификатор, первичный ключ, серийный номер,
фамилия варчар(50),
...
идентификатор структурного подразделения, внешний ключ, целое число).


### Решение 1

 
### СОТРУДНИКИ (
* идентификатор сотрудника, первичный ключ, serial,
*	Ф.И.О. VARCHAR(60),
*	оклад DECIMAL/NUMERIC,
*	дата найма DATE,
*	идентификатор должности, внешний ключ, INTEGER,
*	идентификатор структурного подразделения, внешний ключ, INTEGER,
*	идентификатор проекта, внешний ключ, INTEGER)

### ДОЛЖНОСТЬ (
идентификатор должности, первичный ключ, serial,
*	должность VARCHAR(50))

### ТИП ПОДРАЗДЕЛЕНИЯ (
* идентификатор типа подразделения, первичный ключ, serial,
*	тип подразделения VARCHAR(50))

### СТРУКТУРНОЕ ПОДРАЗДЕЛЕНИЕ (
* идентификатор структурного подразделения, первичный ключ, serial,
*	структурное подразделение VARCHAR(60),
*	идентификатор типа подразделения, внешний ключ, INTEGER
*	идентификатор филиала, внешний ключ, INTEGER
*	идентификатор должности, внешний ключ, INTEGER)

### ПРОЕКТ (
*	идентификатор проекта, первичный ключ, serial,
*	проект VARCHAR(50))


### АДРЕС ФИЛИАЛА (
* идентификатор филиала, первичный ключ, serial,
*	адрес филиала VARCHAR(60)
*	регион филиала, внешний ключ, INTEGER)

### ДАТА НАЙМА (
* идентификатор даты найма, первичный ключ, serial,
•	дата найма, DATE)



