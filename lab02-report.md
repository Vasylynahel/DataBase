# Лабораторна робота 2. Створення складних SQL запитів

## Загальна інформація

**Здобувач освіти:** [Ваше ПІБ]
**Група:** [Номер групи]
**Обраний рівень складності:** [1/2/3]

## Виконання завдань

### Рівень 1

#### 1. З'єднання таблиць

**Завдання 1.1:** INNER JOIN - список товарів з категоріями та постачальниками

```sql
SELECT p.product_name, c.category_name, s.company_name, p.unit_price
FROM products p
INNER JOIN categories c ON p.category_id = c.category_id
INNER JOIN suppliers s ON p.supplier_id = s.supplier_id
ORDER BY c.category_name, p.product_name;
```

**Результат виконання:**
```
https://ibb.co/Dgss97t6 

```

**Пояснення:** Цей запит показує продукт, його категорію, компанію-постачальника та ціну. Назву продукта бере з таблиці продукти, приєднує таблицю категорії для стовпця категорії товара, а також підключає таблицю suppliers. Сортує по категорії і потім по назві товара.



**Завдання 1.2:** LEFT JOIN - клієнти з кількістю замовлень

```sql
SELECT c.contact_name, c.customer_type, r.region_name,
      COUNT(o.order_id) as order_count
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
LEFT JOIN regions r ON c.region_id = r.region_id
GROUP BY c.customer_id, c.contact_name, c.customer_type, r.region_name
ORDER BY order_count DESC;
```

**Результат виконання:**
```
https://ibb.co/vtqDPsZ
```

**Пояснення:** INNER JOIN повертає лише клієнтів, у яких є замовлення. LEFT JOIN повертає всіх клієнтів, навіть тих, хто не робив жодного замовлення. У твоєму запиті LEFT JOIN показує клієнтів з кількістю замовлень, включно з нульовою кількістю. INNER JOIN виключив би клієнтів без замовлень.



**Завдання 1.3:** Множинне з'єднання - детальна інформація про замовлення

```sql
SELECT 
    o.order_id,
    o.order_date,
    c.contact_name AS customer_name,
    e.first_name, e.last_name,
    p.product_name,
    od.quantity,
    od.unit_price
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN employees e ON o.employee_id = e.employee_id
INNER JOIN order_items od ON o.order_id = od.order_id
INNER JOIN products p ON od.product_id = p.product_id
ORDER BY o.order_date DESC, o.order_id;
```

**Результат виконання:**
```
https://ibb.co/fV8rnZgr
```

**Аналіз складності:** Оцініть складність запиту та поясніть послідовність з'єднань.
Запит об'єднує 5 таблиць. Таблиці orders і order_items: одне замовлення може мати багато позицій - це збільшує кількість рядків у результаті.



#### 2. Агрегатні функції

**Завдання 2.1:** Статистика товарів за категоріями

```sql
SELECT c.category_name,
      COUNT(p.product_id) as product_count,
      AVG(p.unit_price) as avg_price,
      MIN(p.unit_price) as min_price,
      MAX(p.unit_price) as max_price
FROM categories c
LEFT JOIN products p ON c.category_id = p.category_id
GROUP BY c.category_id, c.category_name
ORDER BY product_count DESC;
```

**Результат виконання:**
```
https://ibb.co/Xr12c0B0
```

**Завдання 2.2:** Продажі за регіонами з використанням HAVING

```sql
-- SUM, GROUP BY, HAVING
SELECT 
    r.region_name,
    SUM(od.unit_price * od.quantity) AS total_sales
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN regions r ON c.region_id = r.region_id
INNER JOIN order_items od ON o.order_id = od.order_id
GROUP BY r.region_name
HAVING SUM(od.unit_price * od.quantity) > 0
ORDER BY total_sales DESC;
```

**Результат виконання:**
```
https://ibb.co/G4SNq6Jb
```

**Завдання 2.3:** Постачальники з кількістю товарів більше 2

