### Задание
1. Используя команду cat в терминале операционной системы Linux, создать
два файла Домашние животные (заполнив файл собаками, кошками,
хомяками) и Вьючные животными заполнив файл Лошадьми, верблюдами и
ослы), а затем объединить их. Просмотреть содержимое созданного файла.
Переименовать файл, дав ему новое имя (Друзья человека).

```bash
cat > "Домашние животные"
Собаки
Кошки
Хомяки
cat > "Вьючные животные"
Лошади
Верблюды
Ослы
cat "Домашние животные" "Вьючные животные" > Животные
cat Животные
mv Животные "Друзья человека"
```

![task1](/Images/task1.png)

2. Создать директорию, переместить файл туда.

```bash
mkdir New_dir
mv "Друзья человека" New_dir/
```
![task2](/Images/task2.png)

3. Подключить дополнительный репозиторий MySQL. Установить любой пакет
из этого репозитория.

```bash
sudo apt-get update
sudo apt update
sudo apt install mysql-server
sudo service mysql status
```
![task3](/Images/task3.png)

4. Установить и удалить deb-пакет с помощью dpkg.

```bash
wget http://ftp.us.debian.org/debian/pool/main/s/sl/sl_5.02-1_amd64.deb
sudo dpkg -i sl_5.02-1_amd64.deb
sudo dpkg -r sl
```
![task4](/Images/task4.png)

5. Выложить историю команд в терминале ubuntu
   
```bash
history
```
![task5](/Images/task5.png)

6. Нарисовать диаграмму, в которой есть класс родительский класс, домашние
животные и вьючные животные, в составы которых в случае домашних
животных войдут классы: собаки, кошки, хомяки, а в класс вьючные животные
войдут: Лошади, верблюды и ослы.

![diagram](/Images/diagram.png)

7. В подключенном MySQL репозитории создать базу данных “Друзья
человека”

```sql
CREATE DATABASE друзья_человека;
```

8. Создать таблицы с иерархией из диаграммы в БД

```sql
USE друзья_человека;

CREATE TABLE родительский_класс (
  id INT PRIMARY KEY AUTO_INCREMENT,
  type VARCHAR(20)
);

CREATE TABLE домашние_животные (
  id INT PRIMARY KEY AUTO_INCREMENT,
  type VARCHAR(20),
  FOREIGN KEY (id) REFERENCES родительский_класс(id)
);

CREATE TABLE cобакии (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20),
  action VARCHAR(20),
  date_of_birth DATE,
  FOREIGN KEY (id) REFERENCES домашние_животные(id)
);


CREATE TABLE кошки (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20),
  action VARCHAR(20),
  date_of_birth DATE,
  FOREIGN KEY (id) REFERENCES домашние_животные(id)
);


CREATE TABLE хомяки (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20),
  action VARCHAR(20),
  date_of_birth DATE,
  FOREIGN KEY (id) REFERENCES домашние_животные(id)
);


CREATE TABLE вьючные_животные (
  id INT PRIMARY KEY AUTO_INCREMENT,
  type VARCHAR(20),
  FOREIGN KEY (id) REFERENCES родительский_класс(id)
);


CREATE TABLE лошади (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20),
  action VARCHAR(20),
  date_of_birth DATE,
  FOREIGN KEY (id) REFERENCES вьючные_животные(id)
);


CREATE TABLE верблюды (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20),
  action VARCHAR(20),
  date_of_birth DATE,
  FOREIGN KEY (id) REFERENCES вьючные_животные(id)
);


CREATE TABLE ослы (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20),
  action VARCHAR(20),
  date_of_birth DATE,
  FOREIGN KEY (id) REFERENCES вьючные_животные(id)
);

SHOW TABLES;
```
![task8](/Images/task8.png)

9. Заполнить низкоуровневые таблицы именами(животных), командами
которые они выполняют и датами рождения

