# Звіт з лабораторної роботи №4

## Загальна інформація
- ПІБ студента: Ільчук Василина Іванівна
- Група: ІПЗ-32
- Варіант (предметна область):База даних кінотеатру
- Рівень виконання:3

## Опис предметної області
Предметна область — кінотеатр, що надає послуги з показу фільмів у різних кінозалах.
Система повинна: зберігати інформацію про фільми; планувати сеанси в конкретних залах; зберігати дані про клієнтів; продавати квитки з контролем віку; вести логування операцій; формувати статистичні звіти; підтримувати вебінтерфейс та API.

## Концептуальна модель
![ER-діаграма](../cinema/cinema_er_diagram.png)

## Логічна схема
### Таблиці:

film (film_id PK)

hall (hall_id PK)

customer (customer_id PK)

session (session_id PK, film_id FK, hall_id FK)

ticket (ticket_id PK, session_id FK, customer_id FK)

### Обмеження:

UNIQUE(session_id, seat_number) — одне місце не можна продати двічі

CHECK для віку

FOREIGN KEY з каскадними діями

## Реалізація в PostgreSQL
```sql
CREATE TABLE film (
    film_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    release_year INT,
    director VARCHAR(100),
    description TEXT,
    age_limit INT
);

CREATE TABLE hall (
    hall_id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    seats_count INT CHECK (seats_count > 0)
);

CREATE TABLE customer (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT CHECK(age >= 0)
);

CREATE TABLE session (
    session_id SERIAL PRIMARY KEY,
    film_id INT REFERENCES film(film_id),
    hall_id INT REFERENCES hall(hall_id),
    start_time TIMESTAMP NOT NULL,
    price NUMERIC(6,2)
);

CREATE TABLE ticket (
    ticket_id SERIAL PRIMARY KEY,
    session_id INT REFERENCES session(session_id),
    customer_id INT REFERENCES customer(customer_id),
    seat_number VARCHAR(5),
    is_online BOOLEAN,
    purchase_time TIMESTAMP DEFAULT NOW(),
    UNIQUE(session_id, seat_number)
);
```

## SQL-запити
Розклад сеансів
```sql
SELECT s.session_id, f.name, h.name, s.start_time, s.price
FROM session s
JOIN film f ON s.film_id = f.film_id
JOIN hall h ON s.hall_id = h.hall_id;
```

Продані квитки по фільмах
```sql
SELECT f.name, COUNT(t.ticket_id)
FROM film f
JOIN session s ON f.film_id = s.film_id
JOIN ticket t ON s.session_id = t.session_id
GROUP BY f.name;
```

Звіт по заповнюваності залів
```sql
SELECT s.session_id, COUNT(t.ticket_id), h.seats_count
FROM session s
JOIN hall h ON s.hall_id = h.hall_id
LEFT JOIN ticket t ON s.session_id = t.session_id
GROUP BY s.session_id, h.seats_count;
```

розклад сеансів із назвами фільмів і залами
```sql
SELECT 
  session.session_id,
  film.name AS фільм,
  hall.name AS зал,
  session.start_time AS час_початку,
  session.price AS ціна
FROM session
JOIN film ON session.film_id = film.film_id
JOIN hall ON session.hall_id = hall.hall_id
ORDER BY session.start_time;
```
квитки певного клієнта
```sql
SELECT 
  customer.first_name || ' ' || customer.last_name AS клієнт,
  film.name AS фільм,
  session.start_time AS час_сеансу,
  ticket.seat_number AS місце,
  CASE WHEN ticket.is_online THEN 'Онлайн' ELSE 'У касі' END AS спосіб_покупки
FROM ticket
JOIN session ON ticket.session_id = session.session_id
JOIN film ON session.film_id = film.film_id
JOIN customer ON ticket.customer_id = customer.customer_id
WHERE customer.last_name = 'Петренко';
```


## Консольний застосунок
#### Реалізовано на Python + psycopg2

#### Містить:

    перегляд фільмів
    
    додавання клієнтів
    
    покупка квитків із перевіркою віку
    
    видалення квитка

## Вебзастосунок (якщо реалізовано)
#### Реалізовано на Flask

#### Шаблони на HTML + Bootstrap

#### Доступні сторінки:

    список сеансів

    список квитків

    покупка квитка

    фільтри / пошук

## Розширена функціональність
### Аутентифікація

    Реєстрація користувачів

    Вхід / вихід

    Ролі:

        admin

        user

### Тригери

    ticket_audit_insert

    ticket_audit_delete

### Збережені процедури

    buy_ticket(customer, session, seat)

    cancel_ticket(ticket_id)

    session_revenue(session_id)

### Журнали логів

Таблиця ticket_audit

### Представлення (VIEW)

v_sessions_detailed

v_tickets_detailed

v_session_occupancy

### Аналітичні запити

    ТОП-фільми по прибутку

    Заповнюваність залів

    Продажі по вікових групах клієнтів

### API (JSON)

    /api/films

    /api/sessions

    /api/tickets

## Висновки
У ході роботи було:

спроєктовано ER-діаграму та логічну модель БД

створено повноцінну реляційну базу PostgreSQL

наповнено тестовими даними

реалізовано консольний застосунок

створено вебзастосунок Flask з HTML-шаблонами

реалізовано збережені процедури, тригери, в’юхи та логування

побудовано аналітичні SQL-звіти

виконано повний рівень 3 з аутентифікацією та API
