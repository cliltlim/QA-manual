**Работа с SQL** 

Создание таблиц по примеру 

**create** **table** Client (

ClientID **INT** **primary** **key**,

LastName **VARCHAR** (50),

FirstName **VARCHAR** (50),

BirthDate **DATE**,

Phone **VARCHAR**(17) **check** (Phone \~ '^\\+\[0-9]\[\[:space:]]\[0-9]{3}\[\[:space:]]\[0-9]{3}\[\[:space:]]\[0-9]{2}\[\[:space:]]\[0-9]{2}$'));

**insert** **into** Client (ClientID, LastName, FirstName, BirthDate, Phone) **values**

(2,'Липская', 'Виктория', '1980-02-13', '+7 911 111 11 11' ),

(3, 'Пуртов', 'Дмитрий','1971-06-23', '+7 962 714 45 23' ),

(4, 'Нестерова', 'Анастасия','1977-08-25', '+7 965 236 54 44'),

(5, 'Григоров', 'Виталий', '1988-01-27', '+7 962 222 22 22'),

(6, 'Лимонов', 'Петр', '1960-05-30', '+7 951 212 22 22');

// номер телефона приведен к единому виду для удобства 

**create** **table** Orders (

OrderID **INT** **primary** **key**,

ClientID **INT** **references** Client(ClientID),

OrderDate **date**);

**insert** **into** Orders (OrderID, ClientID, OrderDate ) **values**

(111202, 2, '2015-07-14' ),

(111205, **NULL**, '2015-07-14'),

(111206,5, '2015-07-22'),

(111207,**Null** , '2015-07-24'),

(111208, 2, '2015-07-26'),

(111209, 2, '2015-08-30');

_// Null установлено тк мы имеем только части таблиц и в таблице Client нет ClientID 55 и 44,  а тк это foreign key для таблицы Orders, то нам выдаст ошибку если вписать данные которых нет в таблице Client_

\
\
\
\
\
\
\
\
\


Данные таблицы неполные поэтому напишем для начала запрос который посчитает количество заказов по тем данным, что у нас есть 

**select** _c_.LastName, _c_.FirstName, **COUNT**(_o_.OrderID) **as** _OrdersCount_ **from** CLient _c_

**left** **join** Orders _o_

**on** _c_.CLientID = _o_.ClientID

**group** **by**

_c_.LastName, _c_.FirstName

**order** **by**

OrdersCount **desc**;

**Получаем примерно такую таблицу** 

lastname |firstname|orderscount|

\---------+---------+-----------+

Липская  |Виктория |          3|

Григоров |Виталий  |          1|

Лимонов  |Петр     |          0|

Пуртов   |Дмитрий  |          0|

Нестерова|Анастасия|          0|

В задании указано что необходимо вывести только тех покупателей, количество заказов у которых больше 25 

для этого необходимо добавить ограничение having

 

**ФИНАЛЬНОЕ РЕШЕНИЕ** 

 

**select** _c_.LastName, _c_.FirstName, **COUNT**(_o_.OrderID) **as** _OrdersCount_ **from** CLient _c_

**left** **join** Orders _o_

**on** _c_.CLientID = _o_.ClientID

**group** **by**

_c_.LastName, _c_.FirstName

**Having**

**Count**(\*) > 25

**order** **by**

OrdersCount **desc**;

 

теперь в ответе мы увидим таблицу содержащую в себе информацию о тех пользователях количество заказов у которых более 25
