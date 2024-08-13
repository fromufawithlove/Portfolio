## Тестовое задание в "Умскул"

### SQL

Таблица с заказами - orders:
   - order_id - идентификатор заказа
   - user_id - идентификатор клиента
   - program_id - идентификатор программы
   - order_date - дата и время создания заказа
   - buy_date - дата и время покупки
   - state - статус заказа (success - заказ оплачен, pending - заказ в работе/закрыт и не реализован)
   - order_sum - стоимость заказа, руб

Таблица programs (справочник) с программами:order
   - id - идентификатор программы
   - name - наименование программы
   - type - тип программы (курс, специализация, профессия)
   - direction - бизнес юнит (БЮ), направление обучения: Аналитика, Программирование, Дизайн и т.д.

![](https://github.com/shatalina/data_science_YP/blob/main/%D0%A2%D0%B5%D1%81%D1%82%D0%BE%D0%B2%D0%BE%D0%B5%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%A3%D0%BC%D1%81%D0%BA%D1%83%D0%BB/Screenshot_1.jpg)


1. Вывести на экран только запись с  полями последнего заказа и названием купленной программы для каждого пользователя.

SELECT orders.*, programs.name  
FROM orders  
INNER JOIN programs ON orders.programs_id = programs.id  
WHERE buy_date =  
    (SELECT MAX(buy_date) FROM orders AS ab WHERE orders.user_id=ab.user_id)  
ORDER BY user_id;  

2. С помощью запроса проверить таблицу orders на дубликаты записей о покупках.

SELECT  user_id, programs_id, order_date, buy_date, state, order_sum, count(*)  
FROM    orders  
GROUP BY user_id, programs_id, order_date, buy_date, state, order_sum  
ORDER BY user_id, programs_id, order_date, buy_date, state, order_sum;  

Выведет таблицу orders со столбцом - количеством упоминаний в таблице.

3. Используя запрос, найти пятое по сумме продаж направление обучения.

WITH sessions AS  
  (SELECT SUM(order_sum) AS sum, programs.direction,  
  ROW_NUMBER() OVER (ORDER BY SUM(order_sum) DESC) AS rn  
  FROM orders  
  INNER JOIN programs ON orders.programs_id = programs.id  
  GROUP BY  programs.direction  
  ORDER BY sum DESC  
  LIMIT 5)  
SELECT *  
FROM sessions  
WHERE rn=5;  

Выведет сумму сбора, название направления, место в рейтинге.

4. Подсчитать распределение количества клиентов по 5ти бинам по сумме оплаты по направлению “Аналитика” за 4й квартал 2023 года (использовать следующие бины меньше 50000, 50000-100000, 100000-150000, 150000-200000, больше 200000).

SELECT COUNT(a.user_id) as count_users,  
       CASE  
       WHEN a.total_sum < 50000 THEN '<50000'  
       WHEN a.total_sum >= 50000 AND a.total_sum < 100000 THEN '50000-100000'  
       WHEN a.total_sum >= 100000 AND a.total_sum < 150000 THEN '100000-150000'  
       WHEN a.total_sum >= 150000 AND a.total_sum < 200000 THEN '150000-200000'  
       WHEN a.total_sum >= 200000 THEN '200000>'  
       END AS bin  
FROM  
   (SELECT user_id, SUM(order_sum) as total_sum  
   FROM orders  
   INNER JOIN programs ON orders.programs_id = programs.id  
   WHERE programs.direction = 'Analytics' AND buy_date BETWEEN '01-10-2023' AND'31-12-2023'  
   GROUP BY user_id  
   ORDER BY total_sum DESC) AS a  
GROUP BY bin  
ORDER BY count_users DESC;  


### Python

1. Напишите функцию, которая принимает список словарей, представляющих студентов и их оценки за экзамены. Функция должна вернуть список студентов, у которых средняя оценка выше определенного порога.

students = [
    {"name": "Иван", "grades": [5, 4, 4, 5]},
    {"name": "Мария", "grades": [3, 3, 4, 3]},
    {"name": "Петр", "grades": [4, 4, 5, 5]}
]

def top_students(students, threshold):
    # Ваш код здесь
    pass

print(top_students(students, 4))  # Должно вернуть ["Иван", "Петр"]

Ответ:

students = [
    {"name": "Иван", "grades": [5, 4, 4, 5]},
    {"name": "Мария", "grades": [3, 3, 4, 3]},
    {"name": "Петр", "grades": [4, 4, 5, 5]}
]


def top_students(students, threshold):
    array = []
    
    for i in range(0, len(students)):
        if sum(students[i]['grades'])/len(students[i]['grades']) >  threshold:      
           array.append(students[i]["name"])
    return array               
        

print(top_students(students, 4))  # Должно вернуть ["Иван", "Петр"]

2. Напишите функцию, которая принимает строку, представляющую дату в формате "ДД-ММ-ГГГГ", и возвращает название дня недели для этой даты.

import datetime

def day_of_week(date_string):
    # Ваш код здесь
    pass

print(day_of_week("30-07-2024"))  # Должно вернуть "Вторник"

Ответ:

from datetime import datetime
def day_of_week(date_string):    
     week = {
            0: "Понедельник",            
            1: "Вторник",
            2: "Среда",            
            3: "Четверг",
            4: "Пятница",            
            5: "Суббота",
            6: "Воскресенье" }
            
    return week[datetime.strptime(date_string,"%d-%m-%Y").weekday()]
    
print(day_of_week("30-07-2024"))


