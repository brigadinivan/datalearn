
2.2: Что такое базы данных и как они помогают при работе с данными
- Вам необходимо установить Postgres базу данных к себе на компьютер. Вы можете посмотреть инструкции по установки Postgres.
## выполнено. Postgres установлен



2.3: Подключение к Базам Данных и SQL

- Вам необходимо установить клиент SQL для подключения базы данных. Вы можете посмотреть инструкции по установки DBeaver. Так же вы можете использовать любой другой клиент для подключения к ваше БД. 
## выполнено. DBeaver установлен

Создайте 3 таблицы и загрузите данные из Superstore Excel файл в вашу базу данных. Сохраните в вашем GitHub скрипт загрузки данных и создания таблиц. Вы можете использовать готовый пример sql файлов.
## выполнено

Напишите запросы, чтобы ответить на вопросы из Модуля 01.

Total Sales (общая выручка)

select sum(sales) as total_sales             
from orders;

total_sales |
------------+
2297200.8603|

Total Profit (общая прибыль)
select sum(profit) as total_profit 
from orders;total_profit           |

total_profit           |
-----------------------+
286397.0216999999887055|

сумма прибыли по категориям товаров
select category, sum(profit)
FROM public.orders
group by category;

category       |sum                    |
---------------+-----------------------+
Furniture      | 18451.2727999999933410|
Office Supplies|122490.8007999999985225|
Technology     |145454.9480999999968420|


Profit Ratio (коэффициент прибыли) profit_ratio = total_profit/total_sales

SELECT 
sum(profit)/sum(sales) as profit_ratio
FROM public.orders;

profit_ratio          |
----------------------+
0.12467217240315604661|

Profit per Order (прибыль на заказ)

SELECT order_id,
sum(profit) as profit_per_order
FROM public.orders
group by order_id
order by profit_per_order desc 
limit 5;

order_id      |profit_per_order     |
--------------+---------------------+
CA-2018-118689|8762.3891000000000000|
CA-2019-140151|6734.4720000000000000|
CA-2019-166709|5039.9856000000000000|
CA-2018-117121|4946.3700000000000000|
CA-2016-116904|4668.6935000000000000|

Sales per Customer (Продажи на клиента)

SELECT customer_id, customer_name,
sum(sales) as sales_per_customer
FROM public.orders
group by customer_id, customer_name 
order by sales_per_customer desc 
limit 5;

customer_id|customer_name|sales_per_customer|
-----------+-------------+------------------+
SM-20320   |Sean Miller  |        25043.0500|
TC-20980   |Tamara Chand |        19052.2180|
RB-19360   |Raymond Buch |        15117.3390|
TA-21385   |Tom Ashbrook |        14595.6200|
AB-10105   |Adrian Barton|        14473.5710|

Avg. Discount (Сред. Скидка)

select avg(discount) as average_discount
FROM public.orders;

average_discount      |
----------------------+
0.15620272163297978787|

Monthly Sales by Segment (Ежемесячные продажи по сегментам)
-- сначала общие продажи по сегментам

select segment, count(sales) 
FROM public.orders
group by segment;

segment    |count|
-----------+-----+
Consumer   | 5191|
Corporate  | 3020|
Home Office| 1783|


select segment, count(sales) as quantity_sales,
round(sum(sales),1) as total_income,
round(sum(profit),1) as revenue 
FROM public.orders
group by segment;

segment    |quantity_sales|total_income|revenue |
-----------+--------------+------------+--------+
Consumer   |          5191|   1161401.3|134119.2|
Corporate  |          3020|    706146.4| 91979.1|
Home Office|          1783|    429653.1| 60298.7|


выборкf по годам

select segment,
count(sales) as quantity_sales,
case	
when extract ('year' from order_date) = 2016 then '2016'
when extract ('year' from order_date) = 2017 then '2017'
when extract ('year' from order_date) = 2018 then '2018'
when extract ('year' from order_date) = 2019 then '2019'	
end
FROM public.orders
group by segment, order_date
order by order_date;


select segment, count(sales) as count_sales,
extract(year from order_date) as year
FROM public.orders
group by segment, year
order by year, segment;

segment    |count_sales|year|
-----------+-----------+----+
Consumer   |       1070|2016|
Corporate  |        611|2016|
Home Office|        312|2016|
Consumer   |       1125|2017|
Corporate  |        636|2017|
Home Office|        341|2017|
Consumer   |       1328|2018|
Corporate  |        793|2018|
Home Office|        466|2018|
Consumer   |       1668|2019|
Corporate  |        980|2019|
Home Office|        664|2019|
 

Year Sales by Product Category (Годовые продажи по категориям продуктов)

select category, count(sales) as count_sales,
extract(year from order_date) as year
FROM public.orders
group by category, year
order by year, category;

category       |count_sales|year|
---------------+-----------+----+
Furniture      |        421|2016|
Office Supplies|       1217|2016|
Technology     |        355|2016|
Furniture      |        452|2017|
Office Supplies|       1241|2017|
Technology     |        409|2017|
Furniture      |        562|2018|
Office Supplies|       1566|2018|




 




