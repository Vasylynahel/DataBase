# Лабораторна робота 1. Робота з СУБД PostgreSQL та основи SQL

## Загальна інформація

**Здобувач освіти:** Ільчук Василина
**Група:** ІПЗ-32
**Обраний рівень складності:** 3
**Посилання на проєкт:** https://supabase.com/dashboard/project/zxvbuwbxfpoqxozqvnft/sql/096677d7-2f3e-4f2a-8d3a-dea477c6d4d7

## Виконання завдань

### 1. Отримати всі записи з таблиці customers.

```sql
-- Запит для отримання списку таблиць
SELECT *
FROM customers
```

Отримано 15 записів клієнтів

Скріншот https://ibb.co/kTrxh2P 



### 2. Вивести тільки назви товарів і їхні ціни з таблиці products.

```sql
SELECT product_name, unit_price
FROM products
```

Результат: 	Отримано лише дві колонки з таблиці: product_name та unit_price 

Скріншот https://ibb.co/JwM1tPMN



### 3. Показати контактні дані всіх співробітників (ім'я, прізвище, телефон, email).

```sql
SELECT last_name, first_name, phone, email
FROM employees
```

Результат: 	Отримано дані  працівників

Скріншот https://ibb.co/S7MB6g8H



### 4. Знайти всіх клієнтів з міста Київ.

```sql
SELECT *
FROM customers
Where city = 'Київ'
```

Результат: 	Отримано клієнтів з міста Київ. Всього 4 записи.

Скріншот https://ibb.co/dsJfsnh0



### 5. Вивести товари, які коштують більше 25000 грн.

```sql
SELECT *
FROM products
WHERE unit_price > 25000
```

Результат: 	Отримано 13 товарів з ціною більшою, ніж 250000 грн

Скріншот https://ibb.co/CpmGHv6x



### 6. Показати всі замовлення зі статусом 'delivered'.

```sql
SELECT *
FROM orders
WHERE order_status = 'delivered'
```

Результат: 	Отримано всі замовлення зі статусом “delivered”

Скріншот https://ibb.co/rR4TYmBx 



### 7. Знайти співробітників, які працюють у відділі продажів (посада містить слово "продаж").

```sql
SELECT *
FROM employees
WHERE title LIKE '%прод%'
```

Результат: 	Отримано працівників з відділу продажів

Скріншот https://ibb.co/Rp7RFKSK

### 8. Відсортувати товари за зростанням ціни.

```sql
SELECT *
FROM products
ORDER BY unit_price
```

Результат: 	Перший запис має мінімальну вартість товару, а останній - найдорожчий

Скріншот https://ibb.co/Qh7G0GB



### 9.Показати клієнтів в алфавітному порядку за іменем контактної особи.

```sql
SELECT *
FROM customers
ORDER BY contact_name
```

Результат: 	Виведені всі компанії або особи-замовники в алфавітному порядку

Скріншот https://ibb.co/TMVgxVdV


### 10. Вивести замовлення від найновіших до найстаріших.

```sql
SELECT *
FROM orders
ORDER BY order_date DESC
```

Результат:  Вивести замовлення від найновіших до найстаріших.

Скріншот https://ibb.co/Y4s2FCy6



### 11. Показати перші 10 найдорожчих товарів.

```sql
SELECT *
FROM products
ORDER BY unit_price DESC
LIMIT 10
```

Результат: 	Отримано найдорожчі товари до 10 запису

Скріншот https://ibb.co/CKQQDcM8
№№№№№


### 12. Вивести 5 останніх замовлень (за датою).

```sql
SELECT *
FROM orders
ORDER BY order_date DESC
LIMIT 5
```

Результат:  Отримано останні 5 замовлень. Дата останнього 08-20, а п’ятого з кінця 08-05


Скріншот https://ibb.co/ZpgsSL8j



### 13. Отримати перших 8 клієнтів в алфавітному порядку.

