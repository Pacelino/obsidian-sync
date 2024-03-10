### Команды
`django-admin startproject имя_проекта` - создает проект
`./manage.py runserver` - запускаем проект 
`./manage.py migrate  ` - создаст модели для работы с бд
`./manage.py createsuperuser` - создаст суперюзера
`./manage.py startapp orders` - создание приложения
`./manage.py shell` - попасть в консоль связанная с приложением
`класс_модели.objects.all()` - в `shell` возвращает все объекты моделей (все переменные из бд).
http://127.0.0.1:8000/admin/login/?next=/admin/ - панель администратора django.
### Файлы
`manage.py `- управление самим приложением
`settings.py` - хранятся переменные django, конфиг django 
`urls.py `- урлы по которым будет все доступно 
`admin.py` - нужен для отображения моделей в панели администратора
`apps.py` - конфигурация приложения
`models.py` - тут создаем модели(базы данных)
`tests.py` - тут пишу unit тесты для приложения
`views.py` - пишем представление, логика веба
### Приложения 
по факту это разделение одной большой задачи на несколько маленьких задач (приложений). После создания приложения появляется папка с файлами для работы с приложением. Для успешной работы добавляем наше созданное приложение в список `INSTALED_APPS` в `settings.py`

### Модели
Модель это реализация таблиц базы данных через работу с классами в файле `models.py`. Имя класса - название таблицы, переменные - столбцы таблицы с различными типами. 
```python
class SalesOrder(models.Model):

	amount = models.IntegerField()
	description = models.CharField(max_length = 155)
```
для создания нужно провести миграцию `./manage.py makemigrations`. В папке `migrations` хранятся все миграции. Для того чтобы залить их в базу `./manage.py migrate`.

Для отображения в админке нужно объявить ее в `admin.py`
```python
from .models import SalesOrder
# Register your models here.

admin.site.register(SalesOrder)
```

Для того чтобы пощупать данные из модели запускаем `./manage.py shell` и импортируем в нее наш класс`from orders.models import SalesOrder`. Для получения всех значений `SalesOrder.objects.all()`. Также можно фильтровать значения используя `SalesOrder.objects.filter(amount > 10)`


### Связи между моделями 
**ForeignKey** - многие к одному 
**Пример:**
```python
class Reporter(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    email = models.EmailField()

class Article(models.Model):
    headline = models.CharField(max_length=100)
    pub_date = models.DateField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)
```
Статья может иметь только одного репортера. А репортер может иметь бесконечное количество статей, поэтому выгодней отметить в классе статьей отметить, кто репортер статьи. 

**ManyToMany** - многие ко многим
**Пример:**
```python
class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    toppings = models.ManyToManyField(Topping)
```

В пицце может быть много допингов, так и допинги могут находиться во многих пиццах. 

**OneToOne** = один ко одному
**Пример:**
```python
class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):
        return f"{self.name} the place"

class Restaurant(models.Model):
    place = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
    )
```


###  HTML rendering View
Для вывода данных приложения нужно зайти в `views.py` и написать функцию.
Простой пример:
```python
def orders_page(request):
	return render(request, 'index.html', 
	{'orders': SalesOrder.objects.all()})
```
в функцию `render` передаем request и файл где написана сама отрисовка. Все [[Фронт|html]] хранятса в папке templates созданной в проекте. 
Для того чтобы к моделям можно было обратиться в фронте передаем их через словарь. 


Для связи ссылки фронта и моделей создадим url. В папке проекта `urls`.
```python
from orders.views import orders_page

urlpatterns = [
path('admin/', admin.site.urls),
path('', orders_page),
]
```

Для импорта стилей страниц подключим bootstrap.

### API View на Django REST Framework
```
pip install djangorestframework
```

Вместо создания функции как в простом view, создается класс наследуемый от `viewsets.ModelViewSet` в этом классе обязательно должен быть `serializer` и `QuerySet`. 
**QuerySet** - это словарь который мы передаем на фронт (как в `render` передавал словарь с `orders`). 
[[serializer]] - формирует объект, который передается в шаблон фронта. Для того чтобы с ним работать нужно его создать `serializers.py` в папки приложения. 
Объявляем сериализатор в созданном классе `OrderView`. 
```python
class OrderView(viewsets.ModelViewSet):
	queryset = SalesOrder.objects.all()
	serializer_class = OrderSerializer
```
Для работы пробрасываем в `urls.py`. Для этого нужно воспользоваться `router`.
```python
router = DefaultRouter()
```
Далее нужно записать view в router и путь по которому все будет доступно. 
```python
router.register('api/orders', OrderView)

urlpatterns = [
path("admin/", admin.site.urls),
path("", orders_page),
path("app/", orders_app),
]

urlpatterns += router.urls
```

Запускаем, переходим по `http://127.0.0.1:8000/api/orders/?format=json` 

### Фронтенд на vue.js

Для этого создаем новый файл в templates. И подключаем vue.js и axios для удобного взаимодействия с api. 
```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.8/dist/vue.js"></script>

<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

Создаем новую функцию во view.py. 

Создаем контейнер в теге `div`, где будем писать страницу.
```html
<div class="container" id ="orders_app"></div>
```
Оборачиваем div в `{% verbatim %}`, чтобы джанго не будет рендерить шаблон, а все будет рендорить через vue.js. 

**Создаем js файл**
Для этого сначала создаем папку static на уровне проекта. 
В файле setting.py прописываем, где хранятся статические файлы. 
```python
STATICFILES_DIRS = [

os.path.join(BASE_DIR, "static")

]
```
Подключаем в html js файл 
```html
<script src="static/app.js"></script>
```
