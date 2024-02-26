Он является [[Магические методы|магическим методом]], который позволяется делать кастомный вывод через `print()` объекта класса. 
Он срабатывает, когда используется `print` на объекте класса. 
```python
class ElectricCar(Car):

    def __init__(self, color, consumption, bat_capacity, mileage=0):
        super().__init__(color, consumption, bat_capacity, mileage)
        self.bat_capacity = bat_capacity

def __str__(self):
    return f"Электромобиль. " \
           f"Цвет: {self.color}. " \
           f"Пробег: {self.mileage} км. " \
           f"Остаток заряда: {self.reserve} кВт*ч."

    def drive(self, distance):
        if not self.engine_on:
            return "Двигатель не запущен."
        if self.reserve / self.consumption * 100 < distance:
            return "Малый запас заряда."
        self.mileage += distance
        self.reserve -= distance / 100 * self.consumption
        return f"Проехали {distance} км. Остаток заряда: {self.reserve} кВт*ч."

    def recharge(self):
        self.reserve = self.bat_capacity
```
Проверим, как будет работать код:

```python
electric_car = ElectricCar(color="белый", consumption=15, bat_capacity=90)
print(electric_car.start_engine())
print(electric_car.drive(100))
print(electric_car)
```

Вывод программы:

```
Двигатель запущен.
Проехали 100 км. Остаток заряда: 75.0 кВт*ч.
Электромобиль. Цвет: белый. Пробег: 100 км. Остаток заряда: 75.0 кВт*ч.
```