```sql
SELECT *
FROM customers
ORDER BY contact_name
LIMIT 8
```

Результат: Отримано лише 8 клієнтів, які поставленні в алфавітному порядку

Скріншот https://ibb.co/rfkQyWhc


## Рівень 2
### 14. Знайти всіх клієнтів, чиї імена починаються на "Іван".

```sql
SELECT *
FROM customers
WHERE contact_name LIKE '% Іван %'
```

Результат: 	Отримано одного замовника з іменем Іван. Пробіли після і до знаків % не дозволяють отримати записи з “Іванович”, “Іванівна”, “Іванова”.

Скріншот https://ibb.co/LXhmr6Ng



### 15. Вивести товари, в назві яких є слово "phone" або "телефон".

```sql
SELECT *
FROM products
WHERE product_name LIKE '%Phone%' OR product_name LIKE '%телефон%'
```

Результат: 	Отримано один запис iPhone 15 128GB Чорний. Якби в умові “Phone” був з малої літери, тоді б не вивело жодного запису

Скріншот https://ibb.co/Fb34LzCf



### 16. Вивести замовників, контактні особи яких є директорами (“директор”).

```sql
SELECT *
FROM customers
WHERE contact_title LIKE '%директор%'
```

Результат: 	Отримано 3-х директорів

Скріншот https://ibb.co/99XJ2S8X



### 17. Вивести категорії, в яких є сполучник “та”

```sql
SELECT *
FROM categories
WHERE category_name LIKE '%та%'
```

Результат: 	Виведені категорії назва, яких складається з двох категорій, наприклад, смартфони та телефони.

Скріншот https://ibb.co/3mXr9pGj



### 18. Вивести товари, назви яких починаються з Samsung

```sql
SELECT *
FROM products
WHERE product_name LIKE 'Samsung%'
```

Результат: 	Отримано 3 записи, які починаються на Samsung

Скріншот https://ibb.co/nMScGxPY


### 19. Знайти товари дорожчі за 15000 грн і дешевші за 50000 грн.

```sql
SELECT *
From products
WHERE unit_price > 15000 AND unit_price < 50000
```

Результат: 	Отримані товари, ціни яких розташовані в заданому діапазоні

Скріншот https://ibb.co/6cckGJ96




### 20. Вивести клієнтів з Києва або Львова, які є юридичними особами.

```sql
SELECT *
FROM customers
WHERE customer_type = 'company'
 AND city IN ('Київ', 'Львів');
```

Результат: 	Отримані юр особи з Києва, зі Львова немає жодної

Скріншот https://ibb.co/ycFpXrCx



### 21. Вивести замовників з Дніпра, які зробили більше 3-х замовлень

```sql
SELECT c.contact_name, c.city, c.customer_type, o.total_orders
FROM customers c
JOIN customer_orders_summary o ON c.customer_id = o.customer_id
WHERE c.city = 'Дніпро' AND o.total_orders > 3;
```

Результат: 	Виведені ті замовники, які зробили більше 3-х замовлень і з Дніпра

Скріншот https://ibb.co/FbgHTS7d



### 22. Вивести клієнтів-фізичних осіб, зареєстрованих після 1 березня 2023 року, які не проживають у Києві

```sql
SELECT contact_name, city, customer_type, registration_date
FROM customers
WHERE customer_type != 'company'
 AND registration_date > '2023-03-01'
 AND city != 'Київ';
```

Результат: 	Виведено 7 записів

Скріншот https://ibb.co/SXcHH60H



### 23. Вивести працівників з продажу, які працюють у Києві або Львові або Одесі

```sql
SELECT last_name, first_name, title, city
FROM employees
WHERE title = 'Менеджер з продажу'
 AND (city = 'Одеса' OR city = 'Львів' OR city = 'Київ')
```

Результат:  Виведено менеджерів з продажу і зі Львова або Одеси, або Києва

Скріншот https://ibb.co/nN4fLQJB


