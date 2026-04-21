# Лабораторна робота №6
## Реляційна алгебра. Побудова запитів до бази даних

**Виконала:** Кульпинова Анастасія Дмитрівна  
**Група:** ЗКС11 

**Варіант:** 4  

---

## 1. Вербальна модель предметної області

Предметна область — **університетський розклад**.

- Є **групи студентів**, про які відомі: ідентифікатор групи, назва групи.
- Є **студенти**, про яких відомі: ідентифікатор, ім'я, прізвище, email, група (зовнішній ключ до групи).
- Є **викладачі**, про яких відомі: ідентифікатор, ім'я, прізвище, email.
- Є **предмети**, про які відомі: ідентифікатор, назва предмета.
- Є **розклад**, який містить: ідентифікатор розкладу, дату, час, групу (FK), викладача (FK), предмет (FK).
- Є **адміністратори**, про яких відомі: ідентифікатор, ім'я, email.

**Бізнес-правила:**
- Один студент належить рівно до однієї групи.
- Одна група може містити багато студентів.
- Один викладач може вести багато предметів.
- Один предмет може вестись багатьма викладачами.
- Розклад фіксує, який викладач, у якої групи, з якого предмета, коли проводить заняття.

---

## 2. ER-модель (діаграма "сутність-зв'язок")

**Сутності та зв'язки:**

- **Student** (student_id PK, name, surname, email, group_id FK) ──► **Group** (group_id PK, group_name)
- **Teacher** (teacher_id PK, name, surname, email) ──► **Schedule** (schedule_id PK, date, time)
- **Subject** (subject_id PK, subject_name) ──► **Schedule** (schedule_id PK, date, time)
- **Schedule** зв'язує Group, Teacher, Subject
- **Administrator** (admin_id PK, name, email)

**Зв'язки:**
- Student (N) : (1) Group
- Teacher (N) : (M) Subject (через Schedule)
- Schedule (N) : (1) Group, (N) : (1) Teacher, (N) : (1) Subject
  
## 3. Source файл з роботою (визначення даних)

### Домени

