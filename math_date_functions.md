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






"
