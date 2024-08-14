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


#### 1. Вывести на экран только запись с  полями последнего заказа и названием купленной программы для каждого пользователя.

SELECT orders.*, programs.name  
FROM orders  
INNER JOIN programs ON orders.programs_id = programs.id  
WHERE buy_date =  
    (SELECT MAX(buy_date) FROM orders AS ab WHERE orders.user_id=ab.user_id)  
ORDER BY user_id;  

#### 2. С помощью запроса проверить таблицу orders на дубликаты записей о покупках.

SELECT  user_id, programs_id, order_date, buy_date, state, order_sum, count(*)  
FROM    orders  
GROUP BY user_id, programs_id, order_date, buy_date, state, order_sum  
ORDER BY user_id, programs_id, order_date, buy_date, state, order_sum;  

Выведет таблицу orders со столбцом - количеством упоминаний в таблице.

#### 3. Используя запрос, найти пятое по сумме продаж направление обучения.

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

#### 4. Подсчитать распределение количества клиентов по 5ти бинам по сумме оплаты по направлению “Аналитика” за 4й квартал 2023 года (использовать следующие бины меньше 50000, 50000-100000, 100000-150000, 150000-200000, больше 200000).

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
   WHERE programs.direction = 'Аналитика' AND buy_date BETWEEN '01-10-2023' AND'31-12-2023'  
   GROUP BY user_id  
   ORDER BY total_sum DESC) AS a  
GROUP BY bin  
ORDER BY count_users DESC;  

#### 5. Рассчитать нарастающим итогом, как увеличивалось кол-во проданных программ каждый месяц за 2023 год с разбивкой по направлениям обучения  “Программирование” и “Аналитика”. Дополнить расчет процентом  прироста по месяцам год к году (YoY).

SELECT  
  now_year.month_buy, 
  now_year.year_buy, 
  now_year.direction,  
  now_year.count_orders,  
  now_year.count_cum,  
  ROUND((now_year.count_cum/last_year.count_cum -1)*100, 2) AS growth 

FROM   

(WITH a AS(  
    SELECT EXTRACT(MONTH FROM buy_date) as month_buy, EXTRACT(YEAR FROM buy_date) as year_buy, programs.direction, COUNT(order_id) as count_orders  
    FROM orders  
    INNER JOIN programs ON orders.programs_id = programs.id   
    WHERE programs.direction IN ('Аналитика', 'Программирование') AND EXTRACT(YEAR FROM buy_date) = '2022'   
    GROUP BY EXTRACT(MONTH FROM buy_date), EXTRACT(YEAR FROM buy_date), programs.direction   
    )
    SELECT *,  
    SUM(a.count_orders) OVER (PARTITION BY a.direction ORDER BY a.month_buy) AS count_cum  
    FROM a) AS last_year  
    
FULL OUTER JOIN  

(WITH a AS(  
    SELECT EXTRACT(MONTH FROM buy_date) as month_buy, EXTRACT(YEAR FROM buy_date) as year_buy, programs.direction, COUNT(order_id) as count_orders  
    FROM orders  
    INNER JOIN programs ON orders.programs_id = programs.id  
    WHERE programs.direction IN ('Аналитика', 'Программирование') AND EXTRACT(YEAR FROM buy_date) = '2023'  
    GROUP BY EXTRACT(MONTH FROM buy_date), EXTRACT(YEAR FROM buy_date), programs.direction  
    )  
    SELECT *,  
    SUM(a.count_orders) OVER (PARTITION BY a.direction ORDER BY a.month_buy)  
     AS count_cum   
     FROM a) AS now_year  

ON last_year.month_buy = now_year.month_buy  AND last_year.direction = now_year.direction  
ORDER BY now_year.direction, now_year.month_buy;  

#### 6. Есть ли разница в селектах?

запрос №1  

SELECT *  
FROM TableA  
LEFT JOIN TableB ON TableA.id = TableB.a_id AND TableB.column = 'value';  

запрос №2  

SELECT *  
FROM TableA  
LEFT JOIN TableB ON TableA.id = TableB.a_id  
WHERE TableB.column = 'value';  

#### Ответ:

Разница есть. Первый вариант сохранит все строки Таблицы А, заполнив null значения отличные от value. Второй вариант выведет только те строки, где условие по value выполнено.

### Python

#### 1. Напишите функцию, которая принимает список словарей, представляющих студентов и их оценки за экзамены. Функция должна вернуть список студентов, у которых средняя оценка выше определенного порога.

students = [  
    {"name": "Иван", "grades": [5, 4, 4, 5]},  
    {"name": "Мария", "grades": [3, 3, 4, 3]},  
    {"name": "Петр", "grades": [4, 4, 5, 5]}  
]