```sql
CREATE DOMAIN STUDENT_ID CHAR(5);
CREATE DOMAIN NAME CHAR(20);
CREATE DOMAIN SURNAME CHAR(20);
CREATE DOMAIN EMAIL CHAR(30);
CREATE DOMAIN GROUP_ID CHAR(5);
CREATE DOMAIN GROUP_NAME CHAR(15);
CREATE DOMAIN TEACHER_ID CHAR(5);
CREATE DOMAIN SUBJECT_ID CHAR(5);
CREATE DOMAIN SUBJECT_NAME CHAR(30);
CREATE DOMAIN SCHEDULE_ID CHAR(5);
CREATE DOMAIN DATE_TYPE CHAR(10);
CREATE DOMAIN TIME_TYPE CHAR(5);
CREATE DOMAIN ADMIN_ID CHAR(5);
CREATE BASE RELATION Group
(GROUP_ID DOMAIN(GROUP_ID),
 GROUP_NAME DOMAIN(GROUP_NAME))
 PRIMARY KEY (GROUP_ID);

CREATE BASE RELATION Student
(STUDENT_ID DOMAIN(STUDENT_ID),
 NAME DOMAIN(NAME),
 SURNAME DOMAIN(SURNAME),
 EMAIL DOMAIN(EMAIL),
 GROUP_ID DOMAIN(GROUP_ID))
 PRIMARY KEY (STUDENT_ID)
 FOREIGN KEY (GROUP_ID) REFERENCES Group
   DELETE CASCADES
   UPDATE RESTRICTED;

CREATE BASE RELATION Teacher
(TEACHER_ID DOMAIN(TEACHER_ID),
 NAME DOMAIN(NAME),
 SURNAME DOMAIN(SURNAME),
 EMAIL DOMAIN(EMAIL))
 PRIMARY KEY (TEACHER_ID);

CREATE BASE RELATION Subject
(SUBJECT_ID DOMAIN(SUBJECT_ID),
 SUBJECT_NAME DOMAIN(SUBJECT_NAME))
 PRIMARY KEY (SUBJECT_ID);

CREATE BASE RELATION Schedule
(SCHEDULE_ID DOMAIN(SCHEDULE_ID),
 DATE DOMAIN(DATE_TYPE),
 TIME DOMAIN(TIME_TYPE),
 GROUP_ID DOMAIN(GROUP_ID),
 TEACHER_ID DOMAIN(TEACHER_ID),
 SUBJECT_ID DOMAIN(SUBJECT_ID))
 PRIMARY KEY (SCHEDULE_ID)
 FOREIGN KEY (GROUP_ID) REFERENCES Group
   DELETE CASCADES
   UPDATE RESTRICTED
 FOREIGN KEY (TEACHER_ID) REFERENCES Teacher
   DELETE CASCADES
   UPDATE RESTRICTED
 FOREIGN KEY (SUBJECT_ID) REFERENCES Subject
   DELETE CASCADES
   UPDATE RESTRICTED;

CREATE BASE RELATION Administrator
(ADMIN_ID DOMAIN(ADMIN_ID),
 NAME DOMAIN(NAME),
 EMAIL DOMAIN(EMAIL))
 PRIMARY KEY (ADMIN_ID);

## 4. Запити мовою реляційної алгебри

### Запит 1 (варіант 1, №1)
**Отримати повну інформацію про постачальників**

### Запит 2 (варіант 1, №2)
**Отримати всі міста проектів**

### Запит 3 (варіант 1, №3)
**Отримати повну інформацію про всі товари з Одеси**

### Запит 4 (варіант 1, №4)
**Отримати всі поєднання "ім'я проекту-місто проекту"**

### Запит 5 (варіант 1, №5)
**Отримати номери товарів, що постачаються або постачальником зі статусом більше 20, або для проекту у Харкові**

### Запит 6 (варіант 1, №6)
**Отримати імена проектів, до яких постачаються товари на ім'я Кухня**

### Запит 7 (варіант 1, №7)
**Отримати імена проектів, які поставляються товари P4**

### Запит 8 (варіант 1, №8)
**Отримати всі поставки, де кількість знаходиться в діапазоні від 300 до 500 включно**

### Запит 9 (власна предметна область)
**Отримати розклад (дата, час, предмет, викладач) для групи з group_id = 1**

### Запит 10 (власна предметна область)
**Отримати список викладачів, які проводять заняття з предмета «Бази даних»**

### Приклади даних

**Table: Group**

| group_id | group_name |
|----------|------------|
| 1 | ІПЗ-11 |
| 2 | ІПЗ-12 |
| 3 | КН-21 |
| 4 | КН-22 |

**Table: Student**

| student_id | name | surname | email | group_id |
|------------|------|---------|-------|----------|
| 101 | Олександр | Шевченко | o.shevchenko@univ.edu | 1 |
| 102 | Марія | Франко | m.franko@univ.edu | 1 |
| 103 | Андрій | Костенко | a.kostenko@univ.edu | 2 |
| 104 | Олена | Сосюра | o.sosyura@univ.edu | 2 |
| 105 | Ірина | Ліна | i.lina@univ.edu | 3 |

**Table: Teacher**

| teacher_id | name | surname | email |
|------------|------|---------|-------|
| 201 | Петро | Мельник | p.melnyk@univ.edu |
| 202 | Наталія | Бондар | n.bondar@univ.edu |
| 203 | Василь | Коваленко | v.kovalenko@univ.edu |
| 204 | Оксана | Ткаченко | o.tkachenko@univ.edu |

**Table: Subject**

| subject_id | subject_name |
|------------|--------------|
| 301 | Бази даних |
| 302 | Програмування |
| 303 | Математика |
| 304 | WEB-технології |

**Table: Schedule**

| schedule_id | date | time | group_id | teacher_id | subject_id |
|-------------|------------|----------|----------|------------|------------|
| 1 | 2026-04-21 | 10:00 | 1 | 201 | 301 |
| 2 | 2026-04-21 | 12:00 | 1 | 202 | 302 |
| 3 | 2026-04-21 | 10:00 | 2 | 203 | 303 |
| 4 | 2026-04-22 | 10:00 | 1 | 204 | 304 |
| 5 | 2026-04-22 | 12:00 | 3 | 201 | 301 |
| 6 | 2026-04-22 | 14:00 | 2 | 202 | 302 |
| 7 | 2026-04-23 | 10:00 | 1 | 203 | 303 |
| 8 | 2026-04-23 | 12:00 | 4 | 204 | 304 |

**Table: Administrator**

| admin_id | name | email |
|----------|------|-------|
| 1 | Адмін | admin@univ.edu |

