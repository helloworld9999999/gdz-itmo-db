Найти и вывести на экран название продуктов и название категорий товаров, к которым относится этот продукт, с учетом того, что в выборку попадут только товары с цветом Red и ценой не менее 100.
```
SELECT p.name, pc.name
FROM production.product AS p
JOIN production.product_subcategory AS psc
ON p.product_subcategory_id = psc.product_subcategory_id
JOIN production.product_category AS pc
ON psc.product_category_id = pc.product_category_id
WHERE p.color = 'Red' AND p.list_price >= 100;
```
Вывести на экран названия подкатегорий с совпадающими именами.
```
SELECT psc.name
FROM production.product_subcategory AS psc
JOIN production.product_subcategory AS psc2
ON psc.name = psc2.name
AND psc.product_subcategory_id <> psc2.product_subcategory_id;
```
Вывести на экран название категорий и количество товаров в данной категории.
```
SELECT pc.name, COUNT(p.product_id)
FROM production.product AS p
JOIN production.product_subcategory AS psc
ON p.product_subcategory_id = psc.product_subcategory_id
JOIN production.product_category AS pc
ON psc.product_category_id = pc.product_category_id
GROUP BY (pc.name);
```
Вывести на экран название подкатегории, а также количество товаров в данной подкатегории с учетом ситуации, что могут существовать подкатегории с одинаковыми именами.
```
SELECT psc.name, COUNT(p.product_id)
FROM production.product AS p
JOIN production.product_subcategory AS psc 
ON p.product_subcategory_id = psc.product_subcategory_id
GROUP BY psc.name, psc.product_subcategory_id;
```
Вывести на экран название первых трех подкатегорий с небольшим количеством товаров.
```
SELECT psc.name, COUNT(p.product_id) AS total
FROM production.product AS p
JOIN production.product_subcategory AS psc 
ON p.product_subcategory_id = psc.product_subcategory_id
GROUP BY psc.name, psc.product_subcategory_id
ORDER BY total DESC
LIMIT 3;
```
Вывести на экран название подкатегории и максимальную цену продукта с цветом Red в этой подкатегории.
```
SELECT psc.name, MAX(p.list_price)
FROM production.product AS p
JOIN production.product_subcategory AS psc
ON p.product_subcategory_id = psc.product_subcategory_id
WHERE p.color = 'Red'
GROUP BY (psc.name);
```
Вывести на экран название поставщика и количество товаров, которые он поставляет.
```
SELECT v.name, COUNT(p.product_id)
FROM purchasing.vendor AS v
JOIN purchasing.product_vendor AS pv
ON v.business_entity_id = pv.business_entity_id
JOIN production.product AS p
ON pv.product_id = p.product_id
GROUP BY (v.name);
```
Вывести на экран название товаров, которые поставляются более чем одним поставщиком.
```
SELECT p.name
FROM production.product AS p
JOIN purchasing.product_vendor AS pv
ON p.product_id = pv.product_id
GROUP BY (p.name)
HAVING COUNT(pv.business_entity_id) > 1
```
Вывести на экран название самого продаваемого товара.
```
SELECT p.name, COUNT(sod.order_qty) AS total
FROM production.product AS p
JOIN sales.sales_order_detail AS sod
ON sod.product_id = p.product_id
GROUP BY (p.name)
ORDER BY total DESC
LIMIT 1;
```
Вывести на экран название категории, товары из которой продаются наиболее активно.
```
SELECT pc.Name, SUM(sod.order_qty)
FROM sales.sales_order_detail AS sod
JOIN production.product AS p 
ON sod.product_id = p.product_id
JOIN production.product_subcategory ps
ON p.product_subcategory_id = ps.product_subcategory_id
JOIN production.product_category pc 
ON ps.product_category_id = pc.product_category_id
GROUP BY pc.name
ORDER BY SUM(sod.order_qty) DESC
LIMIT 1;
```
Вывести на экран названия категорий, количество подкатегорий и количество товаров в них.
```
SELECT pc.name, COUNT(DISTINCT psc.product_subcategory_id), COUNT(DISTINCT p.product_id)
FROM production.product AS p
JOIN production.product_subcategory AS psc
ON psc.product_subcategory_id = p.product_subcategory_id
JOIN production.product_category AS pc
ON psc.product_category_id = pc.product_category_id
GROUP BY (pc.name)
```
Вывести на экран номер кредитного рейтинга и количество товаров, поставляемых компаниями, имеющими этот кредитный рейтинг.
```
SELECT v.credit_rating AS cr, COUNT(p.product_id)
FROM purchasing.vendor AS v
JOIN purchasing.product_vendor AS pv
ON pv.business_entity_id = v.business_entity_id
JOIN production.product AS p
ON pv.product_id = p.product_id
GROUP BY (cr);
```
Найти первые 10 процентов самых дорогих товаров, с учетом ситуации, когда цены у некоторых товаров могут совпадать.
```
SELECT p.name, p.list_price
FROM production.product AS p
ORDER BY p.list_price DESC
LIMIT (SELECT COUNT(*) * 0.1 FROM production.product);
```
Найти первых трех поставщиков, отсортированных по количеству поставляемых товаров, с учетом ситуации, что количество поставляемых товаров может совпадать для разных поставщиков.
```
SELECT v.name, COUNT(p.product_id) AS total
FROM purchasing.vendor AS v
JOIN purchasing.product_vendor AS pv
ON v.business_entity_id = pv.business_entity_id
JOIN production.product AS p
ON pv.product_id = p.product_id
GROUP BY (v.name)
ORDER BY total DESC, v.name ASC
LIMIT 3;
```
Найти для каждого поставщика количество подкатегорий продуктов, к которым относится продукты, поставляемые им, без учета ситуации, когда продукт не относится ни к какой подкатегории.
```
SELECT v.name, COUNT(DISTINCT ps.product_subcategory_id) AS subcategory_count
FROM purchasing.vendor AS v
JOIN purchasing.product_vendor AS pv
ON v.business_entity_id = pv.business_entity_id
JOIN production.product AS p
ON pv.product_id = p.product_id
JOIN production.product_subcategory AS ps
ON p.product_subcategory_id = ps.product_subcategory_id
GROUP BY v.name;
```
Проверить, есть ли продукты с одинаковым названием, если есть, то вывести эти названия. (Решение через JOIN)
```
SELECT p1.name
FROM production.product AS p1
JOIN production.product AS p2
ON p1.name = p2.name
AND p1.product_id <> p2.product_id;
```
На сдаче:
```
SELECT p.name
FROM production.product AS p
JOIN sales.sales_order_detail AS sod
ON p.product_id = sod.product_id
GROUP BY p.name
ORDER BY COUNT(sod.sales_order_id) ASC;
```