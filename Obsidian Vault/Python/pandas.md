```
pip install pandas
```

>Применяется для обработки табличных данных

В pandas есть два класса объектов с которыми он работает.
- `Series` — одномерный массив, который может хранить значения любого типа данных;
- `DataFrame` — двумерный массив (таблица), в котором столбцами являются объекты класса `Series`.

### Series
Создать объект класса `Series` можно следующим образом:

```python
s = pd.Series(data, index=index)
```

В качестве `data` могут выступать: массив `numpy`, словарь, число. В аргумент `index` передаётся список меток осей. Метка может быть числом, но чаще используются метки-строки.

Если `data` это массив `numpy`, то `index` должен иметь такую же длину что и массив `nupmy`. Если аргумент `index` не передан, то `index` - список `[0, ..., len(data) - 1]`  (как обычный список). 

```python
s = pd.Series(np.arange(5), index=["a", "b", "c", "d", "e"])
print(s)
print()
s = pd.Series(np.linspace(0, 1, 5))
print(s)
```

Вывод:
```
a    0
b    1
c    2
d    3
e    4
dtype: int32

0    0.00
1    0.25
2    0.50
3    0.75
4    1.00
dtype: float64
```

Видно, что `Series` сильно похож на словарь, где ключи являются элементы из `index` .
Если `data` - словарь и `index` не передается, то `index` будет состоять из ключей словаря. Когда длинна `index` больше длинны словаря переданного в `data`, то в пустые места подставиться ``NaN``. 

```python
d = {"a": 10, "b": 20, "c": 30, "g": 40}
print(pd.Series(d))
print()
print(pd.Series(d, index=["a", "b", "c", "d"]))
```
```
a    10
b    20
c    30
g    40
dtype: int64

a    10.0
b    20.0
c    30.0
d     NaN
dtype: float64
```

Если в `data` передается число, то обязательно нужно передать `index`.
```python
index = ["a", "b", "c"]
print(pd.Series(5, index=index))
```

Вывод программы:

```
a    5
b    5
c    5
dtype: int64
```

Для `Series` доступно взятие элемента по индексу, срезы, поэлементные математические операции аналогично массивам `numpy`.
```python
s = pd.Series(np.arange(5), index=["a", "b", "c", "d", "e"])
print("Выбор одного элемента")
print(s["a"])
print("Выбор нескольких элементов")
print(s[["a", "d"]])
print("Срез")
print(s[1:])
print("Поэлементное сложение")
print(s + s)
```

```
Выбор одного элемента
0
Выбор нескольких элементов
a    0
d    3
dtype: int32
Срез
b    1
c    2
d    3
e    4
dtype: int32
Поэлементное сложение
a    0
b    2
c    4
d    6
e    8
dtype: int32
```

Для `Series` можно применять фильтрацию данных по условию, записанному в качестве индекса:

```python
s = pd.Series(np.arange(5), index=["a", "b", "c", "d", "e"])
print("Фильтрация")
print(s[s > 2])
```

Вывод программы:

```
Фильтрация
d    3
e    4
dtype: int32
```

У `Series` есть атрибут `name` со значением имени набора данных, а также `index.name` со значением имени индексов. 

```python
s = pd.Series(np.arange(5), index=["a", "b", "c", "d", "e"])
s.name = "Данные"
s.index.name = "Индекс"
print(s)
```

Вывод программы:
```
Индекс
a    0
b    1
c    2
d    3
e    4
Name: Данные, dtype: int32
```

### DataFrame

>Работает с двумерными данными. 

```python
students_marks_dict = {"student": ["Студент_1", "Студент_2", "Студент_3"],
                       "math": [5, 3, 4],
                       "physics": [4, 5, 5]}
students = pd.DataFrame(students_marks_dict)
print(students)
```

Вывод программы:

```
     student  math  physics
0  Студент_1     5        4
1  Студент_2     3        5
2  Студент_3     4        5
```

У `DataFrame` есть обозначения:
- строки - `index` 
- столбцы - `columns`
```python
print(students.index)
print(students.columns)
```

Вывод программы:
```
RangeIndex(start=0, stop=3, step=1)
Index(['student', 'math', 'physics'], dtype='object')
```

По умолчанию в `index` хранится список чисел, но их можно изменить просто передав другой список. 
```python
students.index = ["A", "B", "C"]
print(students)
```

Вывод программы:
```
     student  math  physics
A  Студент_1     5        4
B  Студент_2     3        5
C  Студент_3     4        5
```


- Для доступа к элементам по строке (или индексу) - атрибут `loc`. При использовании строковой метки доступна операция среза:
```python
print(students.loc["B":])
```

Вывод программы:
```
     student  math  physics
B  Студент_2     3        5
C  Студент_3     4        5
```

- Для того чтобы получить значения отдельной строки воспользуемся `loc`  для получения отдельно строки по индексу и сверху накинем `values`.
`DataFrame`
```
       name  maths  physics  computer science
0    Иванов      5        4                 5
1    Петров      4        4                 2
```

```python
data.loc[0].values
```
Вывод:
```
['Иванов' 5 4 5]
```

Обычно табличные данные хранятся в файлах. Такие наборы данных принято называть дата-сетами. Файлы с дата-сетом могут иметь различный формат. `Pandas` поддерживает операции чтения и записи для CSV, Excel 2007+, SQL, HTML, JSON, буфер обмена и др.

Примеры для получения дата-сетов из файлов:
- [[CSV]]. Используется функция `read_csv()`. Аргумент `file` является строкой, в которой записан путь до файла с дата-сетом. Для записи данных из `DataFrame` в CSV-файл используется метод `to_csv(file)`.

