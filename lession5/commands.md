Практическое задание по теме «Операторы, фильтрация, сортировка и ограничение»

1. Пусть в таблице users поля created_at и updated_at оказались незаполненными. Заполните их текущими датой и временем.
1.1. Имитация незаполненных колонок
UPDATE `users` SET `created_at` = NULL;
UPDATE `users` SET `updated_at` = NULL;
1.2. Вставляем текущую дату в колонку updated_at
UPDATE `users` SET `updated_at` = NOW();
1.3. Вставляем вчерашнюю дату в колонку created_at, чтобы даты отличались
UPDATE `users` SET `created_at` = NOW() - INTERVAL 1 DAY;

2.Таблица users была неудачно спроектирована. Записи created_at и updated_at были заданы типом VARCHAR и в них долгое время помещались значения в формате 20.10.2017 8:10. Необходимо преобразовать поля к типу DATETIME, сохранив введённые ранее значения.
UPDATE users SET created_at = str_to_date('created_at', '%d.%m.%Y %H:%i');
ALTER TABLE `users` CHANGE `created_at` `created_at` DATETIME;

3.В таблице складских запасов storehouses_products в поле value могут встречаться самые разные цифры: 0, если товар закончился и выше нуля, если на складе имеются запасы. Необходимо отсортировать записи таким образом, чтобы они выводились в порядке увеличения значения value. Однако нулевые запасы должны выводиться в конце, после всех записей.
SELECT * FROM `storehouses_products` ORDER BY CASE WHEN value = 0 THEN 1 ELSE 0 END, value //Работет, но не пойму как я сам мог бы дойти до этого


Практическое задание теме «Агрегация данных»

1.Подсчитайте средний возраст пользователей в таблице users.
SELECT YEAR(NOW()) - AVG(YEAR(birthday_at)) FROM `users`;
SELECT AVG(TIMESTAMPDIFF(YEAR, birthday_at, NOW())) FROM users #Измеряет точнее 

2.Подсчитайте количество дней рождения, которые приходятся на каждый из дней недели. Следует учесть, что необходимы дни недели текущего года, а не года рождения.
SELECT DAYNAME(DATE_ADD(birthday_at, INTERVAL YEAR(NOW()) - YEAR(birthday_at) YEAR)) as day, COUNT(*) as quantity FROM users GROUP BY day; #Было трудновато с этой задачей ))