def top_students(students, threshold):  
    # Ваш код здесь  
    pass  

print(top_students(students, 4))  # Должно вернуть ["Иван", "Петр"]

#### Ответ:

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

#### 2. Напишите функцию, которая принимает строку, представляющую дату в формате "ДД-ММ-ГГГГ", и возвращает название дня недели для этой даты.

import datetime  

def day_of_week(date_string):  
    # Ваш код здесь  
    pass  

print(day_of_week("30-07-2024"))  # Должно вернуть "Вторник"

#### Ответ:

from datetime  
import datetime  

def day_of_week(date_string):  
    week = {  
            0: "Понедельник",  
            1: "Вторник",  
            2: "Среда",  
            3: "Четверг",  
            4: "Пятница",  
            5: "Суббота",  
            6: "Воскресенье"}  
            
    return week[datetime.strptime(date_string,"%d-%m-%Y").weekday()]  
    
print(day_of_week("30-07-2024"))

### Продуктовая аналитика

#### 1. План А/В тестирования

EdTech платформа запускает новую функцию "Персональные рекомендации". Эта функция использует алгоритмы машинного обучения для предложений студентам курсов и учебных материалов, наиболее соответствующих их текущим потребностям и уровням знаний. Команда хочет понять, как эта функция влияет на успеваемость студентов.

Необходимо доказать, что новая функция "Персональные рекомендации" приводит к улучшению успеваемости студентов на платформе.

Опишите, как вы будете проводить эксперимент для оценки влияния новой функции на успеваемость студентов. Какие метрики вы будете отслеживать? Как вы планируете собрать данные и как будете анализировать их, чтобы определить, есть ли статистически значимое улучшение успеваемости студентов?

(Ответ не более чем на страницу, при желании можно сопроводить решение видеозаписью с объяснением, видео увеличивает вероятность прохождения).

#### Ответ:

Мы имеем дело с А/В экспериментом, в результате которого мы должны принять решение об отмене новой функции или раскате ее на всю платформу. 
А/В эксперимент проводится в следующей последовательности: 

1. Выбор метрики и формулирование гипотез 
Метрикой в нашем случае  может быть % завершения курса и/или набранные баллы в ходе обучения. 
 
Гипотезы:  

Н0: Функция «Персональные рекомендации» не влияет на успеваемость студентов. 
Успеваемость тестовой группы = Успеваемости контрольной группы.  

Н1: Функция «Персональные рекомендации» улучшает успеваемость студентов. 
Успеваемость тестовой группы > Успеваемости контрольной группы 
 

2. Выбор способа рандомизации и параметров выборки контрольной и тестовой групп 
 
Группы должны быть случайны, однородны и обычно равны.  
 
3. Определение размеров выборки 
Размер выборки определяется по формуле. 
  
 
4. Запуск эксперимента и сбор данных 
 
Остановка эксперимента возможна только в случае критических ошибок или совсем плохих показателей метрики в тестовой группе. Эксперимент не должен быть остановлен ранее намеченного срока при хороших показателях метрики для избегания «подглядывания данных» - увеличения вероятности ошибки первого рода. 
 
5. Проверка валидности эксперимента 
 
Проведение А/А теста для обеих групп по метрике за предыдущий период времени.  Проверка Sample Ratio Mismatch - пропорция пользователей в обеих группах должна быть равна запланированной. 
 
6. Расчет результатов и принятие решения о раскате новой функции. 
 
Расчет p-value, определение статистической значимости. Принятие решения о раскате новой функции.

#### 2. Анализ продуктовой фичи (Опциональное)

Продуктовая команда в сервисе бронирования отелей разработала и выкатила фичу: на странице отеля была добавлена кнопка сравнения, при нажатии на которую система будет искать и выдавать аналогичные доступные отели в таком же формате, районе и в указанные даты. 

Вас как продуктового аналитика в команде просят проанализировать эту фичу и понять, насколько хорошо она работает.

Задачи:
скачайте сырые данные логов. Обозначения отдельных полей:
- n_days — период бронирования
-  time — время события, без даты, для сортировки порядка ивентов
-  event — событие (open_hotel - открыл страницу отеля, open_compare- открыл фичу сравнения, book - успешное бронирование)
-  sum_usd — сумма оплаты за бронирование

ссылка: https://drive.google.com/file/d/1VGXQ18-7h7cSru-IwIolPTe8603uOiKd/view?usp=sharing ;

Проанализируйте логи и дайте рекомендации, что делать с фичей дальше (желательно использовать jupyter notebook, либо создать дашборд, обязательно приложить свои выводы и рекомендации). Предпочтительнее всего прислать решение ссылкой на github.

#### Ответ

По ссылке



