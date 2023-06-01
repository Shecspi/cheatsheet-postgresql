### Термины

**Индекс** - специальная структура данных, позволяющая решать задачу ускорения доступа к строкам в таблице, а также задачу предотвращения дублирования значений ключевых атрибутов в различных строках таблицы.

### CREATE TABLE - Создание таблиц
Упрощённый синтаксис  команды `CREATE` (полная инструкция доступна по команде `\h CREATE TABLE`):
```sql
CREATE TABLE <Имя таблицы>
(
	<Имя столбца> <Тип данных> [Ограничения целостности],
	<Имя столбца> <Тип данных> [Ограничения целостности],
	...
	[Ограничения целостности],
	[Первичный ключ],
	[Внешний ключ]
);
```
Например:
```sql
CREATE TABLE aircrafts
( 
	aircraft_code char(3) NOT NULL, 
	model text NOT NULL,
	range integer NOT NULL,
	CHECK ( range > 0 ),
	PRIMARY KEY ( aircraft_code )
);
```

После выполнения этой команды создаётся следующая таблица (проверить можно командой `\d <Имя таблицы`)
```
Столбец       | Тип          | Допустимость NULL
------------- | ------------ | ------------------
aircraft_code | character(3) | not null
model         | text         | not null
range         | integer      | not null

Индексы:  
   "aircraft_pkey" PRIMARY KEY, btree (aircraft_code)  
Ограничения-проверки:  
   "aircraft_range_check" CHECK (range > 0)
```
Для реализации первичного ключа `PRIMARY KEY` всегда создаётся автоматически индекс с типом btree (B-дерево).

### DROP TABLE - Удаление таблицы

Синтаксис:
```sql
DROP TABLE [IF EXISTS] <Имя таблицы>
```

### INSERT - Добавление данных в таблицу

Синтаксис:
```sql
INSERT INTO <Имя таблицы>
(
	<Имя столбца>,
	<Имя стобца>,
	...
)
VALUES
(
	<Значение атрибута>,
	<Значение атрибута>,
	...
);
```

Пример:
```sql
INSERT INTO aircrafts ( aircraft_code, model, range )
VALUES ( 'SU9', 'Sukhoi SuperJet-100', 3000 );

```

### SELECT - Выборка из таблицы

Синтаксис:
```sql
SELECT <Имя столбца>, <Имя столбца>, ...
  FROM <Имя таблицы>        -- Таблица, откуда осуществляется выборка
  [WHERE <Условие>]         -- Условие для выборки
  [ORDER BY <Имя столбца>]  -- Сортировка
```

Пример:
```sql
SELECT * FROM aircrafts ORDER BY model;
```
Вывод:
```
aircraft_code |        model        | range    
---------------+---------------------+-------  
SU9           | Sukhoi SuperJet-100 |  3000  
773           | Boeing 777-300      | 11000  
321           | Airbus A321-200     |  5600  
CN1           | Cessna 208 Caravan  |  1200  
(4 строки)
```

### UPDATE - Обновление данных в таблице

Синтаксис:
```sql
UPDATE <Имя таблицы>
  SET <Имя столбца> = <Значение столбца>
      <Имя столбца> = <Значение столбца>
  WHERE <Устолвие>
```

Пример:
```sql
UPDATE aircrafts SET range = 3500 WHERE aircraft_code = 'SU9';
```

### DELETE - Удаление строки из таблицы

Синтаксис:
```sql
DELETE
  FROM <Имя таблицы>
  WHERE <Условие>
```

Пример:
```sql
DELETE FROM aircrafts WHERE aircraft_code = 'CN1';
```

Для удаления всех строк из таблицы нужно убрать условие `WHERE`:
```sql
DELETE FROM <Имя таблицы>
```