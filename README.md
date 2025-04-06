# trainSQL

Для меня:
- WHERE + ... + LIKE + '%'
- CURDATE() - текущая дата
- DATEDIFF(d1, d2) - разница в днях между датами
- FLOOR() - округление в меньшую сторону
- AVG(...) — берёт среднее значение 

# СЕМЬЯ
<img width="549" alt="image" src="https://github.com/user-attachments/assets/cef8b63d-a544-44e0-99f4-9dabd374314b" />

№21
Определить товары, которые покупали более 1 раза

```
SELECT good_name
FROM(
    SELECT g.good_name, COUNT(p.good) as cnt
    FROM Goods g
    JOIN Payments p
        ON g.good_id = p.good 
    GROUP BY g.good_name
    HAVING cnt > 1) t
```

№23
Найдите самый дорогой деликатес (delicacies) и выведите его цену
```
SELECT g.good_name, p.unit_price
FROM Payments p
JOIN Goods g 
    ON p.good = g.good_id
JOIN GoodTypes gt
    ON g.type = gt.good_type_id 

WHERE good_type_name = 'delicacies'

ORDER BY unit_price DESC
LIMIT 1
```

№24
Определить кто и сколько потратил в июне 2005
```
SELECT member_name, SUM(amount * unit_price) as costs
FROM FamilyMembers f 
JOIN Payments p
    ON f.member_id = p.family_member

WHERE 1=1
    AND MONTH(date) = 6
    AND YEAR(date) = 2005

GROUP BY member_name

```

№25
Определить, какие товары не покупались в 2005 году
```
SELECT good_name

FROM Goods g 
LEFT JOIN(
SELECT *
FROM Payments p 
WHERE YEAR(date) = 2005) t
    ON g.good_id = t.good

WHERE payment_id IS NULL
```

№26
Определить группы товаров, которые не приобретались в 2005 году
```
SELECT good_type_name

FROM(
    SELECT *

    FROM Goods g 
    JOIN Payments p
        ON g.good_id = p.good
    
    WHERE YEAR(date) = 2005) t

RIGHT JOIN GoodTypes gt 
    ON t.type = gt.good_type_id

WHERE date IS NULL
```

№27
Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.
```
SELECT good_type_name, SUM(amount * unit_price) AS costs
FROM Payments p
JOIN Goods g 
    ON p.good = g.good_id
JOIN GoodTypes gt
    ON g.type = gt.good_type_id 
    
WHERE YEAR(date) = 2005

GROUP BY good_type_name

HAVING costs > 0
```

№31
Вывести всех членов семьи с фамилией Quincey.
```
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '% Quincey'
```

№32
Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.
```
SELECT FLOOR(AVG(FLOOR(DATEDIFF(CURDATE(), birthday)/365))) AS age
FROM FamilyMembers
```

№33
Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры.
```
SELECT AVG(unit_price) AS cost
FROM Payments p
	JOIN Goods g ON p.good = g.good_id
WHERE 1 = 1
	AND good_name = 'red caviar'
	OR good_name = 'black caviar'
```



# АВИА
<img width="549" alt="image" src="https://github.com/user-attachments/assets/83678e0a-b25c-4e37-a0f0-a5743423fecb" />

№29
Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов.
```
SELECT DISTINCT  name
FROM Passenger pr 
JOIN Pass_in_trip p 
    ON pr.id = p.passenger
JOIN Trip t 
    ON p.trip = t.id

WHERE 1=1
    AND town_to = 'Moscow'
    AND plane = 'TU-134'
```

№30
Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.
```
SELECT trip, COUNT(trip) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC
```



# РАСПИСАНИЕ
<img width="554" alt="image" src="https://github.com/user-attachments/assets/44932ec6-586c-4e33-ad00-093ce3e482ee" />

№35
Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?
```
SELECT COUNT(classroom) AS COUNT
FROM(
	SELECT DISTINCT classroom
	FROM Schedule
	WHERE 1 = 1
		AND DAY(date) = 2
		AND MONTHNAME(date) = 'September'
	) t
```

№37
Сколько лет самому молодому обучающемуся ?
```
SELECT FLOOR(DATEDIFF(CURDATE(), birthday)/365) AS year 
FROM Student
ORDER BY year 
LIMIT 1
```

№40
Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такой фамилией.
```
SELECT DISTINCT name AS subjects
FROM Schedule s
	JOIN Teacher t ON s.teacher = t.id
	JOIN Subject sub ON s.subject = sub.id
WHERE 1 = 1
	AND first_name LIKE 'P%'
	AND middle_name LIKE 'P%'
	AND last_name = 'Romashkin'
```

№42
Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?
```
SELECT TIMEDIFF(
    (
    SELECT end_pair
    FROM Timepair
    WHERE id = 4
    ),
    (
    SELECT start_pair
    FROM Timepair
    WHERE id = 2
    )
) AS time
FROM Timepair
LIMIT 1
```
