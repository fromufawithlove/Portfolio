## Тестовое задание в KODE. Аналитика

### Задание 1

Вводная:  

Ты работаешь в команде разработки мобильного приложения, и вашимпродуктом является сервис заказа такси. Целевой сценарий для сервиса — это заказ такси. Сценарий большой, включает в себя заполнение данных
пользователя (адреса, выбор класса обслуживания и тд) и дальнейшую оплату.  

Задача:  

Смоделируй диаграмму, в рамках которой будет описано:  
- Действия пользователя по шагам
- Клиент-серверное взаимодействие на каждый шаг? который совершает
пользователь

Диаграмму необходимо выполнить в нотации UML SD (Sequence Diagram). Выполнить ее можно как с помощью plantUML, так и через любойграфический редактор. Диаграмма должна описывать сценарий от заполнения данных для заказа такси до факта начала поездки.  

Подсказки:
- Не забывай про альтернативные сценарии
- Внимательно изучи как это работает у конкурентов, чтобы переиспользовать лучшие решения.
- Обрати внимание на участников процесса (спойлер, это не про людей).

В рамках диаграммы при проектировании клиент-серверного взаимодействия можешь указать REST запросы с описанием типа запроса и пути.

### Ответ

![](https://github.com/shatalina/data_science_YP/blob/main/%D0%A2%D0%B5%D1%81%D1%82%D0%BE%D0%B2%D0%BE%D0%B5%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20KODE/taxi.jpg)

 REST запросы:  

- Редактирование данных пользователя

/users/:id  PUT

surname     string  
name        string  
email       string  
phone       string  
city        string  
dateofbirth string  

- Авторизация  

/auth/signup POST  

email    string
password string  

- Выход из системы

/auth/sighout POST

- Просмотр истории заказов

/orders/history GET

orders              array  
orders[].id         string  
orders[].datetime   datetime  
orders[].adressout  array  
orders[].adressin   array  
orders[].tariff     string  
orders[].animal     boolean  
orders[].child      string  
orders[].options    array  
orders[].comments   string  
orders[].cost       number  
orders[].carnumber  string  
orders[].cardriver  string  
orders[].carcolor   string  
orders[].carmodel   string  
orders[].payment    string  
orders[].status     boolean  
orders[].score      number  
orders[].tips       number  


- Обращение в техподдержку

запрос  
/support/ POST

id         string  
datetime   datetime  
theme      string  
message    string  

ответ  
/support/:id GET  

id         string  
datetime   datetime  
theme      string  
answer     string  

- Заказ такси

Формирование заказа  

/orders/  POST 

id         string  
datetime   datetime  
adressout  array  
adressin   array  
tariff     string  
animal     boolean  
child      string  
options    array  
comments   string  
payment    string  

- Предложение сервиса

/orders/:id/preview  GET  

id         string  
datetime   datetime  
adressout  array  
adressin   array  
tariff     string  
animal     boolean  
child      string  
options    array  
comments   string  
payment    string  
cost       number  

- Согласие пользователя на заказ
  
/orders/:id/preview PUT  

status     boolean  

- Детали поездки

/orders/:id  GET  

id         string  
datetime   datetime  
adressout  array  
adressin   array  
tariff     string  
animal     boolean  
child      string  
options    array  
comments   string  
payment    string  
cost       number  
carnumber  string  
cardriver  string  
carcolor   string  
carmodel   string  
status     boolean  

- Отмена поездки
  
/orders/:id  PUT  

status     boolean  

### Задание 2

Бизнес захотел добавить вызов такси другому человеку. Подумай, какие вопросы ты бы мог задать заказчику для того, чтобы в дальнейшем спроектировать эту функциональность?
Подсказки:  
- Для аналитика важно понимать не только устройство системы, но и ценность функциональности для бизнеса.
- Не усложняй формат, стоит написать просто понятные и четкие вопросы (достаточно будет около 5).

Решение представь в текстовом формате.

### Ответ:

- Какую бизнес-цель вы хотите достичь, добавив новый функционал?
- Как вы будете оценивать, что новый функционал работает удачно?
- Минимальные данные, которые вам необходимо получить о другом, для которого будет заказан сервис?
- Назовите ограничения для другого -  возраст, наличие телефона, наличие приложения сервиса и т.д.
- Назовите ограничения для пользоватетя приложения - когда он не может воспользоваться новым функционалом и почему?

### Задание 3

Вводная:  

Сервис доставки такси выяснил, что назначение ближайшего такси на вызов не самый эффективный вариант для распределения заказов. Они столкнулись с недовольством пользователей, так как алгоритм поиска машины работал долго и неэффективно.  

Задача:  

Продумай функциональные требования для алгоритма поиска машины по заказу. Опиши, как программа распределения заказов должна работать, какие иные критерии заказа должны быть учтены и как? Учитывай, что при распределении заказов таксист должен подтвердить взятие заказа, а пользователь должен уметь управлять заказом после
назначения автомобиля?

Подсказки
- Часто требования легче понять, если они еще и визуализированы с
помощью схемы  
- Изучи предметную область и подумай над альтернативными сценариями, но и не забывай про то, что можно вносить свои
предложения
- Помни про границы системы, не надо описывать всю работы приложения с техническими деталями, ограничься процессом от получения информации об оформленном заказе и до начала поездки.
  
Решение представь в текстовом формате (если будет схема, то вставь ее как изображение).

### Ответ:
  











 

