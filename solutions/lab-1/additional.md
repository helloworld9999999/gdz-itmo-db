1. Найти и вывести на экран названия, цвет и размер всех продуктов, у которых цвет начинается на букву "B" и цена больше средней цены всех продуктов.
```
    SELECT product.name FROM production.product
    WHERE product.color LIKE 'B%'
    AND product.listprice > (
        SELECT AVG(product.listprice) FROM production.product
    );
```

2. Найти и вывести на экран названия продуктов, у которых цена ниже 200, размер определён, и цвет не является "Black".
```
    SELECT product.name FROM production.product
    WHERE product.listprice < 200
    AND product.size IS NOT NULL
    AND product.color <> 'Black';
```

3. Найти и вывести на экран названия и размеры продуктов, которые были модифицированы в 2015 году и имеют цвет "Red".
```
    SELECT product.name, product.size FROM production.product
    WHERE date_trunc('year', product.modifieddate) = date_trunc('year', '01-01-2015')
    AND product.color = 'Red';
```
```
    SELECT p.name, p.size 
    FROM production.product AS p 
    WHERE p.color = 'Red' AND p.modifieddate 
    BETWEEN '2015-01-01' AND '2015-12-31'
```

4. Найти и вывести на экран названия продуктов, у которых цена ниже 50 и размер равен "L". Упорядочить по убыванию даты модификации.
```
    SELECT product.name FROM production.product
    WHERE product.listprice < 50
    AND product.size = 'L'
    ORDER BY product.modifieddate;
```

5. Найти и вывести на экран названия и цены всех продуктов, у которых цена ниже 150 и которые продаются в размерах "S" или "M".
```
    SELECT product.name, product.listprice FROM production.product
    WHERE product.listprice < 150
    AND product.size = 'L' OR product.size = 'M';
```


6. Найти и вывести на экран названия и цвет всех продуктов, у которых размер не определён и цена больше 500.
```
    SELECT product.name, product.color
    FROM production.product
    WHERE product.size IS NULL
    AND product.listprice > 500;
```
7. Найти и вывести на экран уникальные размеры продуктов, у которых цена в диапазоне от 20 до 80.
```
    SELECT DISTINCT product.size
    FROM production.product
    WHERE product.listprice BETWEEN 20 AND 80;
```
8. Найти и вывести на экран названия и цвет продуктов, у которых в названии первый символ "A", а четвёртый символ — "T".
Регулярки:
```
    SELECT product.name, product.color
    FROM production.product
    WHERE product.name ~ '^A..T.*';
```
LIKE:
```
    SELECT product.name, product.color
    FROM production.product
    WHERE product.name ~ '^A__T%';
```
9. Найти и вывести на экран названия продуктов, которые начинаются на букву "C", и длина имени которых не более 6 символов.
Регулярка:
```
    SELECT product.name
    FROM production.product
    WHERE product.name ~ '^C.{0,5}$';
```

LIKE:
```
    SELECT p.name FROM production.product AS p 
    WHERE p.name LIKE 'C%' AND LENGTH(p.name)<=6
```
10. Найти и вывести на экран названия продуктов, которые были модифицированы до 2010 года и имеют цену более 300.
```
    SELECT product.name
    FROM production.product
    WHERE product.listprice > 300
    AND product.modifieddate < '2010-01-01';
```
