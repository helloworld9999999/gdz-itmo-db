1. Найти и вывести на экран количество товаров каждого цвета, исключив из поиска товары, цена которых меньше 30.
```
SELECT product.color, COUNT(*) FROM production.product
WHERE product.listprice >= 30 
GROUP BY product.color
```

2. Найти и вывести на экран список, состоящий из цветов товаров, таких, что минимальная цена товара данного цвета более 100.
```
SELECT p.color
FROM production.product AS p
GROUP BY p.color
HAVING MIN(p.listprice) > 100
```

3. Найти и вывести на экран номера подкатегорий товаров и количество товаров  в каждой подкатегории.
```
SELECT p.productsubcategoryid, count(*)
FROM production.product AS p
GROUP BY p.productsubcategoryid
```
4. Найти и вывести на экран номера товаров и количество фактов продаж данного товара (используется таблица SalesORDERDetail).
```
SELECT s.productid, count(*)
FROM sales.sales_order_detail AS s
GROUP BY s.productid
```

5. Найти и вывести на экран номера товаров, которые были куплены более пяти раз.
```
SELECT s.productid, count(*)
FROM sales.sales_order_detail AS s
GROUP BY s.productid
HAVING count(*) > 5
```

6. Найти и вывести на экран номера покупателей, CustomerID, у которых существует более одного чека, SalesORDERID, с одинаковой датой
```
SELECT customerid
FROM sales.sales_order_header
GROUP BY customerid, orderdate
HAVING COUNT(salesorderid) > 1
```

7. Найти и вывести на экран все номера чеков, на которые приходится более трех продуктов.
```
SELECT salesorderid
FROM sales.sales_order_detail
GROUP BY salesorderid
HAVING COUNT(productid) > 3;
```

8. Найти и вывести на экран все номера продуктов, которые были куплены более трех раз.
```
SELECT productid
FROM sales.sales_order_detail
GROUP BY productid
HAVING COUNT(salesorderid) > 3;
```

9. Найти и вывести на экран все номера продуктов, которые были куплены или три или пять раз.
```
SELECT productid
FROM sales.sales_order_detail
GROUP BY productid
HAVING COUNT(salesorderid) IN (3, 5);
```

10. Найти и вывести на экран все номера подкатегорий, к которым относится более десяти товаров.
```
SELECT productsubcategoryid
FROM production.product
GROUP BY productsubcategoryid
HAVING COUNT(productid) > 10;
```

11. Найти и вывести на экран номера товаров, которые всегда покупались в одном экземпляре за одну покупку.
```
SELECT productid
FROM sales.sales_order_detail
WHERE orderqty = 1
GROUP BY productid
```

12. Найти и вывести на экран номер чека, SalesORDERID, на который приходится с наибольшим разнообразием товаров купленных на этот чек.
```
SELECT salesorderid
FROM sales.sales_order_detail
GROUP BY salesorderid
ORDER BY count(DISTINCT productid) DESC
LIMIT 1;
```

13. Найти и вывести на экран номер чека, SalesORDERID с наибольшей суммой покупки, исходя из того, что цена товара – это UnitPrice, а количество конкретного товара в чеке – это ORDERQty.
```
SELECT salesorderid
FROM sales.sales_order_detail
GROUP BY salesorderid
ORDER BY SUM(orderqty * unitprice) DESC
LIMIT 1;
```

14. Определить количество товаров в каждой подкатегории, исключая товары, для которых подкатегория не определена, и товары, у которых не определен цвет.
```
SELECT productsubcategoryid, count(*)
FROM production.product
WHERE color IS NOT NULL AND productsubcategoryid IS NOT NULL
GROUP BY productsubcategoryid;
```

15. Получить список цветов товаров в порядке убывания количества товаров данного цвета
```
SELECT color
FROM production.product
GROUP BY color
ORDER BY count(productid) DESC;
```

16. Вывести на экран ProductID тех товаров, что всегда покупались в количестве более 1 единицы на один чек, при этом таких покупок было более двух.
```
SELECT productid
FROM sales.sales_order_detail
WHERE orderqty > 1
GROUP BY productid
HAVING COUNT(salesorderid) > 2;
```