- Excel. Используется функция `read_excel()`. Для записи данных из `DataFrame` в Excel-файл используется метод `to_excel()`.

- JSON. Используется функция `read_json()`. Для записи данных из `DataFrame` в JSON используется метод `to_json()`.

Команды для обработки дата-сетов:

- Для получения первых n строк дата-сета используется метод `head(n)`
```python
print(students.head())
```

Вывод:
```
   gender race/ethnicity  ... reading score writing score
0  female        group B  ...            72            74
1  female        group C  ...            90            88
2  female        group B  ...            95            93
3    male        group A  ...            57            44
4    male        group C  ...            78            75

[5 rows x 8 columns]
```

- Для получения последних строк - `tail(n)`.
```python
print(students.tail(3))
```

Вывод программы:

```
     gender race/ethnicity  ... reading score writing score
997  female        group C  ...            71            65
998  female        group D  ...            78            77
999  female        group D  ...            86            86

[3 rows x 8 columns]
```

- Получение части дата-сета используем срез:
```python
print(students[10:13])
```

```
    gender race/ethnicity  ... reading score writing score
10    male        group C  ...            54            52
11    male        group D  ...            52            43
12  female        group B  ...            81            73

[3 rows x 8 columns]
```

- Чтобы отфильтровать данные используем условия для фильтрации:

Выберем пять первых результатов теста по математике для студентов, прошедших подготовительный курс.
```python
print(students[students["test preparation course"] == "completed"]["math score"].head())
```
Вывод программы:
```
1     69
6     88
8     64
13    78
18    46
Name: math score, dtype: int64
```

- Сортировка осуществляется с помощью `sort_values()`. Сортировка по умолчанию производится в порядке возрастания значений. Для сортировки по убыванию в именованный аргумент `ascending` передаётся значение `False`.
```python
with_course = students[students["test preparation course"] == "completed"]
print(with_course[["math score",
                   "reading score",
                   "writing score"]].sort_values(["math score",
                                                  "reading score",
                                                  "writing score"], ascending=False).head())
```

Вывод программы:
```
     math score  reading score  writing score
916         100            100            100
149         100            100             93
625         100             97             99
623         100             96             86
114          99            100            100
```
При сортировке сравнивались последовательно значения в перечисленных столбцах.

- Для фильтрации от нескольких значений одновременно можно использовать условные выражения и оператор логического И (`&`):
```python
data = {'Имя': ['Анна', 'Борис', 'Виктор', 'Дмитрий'],
        'Возраст': [25, 30, 35, 28],
        'Зарплата': [50000, 60000, 70000, 55000]}

df = pd.DataFrame(data)

# Определяем условия фильтрации
condition1 = (df['Возраст'] > 25)
condition2 = (df['Зарплата'] >= 60000)

# Применяем фильтр (оператор логического И)
filtered_df = df[condition1 & condition2]

# Выводим результат
print(filtered_df)
```
Вывод программы:
```
Отфильтрованный DataFrame:
      Имя  Возраст  Зарплата
1   Борис       30     60000
2  Виктор       35     70000
```


- Сортировка по сумме
Для этого создадим ещё один столбец `total_score` и произведём по нему сортировку:

```python
with_course = students[students["test preparation course"] == "completed"]
students["total score"] = students["math score"] + students["reading score"] + students["writing score"]
print(students.sort_values(["total score"], ascending=False).head())
```

Вывод программы:

```
     gender race/ethnicity  ... writing score total score
916    male        group E  ...           100         300
458  female        group E  ...           100         300
962  female        group E  ...           100         300
114  female        group E  ...           100         299
179  female        group D  ...           100         297

[5 rows x 9 columns]
```

- Добавить в таблицу колонку, используем `assign()`.
```python
scores = students.assign(total_score=lambda x: x["math score"] + x["reading score"] + x["writing score"])
print(scores.sort_values(["total_score"], ascending=False).head())
```
Вывод программы:
```
     gender race/ethnicity  ... writing score total_score
916    male        group E  ...           100         300
458  female        group E  ...           100         300
962  female        group E  ...           100         300
114  female        group E  ...           100         299
179  female        group D  ...           100         297
```

- Групировка по признаку.
Используем метод `groupby()`. 

Получили информацию о студентах мужского и женского поля у которые прошли курс по подготовке к тестированию (test preparation course). И подсчитали их. 
```python
print(students.groupby(["gender", "test preparation course"])["writing score"].count())
```
Вывод программы:
```
gender  test preparation course
female  completed                  184
        none                       334
male    completed                  174
        none                       308
Name: race/ethnicity, dtype: int64
```

- Агрегация (операцию, которая позволяет получить сводные данные). 
`Как пример агрегации может выступать среднее арифметическое, минмум, максимум и др.`
Для применения функции агрегации к нескольким столбцам подойдёт метод `agg()`,  в который можно передать словарь с ключами — названиями столбцов, а значения по ключам могут быть списками функций агрегации.

Пример:
Определим среднее арифметическое и медианное значение баллов по за тестирование по математике для студентов мужского и женского пола в зависимости от прохождения подготовительного курса:

```python
agg_functions = {"math score": ["mean", "median"]}
print(students.groupby(["gender", "test preparation course"]).agg(agg_functions))
```

Вывод программы:
```
                               math score       
                                     mean median
gender test preparation course                  
female completed                67.195652   67.0
       none                     61.670659   62.0
male   completed                72.339080   73.0
       none                     66.688312   67.0
```