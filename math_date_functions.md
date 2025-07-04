# MATH and DATE FUNCTION

### Math operators:

- addition (+)
- subtraction (-)
- division (/)
- multiplication (*)
- modulo(% returns the remainder)

#### Math functions:

* ABS(): returns the absolute value of the input expression
* CAST(): converts an expression into another data type

#### Date and time functions:

* DATETIME(): returns the date and time of a time string
* DATE(): returns the date portion of a time string
* TIME(): returns the time portion of a time string
* STRFTIME(): returns a formatted date

#### The following tables contain mock data and can be used for practice,

- <b>bakery</b>: table storing information about orders made at a local bakery
- <b>guesses</b>: table storing the values of a guessing game where the guessed value closest to a number wins

```SQL
SELECT  * 
FROM bakery
LIMIT 5;
```
<img src="https://github.com/user-attachments/assets/e0710e81-8826-4de0-bdcc-baca5f0764c4" width=220 />

```SQL
SELECT price * quantity
FROM bakery;
```
* Using the guesses table with the ABS() function, write a query to find out how close each student’s guess is to the actual number of jelly beans in the jar, which is 804. Select the first name and the absolute difference of each guess from 804.
```SQL
SELECT first_name, ABS(guess - 804)
FROM guesses;
```
<img src="https://github.com/user-attachments/assets/c6b40efd-5aed-4b57-babd-445913036165" width=220 />

* Find out the absolute value of the difference between the average guess of all students from the actual jelly bean count, which is 804

```SQL
SELECT first_name, ABS(AVG(guess) - 804)
FROM guesses;
```

<img src="https://github.com/user-attachments/assets/e5a4fcaa-1f93-4a81-b894-3b136dad3315" width=220 />

#### Cast
he CAST() function is used to convert the value of an expression into another data type.

It has the following syntax:

```SQL 
SELECT CAST(expr AS type-name);
```

* From the bakery table, we want to determine the final price of each order after the discount is applied. However, the value of each discount is currently stored as TEXT, like '1.00 off', so you will need to convert them into numerical values.
* Utilize CAST() to convert the values of the discount column into REAL values. Select the item_name column and the total cost of each order, after applying the discount for each item.

```SQL
SELECT item_name, (price - CAST(discount AS REAL)) * quantity
FROM bakery;
```
<img src="https://github.com/user-attachments/assets/e5d91537-c227-4583-81f4-7e2f0a4042dc" width=220 >

#### Date and Time Functions I

```SQL 
SELECT DATETIME('2020-09-01 17:38:22'), DATETIME('now'), DATETIME('now', 'localtime');
```

<img src="https://github.com/user-attachments/assets/a116d753-816a-400a-884b-1e62314c1112" width=220 />


The following modifiers can be used to shift the date backwards to a specified part of the date.

* <b>start of year</b>: shifts the date to the beginning of the current year.
* <b>start of month</b>: shifts the date to the beginning of the current month.
* <b>start of day</b>: shifts the date to the beginning of the current day.

```sql 
SELECT DATE('2005-09-15', 'start of month');
```

The following modifiers add a specified amount to the date and time of the time string.

- +-N years   : offsets the year
- +-N months  : offsets the month
- +-N days    : offsets the day
- +-N hours   : offsets the hour
- +-N minutes : offsets the minute
- +-N seconds : offsets the second

```SQL 
SELECT DATETIME('2020-02-10', 'start of month', '-1 day', '+7 hours');
```
<img src="https://github.com/user-attachments/assets/c4250b23-f7a8-4bc7-8639-fe6105528747" width=220 />

* For each order in the bakery table, the order will be ready for pick up 2 days after the order is made, at 7:00AM, 7 days a week. The order will always be ready 2 days after the order, no matter what time of day the order was made.

```SQL
SELECT DATETIME(order_date, 'start of day', '+2 days', '+7 hours')
FROM bakery;
```

<img src = "https://github.com/user-attachments/assets/cef62037-ba1c-4154-a602-95c5f9d26610" width=220 />