```sql
-- Запит з HAVING
SELECT 
    s.company_name,
    COUNT(p.product_id) AS product_count
FROM suppliers s
INNER JOIN products p ON s.supplier_id = p.supplier_id
GROUP BY s.supplier_id, s.company_name
HAVING COUNT(p.product_id) > 2
ORDER BY product_count DESC;
```

**Результат виконання:**
```
https://ibb.co/GfFk9Lqp
```



#### 3. Базові підзапити

**Завдання 3.1:** Товари з ціною вище середньої по категорії

```sql
SELECT p.product_name, p.unit_price, c.category_name
FROM products p
INNER JOIN categories c ON p.category_id = c.category_id
WHERE p.unit_price > (
    SELECT AVG(p2.unit_price)
    FROM products p2
    WHERE p2.category_id = p.category_id
)
ORDER BY c.category_name, p.unit_price DESC;
```

**Результат виконання:**
```
https://ibb.co/Gv8qGCQ7
```

**Завдання 3.2:** Клієнти з замовленнями у 2024 році

```sql
-- Підзапит з IN
SELECT c.contact_name, c.company_name
FROM customers c
WHERE c.customer_id IN (
    SELECT o.customer_id
    FROM orders o
    WHERE EXTRACT(YEAR FROM o.order_date) = 2024
)
ORDER BY c.contact_name;
```

**Результат виконання:**
```
https://ibb.co/Z6PFkv4f
```

**Завдання 3.3:** Товари з загальною кількістю продажів

```sql
-- Підзапит у SELECT
SELECT 
    p.product_name,
    p.unit_price,
    (
        SELECT SUM(od.quantity)
        FROM order_items od
        WHERE od.product_id = p.product_id
    ) AS total_sold
FROM products p
ORDER BY total_sold DESC NULLS LAST;
```

**Результат виконання:**
```
https://ibb.co/dJ4yNgh0
```



### Рівень 2

#### 4. Складні з'єднання

**Завдання 4.1:** RIGHT JOIN - аналіз категорій та товарів

```sql
-- RIGHT JOIN код
SELECT c.category_name,
      COUNT(p.product_id) as products_count,
      COALESCE(AVG(p.unit_price), 0) as avg_price
FROM products p
RIGHT JOIN categories c ON p.category_id = c.category_id
GROUP BY c.category_id, c.category_name
ORDER BY products_count DESC;
```

**Результат виконання:**
```
https://ibb.co/SSfJb4R
```

**Завдання 4.2:** Self-join - співробітники та керівники

```sql
-- Self-join код
SELECT e1.first_name || ' ' || e1.last_name as employee,
      e1.title as employee_title,
      e2.first_name || ' ' || e2.last_name as manager,
      e2.title as manager_title
FROM employees e1
LEFT JOIN employees e2 ON e1.reports_to = e2.employee_id
ORDER BY e2.last_name, e1.last_name;
```

**Результат виконання:**
```
https://ibb.co/sdCJ932n
```



#### 5. Віконні функції

**Завдання 5.1:** Ранжування товарів за ціною в категоріях

```sql
-- RANK(), DENSE_RANK(), ROW_NUMBER() код
SELECT p.product_name,
      c.category_name,
      p.unit_price,
      RANK() OVER (PARTITION BY c.category_name ORDER BY p.unit_price DESC) as price_rank,
      DENSE_RANK() OVER (PARTITION BY c.category_name ORDER BY p.unit_price DESC) as price_dense_rank,
      ROW_NUMBER() OVER (PARTITION BY c.category_name ORDER BY p.unit_price DESC) as row_num
FROM products p
JOIN categories c ON p.category_id = c.category_id
ORDER BY c.category_name, p.unit_price DESC;
```

**Результат виконання:**
```
https://ibb.co/gLxZjNbR
```

**Завдання 5.2:** Порівняння замовлень з попередніми датами

```sql
-- LAG(), LEAD() код
SELECT 
    o.order_id,
    o.customer_id,
    c.contact_name,
    o.order_date,
    LAG(o.order_date) OVER (
        PARTITION BY o.customer_id 
        ORDER BY o.order_date
    ) AS previous_order_date,
    o.order_date - LAG(o.order_date) OVER (
        PARTITION BY o.customer_id 
        ORDER BY o.order_date
    ) AS days_between
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
ORDER BY c.contact_name, o.order_date;
```

