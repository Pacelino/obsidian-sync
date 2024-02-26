Есть модель:
```python
class SalesOrder(models.Model):
	amount = models.IntegerField()
	description = models.CharField(max_length=155)
	user = models.ForeignKey(User, on_delete=models.CASCADE, null=True)
	product = models.ManyToManyField(Product)
```
Для обращения к ней :
1) для начала запустим shell связанный с бд `./manage.py shell`
2) в shell импортируем модель 
```python
from orders.models import SalesOrder
```
3) Далее прописываем для доступам ко все данным бд.
```python
order_list = SalesOrder.objects.all()
SalesOrder.objects.create(amount="100",description="mouse", ...)
```
В Django, `objects` - это менеджер запросов (query manager), который связан с моделью базы данных. 
Менеджер запросов предоставляет интерфейс для выполнения запросов к базе данных, связанных с соответствующей моделью. Он предоставляет различные методы для выполнения запросов, таких как `all()`, `filter()`, `get()`, `create()` и другие, которые позволяют выполнять разнообразные операции с данными в базе данных.