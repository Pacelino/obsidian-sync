Подробнее про [[HTML И CSS]] 
Тут про [[Django]]
```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title></title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">

</head>

<body>
{% for order in orders%}
<div class="btn btn-success btn-lg">
amount is {{order.amount}}
<br>
description: {{order.description}}
<div>
<br><br>
{% endfor %}
</body>
</html>
```
### Шаблонизаторы
Позволяют избавиться от повторяющегося кода. Всякие боковые меню и т.д. 

Создаем блок для других html файлов `{% block content %} {% endblock %}`. Код, находясь вне блока будет базовым для других файлов, которые его используют. 
```html
<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<title></title>
</head>
<body>
{% block content %}
{% endblock %}
</body>
</html>
```
Для подключения этого блока вначале html пишем `{%extends "base.html"%}` и оборачиваем вся в `{%block example%}`.