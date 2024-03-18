>Это язык программирования, работающий в браузере. Помогающий настроить [[Фронтенд технологии|фронтенд логику]] с [[HTML И CSS|html]].   


Так же как и в [[HTML И CSS|css]] можно его подключить через `sript`
```html
<script type="text/javascript">
            alert("Привет, мир!");
</script>
```
![[Pasted image 20240318161445.png]]
Для отображение этого alert после загрузки страницы обернем его в функцию и потом в window.onload
```js
window.onload = function(){
	alert("Привет, мир!");
}
```

`window` - собирает информацию о текущем окне 

JavaScript позволяет эффективно работать с дом деревом (HTML кодом). Создавать новые блоки, скрывать, показывать, накладывать css и т.д. 

Сделаем тег `h1` другого цвета
```js
window.onload = function(){
  document.querySelector('h1').style.color = 'red';
}
```

`document` глобальный объект для работы с документом
`querySelector` метод, который позволяет достать из дом деревая первый нужный элемент. Например по `h1` или `p img.myphoto`. 
`querySelectorAll` тоже самое только достает все элементы.  
`style` можно накладывать все css стили для данного элемента. 

Сохранили этот код в переменную `h1`
```js
let h1 = document.querySelector('h1');
```
Навещиваем на переменную `h1` прослушивателя событий (выполняет функцию) по клику
```js
h1.addEventListener("click", function(){
                    h1.style.color = "red";
                });
```

Скрываем картинку по клику 
```js
h1.addEventListener("click", function(){
	document.querySelector("img").style.display = "none";
});
```