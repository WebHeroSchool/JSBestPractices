﻿# JavaScript Best Practices

---
## Избегайте глобальных переменных
> Глобальные переменные и имена функций - невероятно плохая идея. Причина в том, что каждый файл JavaScript, включенный на страницу, работает в одной и той же области. Если в вашем коде есть глобальные переменные или функции, включенные после вашего скрипты, содержащие те же имена переменных и функций, перезапишут ваши переменные / функции. Есть несколько способов для предотвращения использования глобальных переменных.  
> **Использование модульного шаблона**
```
myNameSpace = function () {
  var current = null;
  function init () {...}
  function change () {...}
  function verify () {...}
} ();
```

> **Шаблон раскрытия модуля**
```
myNameSpace = function(){
  var current = null;
  function init(){...}
  function change(){...}
  function verify(){...}
  return{
    init:init,
    change:change
  }
}();
```

> **Если вам не нужно, чтобы какие-либо переменные или функции были доступны извне, просто заключите всю конструкцию в другой набор круглых скобок, чтобы выполнить ее, не присваивая ей никакого имени**
```
myNameSpace = function(){
  var current = null;
  function init(){...}
  function change(){...}
  function verify(){...}
  return{
    init:init,
    set:change
  }
}();
```

## Комментируйте столько, сколько нужно, но не более
> Комментарии - это ваши сообщения другим разработчикам (и самому себе, если вы вернетесь к своему коду после нескольких месяцев работы над чем-то другим). Комментируйте, когда нужно сказать что-то важное, а если вы это делаете, используйте `/* */` обозначения. 
```
module = function () {
  var current = null;
  function init () {
  };
/ *
  function show () {
    ток = 1;
  };
  function hide () {
    Показать();
  };
* /
  return {init: init, show: show, current: current}
} ();
```

## Используйте сокращенные обозначения, когда это имеет смысл
> Cокращенные обозначения - сложный вопрос: с одной стороны, они делают ваш код компактным, но с другой стороны, вы можете усложнить задачу другим разработчикам, читающим ваш код. Объекты - это, наверное, самая универсальная вещь, которая есть в JavaScript. Cпособ их написания выглядит примерно так:
```
var cow = new Object();
cow.colour = 'brown';
cow.commonQuestion = 'What now?';
cow.moo = function(){
  console.log('moo');
}
cow.feet = 4;
cow.accordingToLarson = 'will take over the world';
```
> Однако это означает, что вам нужно повторять имя объекта для каждого свойства или метода. Вместо этого имеет смысл написать следующую конструкцию, также называемую литералом объекта:
```
var cow = {
  colour:'brown',
  commonQuestion:'What now?',
  moo:function(){
    console.log('moo);
  },
  feet:4,
  accordingToLarson:'will take over the world'
};
```

## Модульность - одна функция на задачу
> Это общая передовая практика программирования - обеспечение того, чтобы вы создавали функции, выполняющие одну работу за раз, упрощает другим разработчикам отладку и изменение вашего кода без необходимости сканировать весь код, чтобы определить, какой блок кода выполняет какую функцию.
```
function createLink(text,url){
  var newLink = document.createElement('a');
  newLink.setAttribute('href',url);
  newLink.appendChild(document.createTextNode(text));
  return newLink;
}

function createMenu(){
  var menu = document.getElementById('menu');
  var items = [
    {t:'Home',u:'index.html'},
    {t:'Sales',u:'sales.html'},
    {t:'Contact',u:'contact.html'}
  ];
  for(var i=0;i<items.length;i++){
    var item = createLink(items.t,items.u);
    item.className = 'menu-item';
    menu.appendChild(item);
  }
}
```

## Оптимизация циклов
> Код может стать очень медленными, если вы не сделаете его правильно. Одна из наиболее частых ошибок - чтение атрибута длины массива на каждой итерации:
```
var names = ['George','Ringo','Paul','John'];
for(var i=0;i<names.length;i++){
  doSomeThingWith(names[i]);
}
```
> Это означает, что каждый раз при запуске цикла JavaScript должен считывать длину массива. Вы можете избежать этого, сохранив значение длины в другой переменной:
```
var names = ['George','Ringo','Paul','John'];
var all = names.length;
for(var i=0;i<all;i++){
  doSomeThingWith(names[i]);
}
```

## Сведите доступ к DOM до минимума
> Доступ к DOM в браузерах - занятие дорогое. DOM - это очень сложный API, и рендеринг в браузерах может занять много времени. Вы можете увидеть это при запуске сложных веб-приложений, когда ваш компьютер уже загружен другой работой - изменения занимают больше времени или отображаются на полпути и так далее.Чтобы убедиться, что ваш код работает быстро и не останавливает браузер, постарайтесь свести доступ к DOM до минимума. Вместо того, чтобы постоянно создавать и применять элементы, используйте инструментальную функцию, которая превращает строку в элементы DOM и вызывает эту функцию в конце процесса генерации, чтобы нарушать визуализацию браузера один раз, а не постоянно.

## Проверяйте типы данных
> Убедитесь, что все данные, поступающие в ваши системы, чистые и именно то, что вам нужно. Это наиболее важно при записи параметров, полученных из URL-адреса. В JavaScript очень важно проверить тип параметров, отправляемых вашим функциям (используя `typeof` ключевое слово).
```
function buildMemberList(members){
  if(typeof members === 'object' && 
     typeof members.slice === 'function'){
    var all = members.length;
    var ul = document.createElement('ul');
    for(var i=0;i<all;i++){
      var li = document.createElement('li');
      li.appendChild(document.createTextNode(members[i].name));
      ul.appendChild(li);
    }
    return ul;
  }
}
```

## Добавляйте функциональность с помощью JavaScript, не создавайте слишком много контента
> Создание большого количества HTML в JavaScript может быть довольно сложным и нестабильным. В частности, в Internet Explorer вы можете столкнуться со всевозможными проблемами, изменяя документ, пока он все еще загружается, и манипулируя с помощью `innerHTML`.

## Используйте значения параметров по умолчанию
> Если функция вызывается с отсутствующим аргументом, значение отсутствующего аргумента устанавливается равным `undefined`. Неопределенные значения могут нарушить ваш код. Присваивать аргументам значения по умолчанию - хорошая привычка.
```
function myFunction(x, y) {
  if (y === undefined) {
    y = 0;
  }
}
```

## Избегайте использования `eval()`
> Функция `eval()` используется для запуска текст в качестве кода. Практически во всех случаях в его использовании нет необходимости. Поскольку он позволяет запускать произвольный код, он также представляет проблему безопасности.



