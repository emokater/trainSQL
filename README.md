# trainSQL

<img width="549" alt="image" src="https://github.com/user-attachments/assets/cef8b63d-a544-44e0-99f4-9dabd374314b" />

# №21
Определить товары, которые покупали более 1 раза

SELECT good_name
FROM(
    SELECT g.good_name, COUNT(p.good) as cnt
    FROM Goods g
    JOIN Payments p
        ON g.good_id = p.good 
    
    GROUP BY g.good_name
    HAVING cnt > 1) t

  
