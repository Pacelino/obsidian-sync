В это [[Собственные исключения|примере]] используется конструкция `__name__ == "__main__"` она проверяет запущен ли код напрямую или был импортирован и запущен через другой файл (импортирован как модуль). В Python любая программа может быть импортирована как модуль. 

В идеологии Python импортировать модуль — значит полностью его выполнить. Если основной код модуля содержит вызовы функций, ввод или вывод данных без использования указанного условия `__name__ == "__main__"`, то произойдёт полноценный запуск программы. А это не всегда удобно, если из модуля нужна только отдельная функция или какой-либо класс.

Для импорта модуля из файла, например `example_module.py`, нужно указать его имя, если он находится в той же папке, что и импортирующая его программа:

```python
import example_module
```

Если требуется отдельный компонент модуля, например функция или класс, то импорт можно осуществить так:

```python
from example_module import some_function, ExampleClass
```

Для примера сделаю программу приветствия:
Код программы `module_hello.py`:

```python
def hello(name):
    return f"Привет, {name}!"


print(hello(input("Введите своё имя: ")))
```

Код программы `program.py`:

```python
from module_hello import hello

print(hello(input("Добрый день. Введите имя: ")))
```

При выполнении `program.py` нас ожидает неожиданное действие. Программа сначала запросит имя пользователя, а затем сделает это ещё раз, но с приветствием из `program.py`.

```
Введите своё имя: Андрей
Привет, Андрей!
Добрый день. Введите имя: Андрей
Привет, Андрей!
```
Ошибка в том что программа `module_hello` выполняется полностью, включая код с выводом результата(который находится вне функции). 
Чтобы это исправить добавляем проверку в `module_hello`
```python
if __name__ == "__main__":
    print(hello(input("Введите своё имя: ")))
```
Теперь при импортировании модуля `module_hello` код вывода не будет исполняться (код, который находится под `if __name__=="__main__"`).

Для большего удобства обычно в теле указанного условного оператора вызывают функцию `main()`, а основной код программы оформляют уже внутри этой функции.  
Тогда наш модуль можно переписать так:

```python
def hello(name):
    return f"Привет, {name}!"

def main():
    print(hello(input("Введите своё имя: ")))

if __name__ == "__main__":
    main()
```
