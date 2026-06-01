---
tags:
  - databases
  - netology
---
### Создание БД. Подключение через psql

создание: `createdb -U postgres <название>`
подключение: `psql -U postgres -d <название>`
список всех команд: `\?`
выход: `exit` или `\q`
удаление: `dropdb -U postgres <название>`


### SQL, механизм взаимодействия с БД

SQL - это язык структурированных запросов. Данный язык понимает СУБД, которая и производит операции с данными

Типы запросов:
-  DDL (Data Definition Language) - CREATE, ALTER, DROP
-  DML (Data Manipulation Language) - SELECT, INSERT, UPDATE, DELETE
-  TCL (Transaction Control Language) - COMMIT, ROLLBACK, SAVEPOINT
-  DCL (Data Control Language) - GRANT, REVOKE, DENY

"TokenAuthentication" - создание таблицы
Синтаксис:
```
CREATE TABLE [IF NOT EXISTS] <name> (
	<col_name_1> <col_type_1> [CONSTRAINTS],
	...,
	<col_name_N> <col_type_N> [CONSTRAINS],
	[CONSTRAINS]
);
```
Пример:
```
CREATE TABLE IF NOT EXISTS Student (
	Id serial PRIMMARY KEY,
	Name varchar(40) NOT NULL,
	GPA real CHECK(GPA>)
);
```

Типы:
- integer - целые числа
- serial - целые числа с автоинкрементом
- numeric - десятичные числа
- character vrying (varchar) - строки ограниченной длины
- text - строки произвольной длины
- date - дата (без времени)
- timestamp - дата + время 
- boolean - булевые значения
- jsonb - JSON-поля

Ограничения:
- primary key - первичный ключ, обязывает поле быть уникальным и не пустым 
- not null - значение не может быть пустым
- unique - все значения в этом поле должны быть уникальными
- check - добавить проверку значения на описанное условие
- foreign key - внешний ключ, обязывает значение соответствовать значению из другой таблицы

Ключевые слова - заглавные буквы
Названия - строчные буквы

Пример в cmd:

`CREATE TABLE student (id SERIAL PRIMARY KEY, name VARCHAR(60) NOT NULL);`
Пропустить создание если таблица создана:
`CREATE TABLE IF NOT EXISTS student (id SERIAL PRIMARY KEY, name VARCHAR(60) NOT NULL);`
Изменить столбик:
`ALTER TABLE student RENAME name TO first_name;`
Удаление:
`DROP TABLE student;`


### DBeaver


### Связи между отношениями

Типы:
- Один к одному - пользователь и дополнительная информация о нем
- Один ко многим - домашние задания на курсе
- Многие ко многим - пользователи и курсы

Пример таблицы
```
-- все, что начинается с двух дефисов - это комментарий

-- один к одному (1 вариант)

--CREATE TABLE IF NOT EXISTS Student (
--	email VARCHAR(80) PRIMARY KEY,
--	name VARCHAR(40) NOT NULL,
--	password VARCHAR(128) NOT NULL
--);
--
--CREATE TABLE IF NOT EXISTS StudentInfo (
--	email VARCHAR(80) PRIMARY KEY REFERENCES Student(email),
--	birthday date,
--	city VARCHAR(60),
--	roi TEXT
--);

-- один к одному (2 вариант)

CREATE TABLE IF NOT EXISTS Student (
	id SERIAL PRIMARY KEY,
	email VARCHAR(80) UNIQUE NOT NULL,
	name VARCHAR(40) NOT NULL,
	PASSWORD VARCHAR(128) NOT NULL
);

CREATE TABLE IF NOT EXISTS StudentInfo (
	id INTEGER PRIMARY KEY REFERENCES Student(id),
	birthday date,
	city VARCHAR(60),
	roi TEXT
);

-- один ко многим

CREATE TABLE IF NOT EXISTS Course (
	id SERIAL PRIMARY KEY,
	name VARCHAR(60) NOT NULL,
	description TEXT
);

CREATE TABLE IF NOT EXISTS HomeworkTask (
	id SERIAL PRIMARY KEY,
	course_id INTEGER NOT NULL REFERENCES Course(id),
	number INTEGER NOT NULL,
	description TEXT NOT NULL
);

-- многие ко многим (1 вариант)

CREATE TABLE IF NOT EXISTS CourseStudent (
	course_id INTEGER REFERENCES Course(id),
	student_id INTEGER REFERENCES Student(id),
	CONSTRAINT pk PRIMARY KEY (course_id, student_id)
);

-- многие ко многим (2 вариант)

CREATE TABLE IF NOT EXISTS HomeworkSolution (
	id SERIAL PRIMARY KEY,
	task_id INTEGER NOT NULL REFERENCES HomeworkTask(id),
	student_id INTEGER NOT NULL REFERENCES Student(id),
	solution TEXT NOT NULL
);

```