### 24. Отримати категорії товарів, які НЕ містять слова "аксесуари" або "ігри" в описі.

```sql
SELECT category_name, description
FROM categories
WHERE NOT (description LIKE '%аксесуари%' OR description LIKE '%ігри%');
```

Результат: 	Виведені категорії без слів “аксесуари” або “ігри”

Скріншот https://ibb.co/0pg2zw5Y



### 25. Вивести клієнтів з міст Київ, Харків, Одеса, Дніпро.

```sql
SELECT contact_name, city, customer_type
FROM customers
WHERE city IN ('Київ', 'Харків', 'Одеса', 'Дніпро');
```

Результат: 	Виведені всі клієнти з наведених міст

Скріншот https://ibb.co/MxmR48jf



### 26. Знайти товари в ціновому діапазоні від 10000 до 30000 грн.

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price BETWEEN 10000 AND 30000;
```

Результат: 	Виведені товари дорожчі, ніж 10000, та нижчі, ніж 30000 грн.

Скріншот https://ibb.co/Q3YKmJnB



### 27. Вивести клієнтів з областей:  Львівська, Дніпропетровська - та міста Київ

```sql
SELECT contact_name, region_name, city
FROM customer_orders_summary
WHERE region_name IN ('м. Київ', 'Львівська область', 'Дніпропетровська область');
```

Результат: 	Повернуто 10 клієнтів з міста Київ та 2-х областей

Скріншот https://ibb.co/jZBWvgGb



### 28. Вивести працівників, які мають одну з посад: "Менеджер з продажу", "Спеціаліст з логістики"

```sql
SELECT last_name, first_name, title
FROM employees
WHERE title IN ('Менеджер з продажу', 'Спеціаліст з логістики');
```

Скріншот  https://ibb.co/wZfPTPFj




### 29. Знайти клієнтів із загальною сумою замовлень від 50 000 до 150 000 грн

```sql
SELECT contact_name, total_amount
FROM customer_orders_summary
WHERE total_amount BETWEEN 50000 AND 150000;
```

Скріншот https://ibb.co/hpF0pgD



### 30. Знайти працівників, які обробили від 5 до 15 замовлень

```sql
SELECT full_name, orders_handled
FROM employee_performance
WHERE orders_handled BETWEEN 5 AND 15;
```

Скріншот https://ibb.co/gLhnbCcW




### 31. Знайти клієнтів, у яких не вказано компанію (фізичні особи без company_name)

```sql
SELECT contact_name, company_name
FROM customers
WHERE company_name IS NULL;
```

Скріншот https://ibb.co/ksfSrsfq




### 32. Знайти клієнтів, у яких вказана посада контактної особи.

```sql
SELECT contact_name, contact_title
FROM customers
WHERE contact_title IS NOT NULL;
```

Скріншот https://ibb.co/gZ4jc4kL



### 33. Знайти клієнтів, імена яких починаються на "К" або містять "енко", з міст Київ або Харків.

```sql
SELECT contact_name, city
FROM customers
WHERE (contact_name LIKE 'К%' OR contact_name LIKE '%енко%')
 AND city IN ('Київ', 'Харків');
```

Скріншот https://ibb.co/jvxmMk2L




### 34. Знайти клієнтів, які є фізичними особами, зареєстровані між 1 березня і 1 червня 2023 року

```sql
SELECT contact_name, registration_date
FROM customers
WHERE customer_type = 'individual'
 AND registration_date BETWEEN '2023-03-01' AND '2023-06-01'
```

Скріншот https://ibb.co/WWR4qfHV




### 35. Знайти працівників, які мають посади, що містять слово "менеджер", працюють не в Києві

```sql
SELECT full_name, title, city
FROM employee_performance
WHERE title ILIKE '%менеджер%'
 AND city != 'Київ'
