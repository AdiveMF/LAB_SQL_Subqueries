# LAB_SQL_Subqueries
LAB_SQL_Subqueries

USE sakila;

-- 1. Determinar el número de copias de la película "Hunchback Impossible" en el inventario
SELECT COUNT(*)
FROM inventory
WHERE film_id = (SELECT film_id FROM film WHERE title = 'Hunchback Impossible');

-- 2. Listar todas las películas cuya duración sea mayor que la duración promedio de todas las películas
SELECT title
FROM film
WHERE length > (SELECT AVG(length) FROM film);

-- 3. Mostrar todos los actores que aparecen en la película "Alone Trip"
SELECT actor_id, first_name, last_name
FROM actor
WHERE actor_id IN (SELECT actor_id FROM film_actor WHERE film_id = (SELECT film_id FROM film WHERE title = 'Alone Trip'));

-- BONUS
-- 4. Identificar todas las películas categorizadas como familiares
SELECT title
FROM film
WHERE film_id IN (SELECT film_id FROM film_category WHERE category_id = (SELECT category_id FROM category WHERE name = 'Family'));

-- 5. Recuperar el nombre y correo electrónico de los clientes de Canadá utilizando subconsultas y joins
SELECT customer.first_name, customer.last_name, customer.email
FROM customer
WHERE address_id IN (SELECT address_id FROM address WHERE city_id IN (SELECT city_id FROM city WHERE country_id = (SELECT country_id FROM country WHERE country = 'Canada')));

-- 6. Determinar las películas protagonizadas por el actor más prolífico
SELECT film.title
FROM film
WHERE film_id IN (SELECT film_id FROM film_actor WHERE actor_id = (SELECT actor_id FROM film_actor 
GROUP BY actor_id 
ORDER BY COUNT(film_id) DESC 
LIMIT 1));

-- 7. Encontrar las películas alquiladas por el cliente más rentable

-- 8. Recuperar el client_id y el total_amount_spent de los clientes que gastaron más que el promedio
SELECT customer_id, SUM(amount) AS total_amount_spent
FROM payment
GROUP BY customer_id
HAVING total_amount_spent > (SELECT AVG(total) FROM (SELECT SUM(amount) AS total FROM payment GROUP BY customer_id) AS subquery);


