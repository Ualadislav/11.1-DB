# Домашнее задание к занятию 11.1. «Базы данных, их типы»

---

### Задание 1. СУБД

### Кейс
Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать 
правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для
каждой предметной области. 

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему? 
 
1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков.
СУБД должна гарантировать целостность и чёткую структуру данных.

Ответ: Для данной структуры рекомендую использовать реляционную БД. Особый плюс таких баз — нормализация данных: они делятся на разные таблицы, поэтому исключены повторяющиеся или пустые ячейки. Транзакции реляционных БД соответствуют ACID — набору свойств, который гарантирует их надёжную обработку

1.1.* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы? 

1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к 
маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? 
СУБД должны быть гибкими и быстрыми.

Ответ: Для данного проекта использовать Nosql с графовой структурой. Сложные взаимосвязи лучше реализовать через графовые БД, поскольку в графовых используется более естественный и гибкий подход к данным

1.2.* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?

1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу 
и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.

Ответ: Нормы и правила лучше будет хранить в документо-ориентированной БД. Я предлогаю внедрить MongoDB, поскольку она отлично масштабируется.

1.3.* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это 
реализовать?

Ответ: Мой ответ будет- нет. Поскольку была выбрана MongoDB, в которой отсутствуют связи и возможности объединения.

1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов 
по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать
со связями.

Ответ: Внедряем графовую СУБД, подойдет для этой задачи. т.к. такие БД используют структуру графов для семантических запросов. Данные хранятся в виде узлов, ребер и свойств. Это подходит для данной задачи.

1.4.* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД 
логистов?

1.5.* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?

*Приведите ответ в свободной форме.*

---

### Задание 2. Транзакции

2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы 
транзакция завершилась успешно. Ориентируйтесь на шесть действий.

Ответ: 

Вариант 1:

Рассмотрю оплату через терминал
1. Пользователь отправляет запрос на пополнение баланса, путем ввода номера в терминале для оплаты. А)в офисе мтс(у меня мтс) тут переходим к пункту 2. Б) в терминал в переходе, для начала выбирает оператора, вводит номер.

2. "Запрос" на пополнение поступает оператору. 2.1 Происходит сверка введенного номера со списком аббонентов. Запрос возвращается на терминал и А) разрешает ввод если номер введен верно Б) пользователь видит ошибку. У меня номер введен верно.

3. Пользователь вносит необходимую сумму, терминал "видит" указанную сумму.

4. Терминал направляет ответ оператору на оплату связи введенному номеру.

5. Оператор зачисляет(подразумевается, что в БД происходит увеличение балланса на использование услугами связи, а не реальное пополнение т.к. деньги фактически находятся в банке,а кэш в терминале, который потом попадет в банк): происходит поиск аббонента в БД оператора, изменение баланса счёта, отправка сообщения об успешном пополнении на терминал(выдать чек), на телефон(уведомление о пополнении).

6. С терминала отправляется информация в банк о платеже данному оператору. Банк получив запрос, производит поиск счета оператора, производит начисление средств на счёт оператора. Но нам это уже не важно.

Вариант 2:

1. Пользователь ~~берет модный смартфон и~~ заходит в приложение ~~,скажем я не поленился и в 2022 установил приложение из googleplay,~~ МТС. ~~Жмет нужные кнопочки и приложение направляет запрос на пополнение в банк~~ , вводит сумму, приложение отправляет запрос в банк

2. Банкт получив счет на оплату от оператора производит поиск аббонента

3. Проверку наличия указанной суммы на банковском счету аббонета. Два вариант: деньга есть-все ок идем дальше. деньги нету-возвращает ошибку, приложение мтс говорит мне ~~что я нищеброд~~ о нехватке средств, расходимся

4. ~~Я не нищеброд~~Вариант 1: средства есть. Производит списание указанной суммы в пользу мтс(тут тоже идет обращение к бд на поиск счета оператора в данном регионе), у аббонента вычитается указанная сумма со счета.

5. Банк отправляет ответ в приложение МТС, направляет изменения суммы на счету в приложение банка.

6. Оператор присваивает потраченную сумму к номеру аббонента

2.1.* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?

*Приведите ответ в свободной форме.*

~~Без 6 пунктов окей?~~
Вариант А: оператор сам направляет в указанную дату(скорее всего в базе данных есть такая графа или даже отдельная таблица ~~крутых как яйцо пользователей, которые не считают деньги~~ с пользователями подключившими автоплатеж), направляет в указанную дату запрос в банк на списание, и все тоже самое с пункта 5.

Вариант Б: автоплатеж в банке, банк в указанную дату производит зачисление на счет оператора,с указанным идентификатором аббонента, оперетор получает денежку и тут пункт 6.

---

### Задание 3. SQL vs NoSQL

3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL. 

Ответ: На самом деле трудно сравнивать то, что предназначено к разным инструментом по решению задач, это как сравнивать пилу и молоток- что лучше? Если спилить дерево- пила, забить гвозь- молоток, так и тут.

1. Atomicity, или атомарность — ни одна транзакция не будет зафиксирована в системе частично.

2. Consistency, или непротиворечивость — фиксируются только допустимые результаты транзакций.

3. Isolation, или изолированность — на результат транзакции не влияют транзакции, проходящие параллельно ей.

4. Durability, или долговечность — изменения в базе данных сохраняются несмотря на сбои или действия пользователей.

5. Поддержка: SQL: так как история SQL начинается в 1970-х, вокруг этой идеологии работы с данными сложилось обширное сообщество, готовое помочь тем, у кого возникают сложности. Наработан большой опыт применения SQL в самых разных проектах

NoSQL: так как БД NoSQL гораздо моложе БД SQL, они отличаются меньшим сообществом, для которого характерна большая разобщённость из-за разных подходов, использующихся в разных NoSQL-базах данных.

3.1.* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.

*Приведите ответ в свободной форме.*

---

### Задание 4. Кластеры

Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу 
выделено 1000 машин. 

На основе какого критерия будете выбирать тип СУБД и какая модель *распределённых вычислений* 
здесь справится лучше всего и почему?

*Приведите ответ в свободной форме.*

Ответ: СУБД я выбираю исходя из количества машин и того, как они взаимодействуют между собой. Т.к. у нас "распределенные вычисления" машины связаны между собой. Выбираем сетевую NoSQL " т.к. сетевые базы данных имеют иерархическую структуру, но в отличие от иерархических баз данных, где у одного дочернего элемента может быть только один родитель, сетевой узел может иметь отношения с несколькими объектами.

---

Задания,помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже разобраться в материале/
