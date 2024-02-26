Одним из самых популярных форматов хранения табличных данных является CSV (Comma Separated Values, значения с разделителем-запятой). 

В файлах этого формата данные хранятся в текстовом виде. Строки таблицы записываются в файле с новой строки, а столбцы разделяются определённым символом, чаще всего запятой ',' или точкой с запятой ';'. Первая строка, как правило, содержит заголовки столбцов таблицы. Пример части CSV-файла с информацией о результатах прохождения тестов студентами и некоторой дополнительной информацией:
```
"gender","race/ethnicity","parental level of education","lunch","test preparation course","math score","reading score","writing score"
"female","group B","bachelor's degree","standard","none","72","72","74"
"female","group C","some college","standard","completed","69","90","88"
```

Получим дата-сет из CSV-файла с данными о студентах:

```
import numpy as np
import pandas as pd

students = pd.read_csv("StudentsPerformance.csv")
```

Полученный объект `students` относится к классу `DataFrame`.

Для получения первых n строк дата-сета используется метод `head(n)`. По умолчанию возвращается пять первых строк:

```
print(students.head())
```

Вывод программы:
```
   gender race/ethnicity  ... reading score writing score
0  female        group B  ...            72            74
1  female        group C  ...            90            88
2  female        group B  ...            95            93
3    male        group A  ...            57            44
4    male        group C  ...            78            75

[5 rows x 8 columns]
```

Для получения последних строк - `tail(n)`.
```
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
```
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