```sql
USE друзья_человека;

SET FOREIGN_KEY_CHECKS=0;

INSERT INTO верблюды (name, action, date_of_birth)
VALUES ('Принц', 'Плюнуть', '2017-04-12'),
       ('Желание араба', 'Таращиться', '2010-10-09'),
       ('Вера блюда', 'Отдыхать', '2020-04-19');
       
INSERT INTO лошади (name, action, date_of_birth)
VALUES ('Молния', 'Беги галопом', '2015-01-01'),
       ('Гром', 'Стоять', '2016-02-02'),
       ('Облако', 'Пить', '2017-03-03');

INSERT INTO ослы (name, action, date_of_birth)
VALUES ('Леха', 'Есть', '2018-04-04'),
       ('Ваня', 'Стоять', '2019-05-05'),
       ('Володя', 'Идти', '2020-06-06');
       
INSERT INTO кошки (name, action, date_of_birth)
VALUES ('Скользкий', 'Напасть', '2019-10-10'),
       ('Шершавый', 'Таращиться', '2020-11-11'),
       ('Отбитый', 'Справить нужду', '2020-12-12');

INSERT INTO собакии (name, action, date_of_birth)
VALUES ('Снуп', 'Фас', '2014-07-07'),
       ('Скубиду', 'Бежать', '2015-08-08'),
       ('Сосика', 'Поймать хвост', '2016-09-09');

INSERT INTO хомяки (name, action, date_of_birth)
VALUES ('Хоуми', 'Кушать', '2020-10-10'),
       ('Хома', 'Умереть', '2021-11-11'),
       ('Хоминский', 'Не умирать', '2022-12-12');
```

10. Удалив из таблицы верблюдов, т.к. верблюдов решили перевезти в другой
питомник на зимовку. Объединить таблицы лошади, и ослы в одну таблицу.

```sql
USE друзья_человека;

TRUNCATE TABLE верблюды;

CREATE TABLE ослолошади (
	id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
	action VARCHAR(20),
	date_of_birth DATE,
	FOREIGN KEY (id) REFERENCES вьючные_животные(id)
);

INSERT INTO ослолошади (name, action, date_of_birth)
SELECT name, action, date_of_birth FROM лошади
UNION 
SELECT name, action, date_of_birth FROM ослы;
```

![task10](Images/task10.png)

11.Создать новую таблицу “молодые животные” в которую попадут все
животные старше 1 года, но младше 3 лет и в отдельном столбце с точностью
до месяца подсчитать возраст животных в новой таблице

```sql
USE друзья_человека;

DROP TABLE IF EXISTS молодые_животные;
CREATE TABLE молодые_животные (
	id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
	action VARCHAR(20),
	date_of_birth DATE,
	FOREIGN KEY (id) REFERENCES вьючные_животные(id)
);


SELECT name, action, date_of_birth, колво_месяцев
FROM (SELECT name, action, date_of_birth, TIMESTAMPDIFF(MONTH, date_of_birth, CURDATE()) AS колво_месяцев FROM собакии
UNION 
SELECT name, action, date_of_birth, TIMESTAMPDIFF(MONTH, date_of_birth, CURDATE()) AS колво_месяцев FROM хомяки
UNION
SELECT name, action, date_of_birth, TIMESTAMPDIFF(MONTH, date_of_birth, CURDATE()) AS колво_месяцев FROM ослолошади
UNION
SELECT name, action, date_of_birth, TIMESTAMPDIFF(MONTH, date_of_birth, CURDATE()) AS колво_месяцев FROM кошки) все_животные 
WHERE колво_месяцев BETWEEN 12 and 36;
```

![task11](/Images/task11.png)

12. Объединить все таблицы в одну, при этом сохраняя поля, указывающие на
прошлую принадлежность к старым таблицам.


```sql
USE друзья_человека;

DROP TABLE IF EXISTS все_животные;
CREATE TABLE все_животные(id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
	type VARCHAR(20),
    name VARCHAR(20),
	action VARCHAR(20),
	date_of_birth DATE,
	FOREIGN KEY (id) REFERENCES вьючные_животные(id));

INSERT INTO все_животные (type, name, action, date_of_birth)
SELECT 'Собаки', name, action, date_of_birth FROM собакии
UNION ALL
SELECT 'Хомяки', name, action, date_of_birth FROM хомяки
UNION ALL
SELECT 'Ослолошади',name, action, date_of_birth FROM ослолошади
UNION ALL
SELECT 'Кошки',name, action, date_of_birth FROM кошки;

SELECT * FROM все_животные;
```
![task12](/Images/task12.png)

Задания JAVA OOP [тут](https://github.com/Ksenaty1/Final-2.0/tree/main/Java)
