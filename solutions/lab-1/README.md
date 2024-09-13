# Лабораторная 1
## Описание
В данной лабораторной работе выполнены запросы для работы с базой данных, включая выборки, фильтрацию и сортировку данных. Дополнительно реализованы примеры с использованием регулярных выражений для обработки строк.

## Дополнительные материалы
Разбор дополнительных запросов доступен [тут](./additional.md).

## Запросы
1. Найти и вывести на экран названия продуктов, их цвет и размер.
    ```
        SELECT p.name, p.color, p.size
        FROM production.product as p;
    ```
2. Найти и вывести на экран названия, цвет и размер таких продуктов, у которых цена более 100.
    ```
        SELECT p.name, p.color, p.size
        FROM production.product AS p
        WHERE p.standardcost > 100;
    ```
3. Найти и вывести на экран название, цвет и размер таких продуктов, у которых цена менее 100 и цвет Black.
    ```
        SELECT p.name, p.color, p.size
        FROM production.product AS p
        WHERE p.standardcost < 100 AND p.color='Black';
    ```
4. Найти и вывести на экран название, цвет и размер таких продуктов, у которых цена менее 100 и цвет Black, упорядочив вывод по возрастанию стоимости продуктов.
    ```
        SELECT p.name, p.color, p.size
        FROM production.product AS p
        WHERE p.standardcost < 100 AND p.color = 'Black' 
        ORDER BY p.standardcost ASC;
    ```
5. Найти и вывести на экран название и размер первых трех самых дорогих товаров с цветом Black.
    ```
        SELECT p.name, p.size
        FROM production.product AS p
        WHERE p.color = 'Black' 
        ORDER BY p.standardcost DESC
        LIMIT 3;
    ```
6. Найти и вывести на экран название и цвет таких продуктов, для которых определен и цвет, и размер.
    ```
        SELECT p.name, p.color
        FROM production.product AS p
        WHERE p.color IS NOT NULL AND p.size IS NOT NULL;
    ```
7. Найти и вывести на экран не повторяющиеся цвета продуктов, у которых цена находится в диапазоне от 10 до 50 включительно.
    ```
        SELECT DISTINCT p.color
        FROM production.product AS p
        WHERE p.standardcost BETWEEN 10 AND 50;
    ```
8. Найти и вывести на экран все цвета таких продуктов, у которых в имени первая буква ‘L’ и третья ‘N’.
    ```
        LECT p.color
        FROM production.product AS p
        WHERE p.name LIKE 'L_N%';
    ```
С использованием регулярных выражений:
```
        SELECT p.color
        FROM production.product AS p
        WHERE p.name ~ '^L.N';
```

9. Найти и вывести на экран названия таких продуктов, которых начинаются либо на букву ‘D’, либо на букву ‘M’, и при этом длина имени – более трех символов.
    ```
        SELECT p.name
        FROM production.product AS p
        WHERE p.name LIKE '(D|M)__%';
    ```
С использованием регулярных выражений:
```
        SELECT p.name
        FROM production.product AS p
        WHERE product.name ~ '^(D|M).{3,}';
```


10. Вывести на экран названия продуктов, у которых дата начала продаж – не позднее 2012 года.
    ```
        SELECT p.name
        FROM production.product AS p
        WHERE p.sellstartdate < '2012-12-31';
    ```

11. Найти и вывести на экран названия всех подкатегорий товаров.
    ```
        SELECT p.name
        FROM production.product_subcategory as p;
    ```
12. Найти и вывести на экран названия всех категорий товаров.
    ```
        SELECT p.name
        FROM production.product_category as p;
    ```
13. Найти и вывести на экран имена всех клиентов из таблицы Person, у которых обращение (Title) указано как «Mr.».
    ```
        SELECT p.firstname
        FROM person.person as p
        WHERE p.title = 'Mr.';
    ```

14. Найти и вывести на экран имена всех клиентов из таблицы Person, для которых не определено обращение (Title).       
    ```
        SELECT p.firstname
        FROM person.person as p
        WHERE p.title IS NULL;
    ```

15. Получить все названия товаров в системе, в названии которых третий символ – либо буква “s”, либо буква “r”. Решить задачу как минимум двумя способами.
```
        SELECT p.name
        FROM production.product AS p
        WHERE p.name LIKE 's%' OR p.name LIKE '__r%';
```
С использованием регулярных выражений:
```
        SELECT p.name
        FROM production.product AS p
        WHERE p.name ~ '^..[sr]'
```
16.  Получить все названия товаров в системе, в названии которых ровно 5 символов. 
```
        SELECT p.name
        FROM production.product AS p
        WHERE LENGTH(p.name) = 5 
```
17.  Написать запрос, который возвращает названия товаров, которые были в продаже между мартом 2011 года и мартом 2012 года включительно (необходимо учитывать формат даты)
```
        SELECT p.name
        FROM production.product AS p
        WHERE p.sellstartdate > '2011-03-01'
        AND p.sellenddate < '2012-03-30'
```

18. Найти максимальную стоимость товара (отпускная цена ListPrice) из тех, которые были произведены, начиная с марта 2011 года. 
```
        SELECT p.listprice
        FROM production.product AS p
        WHERE p.modifieddate > '2011-03-01' ORDER BY p.listprice DESC
        LIMIT 1
```
С использованием `MAX`:
```
        SELECT MAX(p.listprice)
        FROM production.product AS p
        WHERE p.modifieddate > '2011-03-01';
```
