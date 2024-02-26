```
pip install matplotlib
```

Это инструмент для визуализации данных. Например, полученных от обработки через [[pandas]].
 ```
import matplotlib.pyplot as plt
```

Построим гистограмму, которая показывает распределение количества студентов по баллам за тест по математике:

```python
plt.hist(students["math score"], label="Тест по математике")
plt.xlabel("Баллы за тест")
plt.ylabel("Количество студентов")
plt.legend()
plt.show()
```