```

Скріншот https://ibb.co/MyVbQKnj




### 36. Знайти клієнтів-компанії з Києва або Харкова, у яких total_amount від 200000 до 400000 грн.

```sql
SELECT contact_name, city, total_amount
FROM customer_orders_summary
WHERE customer_type = 'company'
 AND city IN ('Київ', 'Харків')
 AND total_amount BETWEEN 200000 AND 400000;
```

Скріншот https://ibb.co/fzmpCR8X




### 37. Знайти клієнтів, у яких або немає email (IS NULL), або в email використовується домен @meta.ua.
```sql
SELECT contact_name, email
FROM customers
WHERE email IS NULL
  OR email LIKE '%@meta.ua';
```

Скріншот https://ibb.co/k2KxrYTK



### 38. Вивести клієнтів, відсортованих спочатку за містом (по алфавіту), потім за датою реєстрації (найновіші зверху)

```sql
SELECT contact_name, city, registration_date
FROM customers
ORDER BY city, registration_date DESC;
```

Скріншот https://ibb.co/1t8JLqGc




### 39. Вивести працівників, відсортованих за посадою (по алфавіту), а серед однакових посад — за кількістю оброблених замовлень (від більшого до меншого)

```sql
SELECT full_name, title, orders_handled
FROM employee_performance
ORDER BY title, orders_handled DESC;
```

Скріншот https://ibb.co/N2H6NsXc




### 40. Вивести клієнтів-компанії, відсортованих за сумою замовлень (від найбільшої до найменшої), потім за останньою датою замовлення (свіжі зверху)

```sql
SELECT contact_name, customer_type, total_amount, last_order_date
FROM customer_orders_summary
WHERE customer_type = 'company'
ORDER BY contact_name, last_order_date DESC;
```

Скріншот https://ibb.co/0yTK4jKh



### 41. Показати перші 5 клієнтів за алфавітом

```sql
SELECT contact_name, city
FROM customers
ORDER BY contact_name ASC
LIMIT 5 OFFSET 0;
```

Скріншот https://ibb.co/wrKz3MYL



### 42. Показати наступні 5 клієнтів (сторінка 2)

```sql
SELECT contact_name, city
FROM customers
ORDER BY contact_name ASC
LIMIT 5 OFFSET 5;
```

Скріншот https://ibb.co/5Wpbxb91


## Рівень 3
### 43. Знайти товари, в назві яких є "Samsung" або "Apple", але немає слова "чохол".

```sql
SELECT product_name
FROM products
WHERE (product_name LIKE '%Samsung%' OR product_name LIKE '%Apple%')
 AND product_name NOT LIKE '%чохол%';
```

Скріншот https://ibb.co/RGf2RTdR



### 44. Знайти товари, в назві яких є "ноутбук" або "комп’ютер", але немає "аксесуар"
```sql
SELECT *
FROM products
WHERE (description ILIKE '%ноутбук%' OR description ILIKE '%комп’ютер%')
 AND description NOT ILIKE '%аксесуар%';
```

Скріншот https://ibb.co/277qBmGx



### 45. Знайти товари, які містять у назві "iPhone" або "Galaxy", але не містять "Новий" в описі.

```sql
SELECT *
FROM products
WHERE (product_name ILIKE '%iPhone%' OR product_name ILIKE '%Galaxy%')
 AND description NOT ILIKE '%Новий%'
```

Скріншот https://ibb.co/nM1w0nzk



### 46. Приклад запиту: знайти товари з категорії "Планшети та електронні книги", назва не містить "mini"

```sql
SELECT p.product_name, c.category_name, p.unit_price
FROM products p
JOIN categories c ON p.category_id = c.category_id
WHERE c.category_name = 'Планшети та електронні книги'
 AND p.product_name NOT ILIKE '%mini%';
```

Скріншот https://ibb.co/Xf29PDh2



### 47. Знайти товари з Івано-Франківська, опис яких містить слова "кабель" або "зарядний"

```sql
SELECT p.product_name, p.unit_price, c.category_name, s.city
FROM products p
JOIN categories c ON p.category_id = c.category_id
JOIN suppliers s ON p.supplier_id = s.supplier_id
WHERE (p.description ILIKE '%кабель%' OR p.description ILIKE '%зарядний%')
 AND s.city = 'Івано-Франківськ';