**Результат виконання:**
```
https://ibb.co/q30hhcTZ
```



### Рівень 3

#### 6. Матеріалізовані представлення та рекурсивні запити

**Завдання 6.1:** Матеріалізоване представлення для аналізу продажів

```sql
-- CREATE MATERIALIZED VIEW код
CREATE MATERIALIZED VIEW mv_monthly_sales AS
SELECT
    EXTRACT(YEAR FROM o.order_date) as year,
    EXTRACT(MONTH FROM o.order_date) as month,
    c.category_name,
    r.region_name,
    SUM(oi.quantity * oi.unit_price * (1 - oi.discount)) as total_revenue,
    COUNT(DISTINCT o.order_id) as orders_count,
    AVG(oi.quantity * oi.unit_price * (1 - oi.discount)) as avg_order_value
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
JOIN customers cu ON o.customer_id = cu.customer_id
LEFT JOIN regions r ON cu.region_id = r.region_id
WHERE o.order_status = 'delivered'
GROUP BY year, month, c.category_name, r.region_name;

-- Створення індексу для матеріалізованого представлення
CREATE INDEX idx_mv_monthly_sales_date ON mv_monthly_sales(year, month);
```

```
https://ibb.co/gZ5BrZYV
```
**Пояснення:** Поясніть переваги використання матеріалізованих представлень.

**Завдання 6.2:** Рекурсивний запит для ієрархії співробітників

```sql
-- WITH RECURSIVE код
WITH RECURSIVE employee_hierarchy AS (
    -- Базовий випадок: топ-менеджери (без керівника)
    SELECT employee_id, first_name, last_name, title, reports_to,
          0 as level,
          CAST(last_name || ' ' || first_name as VARCHAR(1000)) as hierarchy_path
    FROM employees
    WHERE reports_to IS NULL

    UNION ALL

    -- Рекурсивний випадок: підлеглі
    SELECT e.employee_id, e.first_name, e.last_name, e.title, e.reports_to,
          eh.level + 1,
          CAST(eh.hierarchy_path || ' -> ' || e.last_name || ' ' || e.first_name as VARCHAR(1000))
    FROM employees e
    JOIN employee_hierarchy eh ON e.reports_to = eh.employee_id
)
SELECT * FROM employee_hierarchy
ORDER BY hierarchy_path;
```

**Результат виконання:**
```
https://ibb.co/mrdRcCkF
```



## Аналіз продуктивності

### Дослідження планів виконання

**Найповільніший запит:**
```sql
-- Код запиту
SELECT 
    r.region_name,
    SUM(od.unit_price * od.quantity) AS total_sales
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN regions r ON c.region_id = r.region_id
INNER JOIN order_items od ON o.order_id = od.order_id
GROUP BY r.region_name
HAVING SUM(od.unit_price * od.quantity) > 0
ORDER BY total_sales DESC;
```

