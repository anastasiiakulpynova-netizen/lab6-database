# Лабораторна робота №6
## Реляційна алгебра. Побудова запитів до бази даних

**Виконала:** [Кульпинова Анастасія Дмитрівна]  
**Група:** [ЗКС11]  
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
┌─────────────┐ ┌─────────────┐
│ Student │ │ Group │
├─────────────┤ ├─────────────┤
│student_id PK│◄────────│ group_id PK │
│name │ FK │ group_name │
│surname │ └─────────────┘
│email │
│group_id FK │
└─────────────┘

┌─────────────┐ ┌─────────────┐
│ Teacher │ │ Subject │
├─────────────┤ ├─────────────┤
│teacher_id PK│ │subject_id PK│
│name │ │subject_name │
│surname │ └─────────────┘
│email │
└─────────────┘

┌─────────────────────────────────────────┐
│ Schedule │
├─────────────────────────────────────────┤
│ schedule_id PK │
│ date │
│ time │
│ group_id FK ──────────────► Group │
│ teacher_id FK ────────────► Teacher │
│ subject_id FK ────────────► Subject │
└─────────────────────────────────────────┘
┌─────────────────┐
│ Administrator │
├─────────────────┤
│ admin_id PK │
│ name │
│ email │
└─────────────────┘

---

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