```

Скріншот https://ibb.co/XZjmsGRx



### 48. Знайти товари дорожчі 20000 грн (категорії 1 або 2) АБО товари дешевші 5000 грн будь-якої категорії.

```sql
SELECT product_name, unit_price, category_id
FROM products
WHERE
 (unit_price > 20000 AND category_id IN (1, 2))
 OR
 (unit_price < 5000);
```

Скріншот https://ibb.co/qFLkPyHk



### 49. Знайти товари з категорій 1, 2 або 3, які або дорожчі за 25000 грн, або мають у назві слово "Pro", але не містять "Samsung"

```sql
SELECT product_name, unit_price, category_id
FROM products
WHERE category_id IN (1, 2, 3)
 AND (
   unit_price > 25000
   OR (product_name ILIKE '%Pro%' AND product_name NOT ILIKE '%Samsung%')
 );
```

Скріншот https://ibb.co/vxYZnGTS



### 50. Вивести всі товари, які або належать до категорій 4 або 5 і коштують до 10000 грн, або мають у назві "LGi" і вартість у діапазоні від 5000 до 15000 грн

```sql
SELECT product_name, unit_price, category_id
FROM products
WHERE (
   (category_id IN (4, 5) AND unit_price <= 10000)
   OR
   (product_name ILIKE '%LG%' AND unit_price BETWEEN 5000 AND 15000)
);
```

Скріншот https://ibb.co/xNhmzct



### 51. Знайти всі товари, де назва містить "Apple" або "Samsung", але не містить "чохол", і водночас ціна або більша за 20000, або менша за 15000
```sql
SELECT product_name, unit_price
FROM products
WHERE
 (
   product_name ILIKE '%Apple%'
   OR product_name ILIKE '%Samsung%'
 )
 AND product_name NOT ILIKE '%чохол%'
 AND (
   unit_price > 20000
   OR unit_price < 15000
 );
```

Скріншот https://ibb.co/tTHywZM5



### 52. Звіт товарів із категорій, що не містять "техніка" та "комп" у назві, з фільтрацією за ціною, містом постачальника та назвами товарів, що містять "Pro" або "Ultra"

```sql
SELECT
   p.product_name,
   p.unit_price,
   c.category_name,
   s.city
FROM products p
JOIN categories c ON p.category_id = c.category_id
JOIN suppliers s ON p.supplier_id = s.supplier_id
WHERE
   (p.product_name ILIKE '%Pro%' OR p.product_name ILIKE '%Ultra%')         -- умова 1
   AND p.unit_price BETWEEN 10000 AND 50000                                      -- умова 2
   AND c.category_name NOT IN ('Аксесуари', 'Чохли')                        -- умова 3
   AND s.city IN ('Київ', 'Харків')                                         -- умова 4
   AND (c.category_name NOT ILIKE '%техніка%' OR c.category_name NOT ILIKE '%комп%') -- умова 5
   AND p.description NOT ILIKE '%Новий%'                            -- умова 6
```

Скріншот https://ibb.co/yB4zzshd



### 53. вибір клієнтів, які: 
### - є company
### - мають більше ніж 2 замовлення,


### - зробили останнє замовлення за останні 3 роки,


### - проживають у містах (Київ, Харків, Дніпро),


### - та загальна сума замовлень більша за 50 000 грн.


```sql
SELECT
   contact_name,
   city,
   customer_type,
   total_orders,
   total_amount,
   last_order_date
FROM customer_orders_summary
WHERE
   customer_type = 'company'
   AND total_orders > 2
   AND last_order_date >= CURRENT_DATE - INTERVAL '3 years'
   AND city IN ('Київ', 'Харків', 'Дніпро')
   AND total_amount > 50000