**План виконання (EXPLAIN ANALYZE):**
```
-- Результат 
EXPLAIN ANALYZE
Sort  (cost=21.12..21.16 rows=16 width=150) (actual time=4.503..4.505 rows=5 loops=1)
    Sort Key: (sum((od.unit_price * (od.quantity)::numeric))) DESC
    Sort Method: quicksort  Memory: 25kB
    ->  HashAggregate  (cost=20.09..20.80 rows=16 width=150) (actual time=4.480..4.484 rows=5 loops=1)
            Group Key: r.region_name
            Filter: (sum((od.unit_price * (od.quantity)::numeric)) > '0'::numeric)
            Batches: 1  Memory Usage: 24kB
            ->  Hash Join  (cost=17.61..19.62 rows=47 width=138) (actual time=4.417..4.447 rows=47 loops=1)
                Hash Cond: (o.customer_id = c.customer_id)
                  ->  Hash Join  (cost=1.70..3.30 rows=47 width=24) (actual time=2.441..2.459 rows=47 loops=1)
                    Hash Cond: (od.order_id = o.order_id)
                    ->  Seq Scan on order_items od  (cost=0.00..1.47 rows=47 width=24) (actual time=1.244..1.248 rows=47 loops=1)
                    ->  Hash  (cost=1.31..1.31 rows=31 width=8) (actual time=1.165..1.166 rows=31 loops=1)
                          Buckets: 1024  Batches: 1  Memory Usage: 10kB
                          ->  Seq Scan on orders o  (cost=0.00..1.31 rows=31 width=8) (actual time=1.149..1.153 rows=31 loops=1)
              ->  Hash  (cost=15.73..15.73 rows=15 width=122) (actual time=1.964..1.964 rows=15 loops=1)
                    Buckets: 1024  Batches: 1  Memory Usage: 9kB
                    ->  Nested Loop  (cost=0.15..15.73 rows=15 width=122) (actual time=1.932..1.955 rows=15 loops=1)
                          ->  Seq Scan on customers c  (cost=0.00..1.15 rows=15 width=8) (actual time=0.608..0.610 rows=15 loops=1)
                          ->  Index Scan using regions_pkey on regions r  (cost=0.15..0.97 rows=1 width=122) (actual time=0.089..0.089 rows=1 loops=15)
                             Index Cond: (region_id = c.region_id)
Planning Time: 16.059 ms
Execution Time: 4.687 ms
```

**Запропоновані оптимізації:**
1. Створити індекс на колонці customer_id у таблиці customers для прискорення з’єднання з таблицею orders.
2. Створити індекс на колонці order_id у таблиці order_items, щоб оптимізувати зв’язок з orders.
3. Створення індексу на колонці region_id у таблиці customers для прискорення з’єднання з таблицею regions.

### Створені індекси

**Індекс 1:**
```sql
-- CREATE INDEX код
CREATE INDEX idx_customers_customer_id ON customers(customer_id);
```
**Обґрунтування:** Індекс дозволяє швидко знаходити записи в customers за customer_id, що використовується у JOIN з orders, зменшуючи час сканування

**Індекс 2:**
```sql
-- CREATE INDEX код
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
```
**Обґрунтування:** Індекс прискорює пошук рядків у order_items за order_id для приєднання до таблиці orders.



## Порівняльний аналіз

### Ефективність різних підходів

**Завдання:** Знайти топ-5 найдорожчих товарів у кожній категорії

**Підхід 1: Віконні функції**
```sql
-- Код з віконними функціями
SELECT *
FROM (
    SELECT
        p.product_id,
        p.product_name,
        c.category_name,
        od.unit_price,
        ROW_NUMBER() OVER (PARTITION BY c.category_name ORDER BY od.unit_price DESC) AS rn
    FROM products p
    INNER JOIN categories c ON p.category_id = c.category_id
    INNER JOIN order_items od ON p.product_id = od.product_id
) sub
WHERE rn <= 5;
```

**Підхід 2: Корельований підзапит**
```sql
-- Код з підзапитом
SELECT
    p.product_id,
    p.product_name,
    c.category_name,
    od.unit_price
FROM products p
INNER JOIN categories c ON p.category_id = c.category_id
INNER JOIN order_items od ON p.product_id = od.product_id
WHERE od.unit_price >= (
    SELECT MIN(unit_price)
    FROM (
        SELECT od2.unit_price
        FROM order_items od2
        INNER JOIN products p2 ON od2.product_id = p2.product_id
        WHERE p2.category_id = p.category_id
        ORDER BY od2.unit_price DESC
        LIMIT 5
    ) top5
);
```

**Час виконання:**
- Віконні функції: 0.944 ms
- Корельований підзапит: 4.568 ms

**Висновок:** Використання віконних функцій є більш ефективним і швидким для задачі вибірки топ-N записів у кожній категорії, оскільки дозволяє уникнути повторних підзапитів і оптимізується планувальником СУБД.



## Висновки

**Самооцінка**: [ваша оцінка роботи, 3-5]

**Обгрунтування**: [обґрунтування самооцінки]
