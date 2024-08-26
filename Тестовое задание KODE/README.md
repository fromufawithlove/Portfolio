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









 