ORDER BY total_amount DESC, last_order_date DESC;
```

Скріншот https://ibb.co/7LHrF6d



### 54. Товари до 10 000 грн

```sql
SELECT
   product_name,
   unit_price,
   category_name
FROM products p
JOIN categories c ON p.category_id = c.category_id
WHERE unit_price <= 10000
ORDER BY unit_price ASC;
```

Скріншот https://ibb.co/rRYbYzzN



### 55. Товари від 10 001 до 30 000 грн

```sql
SELECT
   product_name,
   unit_price,
   category_name
FROM products p
JOIN categories c ON p.category_id = c.category_id
WHERE unit_price BETWEEN 10001 AND 30000
ORDER BY unit_price ASC;
```

Скріншот https://ibb.co/PZF1TVz1

### 56. Товари преміум сегмента (понад 30 000 грн)

```sql
SELECT
   product_name,
   unit_price,
   category_name
FROM products p
JOIN categories c ON p.category_id = c.category_id
WHERE unit_price > 30000
ORDER BY unit_price DESC;
```

Скріншот https://ibb.co/PZF1TVz1



### 57. Кількість клієнтів за містами

```sql
SELECT city, COUNT(*) AS client_count
FROM customers
GROUP BY city
ORDER BY client_count DESC;
```

Скріншот https://ibb.co/Y7JxW7dM



### 58. Кількість клієнтів за регіонами

```sql
SELECT region_name, COUNT(*) AS client_count
FROM customer_orders_summary
GROUP BY region_name
ORDER BY client_count DESC;
```

Скріншот https://ibb.co/9khHJ9Tf



### 59. Кількість клієнтів у містах  Київ, Харків, Львів, Одеса

```sql
SELECT city, COUNT(*) AS client_count
FROM customers
WHERE city IN ('Київ', 'Харків', 'Львів', 'Одеса')
GROUP BY city
ORDER BY client_count DESC;
```

Скріншот https://ibb.co/cczkpsMX 



### 60.
### Вивести:
### кількість клієнтів,


### середню суму замовлень,


### максимальну суму замовлення,


### середню кількість замовлень по регіонах.


```sql
SELECT
   region_name,
   COUNT(*) AS total_clients,
   ROUND(AVG(total_amount), 2) AS avg_order_amount,
   MAX(total_amount) AS max_order_amount,
   ROUND(AVG(total_orders), 2) AS avg_total_orders
FROM customer_orders_summary
GROUP BY region_name
ORDER BY total_clients DESC;
```

Скріншот https://ibb.co/ZvkmQTF



### 61. Кількість замовлень по місяцях (від найбільшого до найменшого)

```sql
SELECT
   TO_CHAR(last_order_date, 'Month') AS month,
   COUNT(*) AS orders_count
FROM customer_orders_summary
GROUP BY month
ORDER BY orders_count DESC;
```

Скріншот https://ibb.co/KxJfzD5B



### 62. Порівняти клієнтів за тим, коли вони зробили останнє замовлення

```sql
SELECT
   contact_name,
   customer_type,
   last_order_date
FROM customer_orders_summary
ORDER BY last_order_date;
```

Скріншот https://ibb.co/wfCnwxX



### 63. клієнти, що замовляли лише 2 раза за весь час

```sql
SELECT contact_name, total_orders, last_order_date
FROM customer_orders_summary
WHERE total_orders = 2
ORDER BY last_order_date DESC;
```

Скріншот https://ibb.co/HD6KZJJ4



### 64. Рейтинг клієнтів за сумою замовлень

```sql
SELECT
   contact_name,
   city,
   total_amount,
   ROW_NUMBER() OVER (ORDER BY total_amount DESC) AS rank_by_amount
FROM customer_orders_summary;
```

Скріншот https://ibb.co/r2VkvH57



## Висновки

**Самооцінка**: 5

**Обгрунтування**: запити, які призначені для високого рівня, виконані. 
