jQuery Kladr
================================================================================

Плагин для автодополнения полей адреса при вводе.<br>
В качестве источника данных используется сервис [kladr-api.ru] [1].

Архитектура
--------------------------------------------------------------------------------

* **$.kladr** - ( *статичный объект* ) предоставляет методы и свойства для работы 
с сервисом
* **$('selector').kladr** - ( *плагин jQuery* ) плагин для автодополнения адреса

Подключение

`````javascript
$('input').kladr({
    option1: 'value1',
    option2: 'value2'
});
`````

Чтение опции, свойства

`````javascript
$('input').kladr('option1');
`````

Изменение опции, свойства

`````javascript
$('input').kladr('option1', 123);
`````

Свойства объекта $.kladr
--------------------------------------------------------------------------------

* **url** - url сервиса, по-умолчанию [http://kladr-api.ru/api.php] [2].
* **type** - перечисление используемых типов объектов. Список значений: *region*, 
*district*, *city*, *street*, *building*.
* **validate** *= function (query) {}* - выполняет проверку коррекстности объекта запроса.
* **api** *= function(query, callback) {}* - непосредственно выполняет запрос к сервису.
В качестве параметров принимает объект запроса и функцию, которой
будет передан ответ сервиса.
* **check** *= function(query, callback) {}* - проверяет существование объекта.
В качестве параметров принимает объект запроса и функцию, которой
будет передан объект (если он существует) либо *false*

Свойства объекта $.kladr дополнительно устанавливаемые плагином
--------------------------------------------------------------------------------

* **setDefault** *= function (param1, param2) {}* - устанавливает значения по-умолчанию
для параметров плагина. В качестве параметров принимает аналогично плагину либо объект
со списком изменяемых параметров, либо пару "параметр - новое значение параметра".
* **getDefault** *= function (param) {}* - возвращает значение по-умолчанию для параметра плагина.
В качестве параметра принимает название параметра плагина.
* **getInputs** *= function (selector) {}* - возвращает jQuery коллекцию полей ввода, к которым был
подключён плагин. В качетве параметра принимает селектор, DOM элемент или же объект jQuery, в котором
будет выполнен поиск соответствующих полей, по-умолчанию *body*.
* **buildAddress** *= function (objs) {}* - строит строку адреса на основании массива объектов КЛАДР.
* **getAddress** *= function (selector, build) {}* - возвращает строку адреса на основании полей ввода,
к которым был подключён плагин. Первый параметр аналогичен параметру функции *getInputs*. В качестве второго
параметра принимает функцию, которой будет собираться строка адреса, по-умолчанию *buildAddress*.
* **getTypes** *= function () {}* - служебная функция, возвращает список типов объектов КЛАДР.

Параметры плагина
--------------------------------------------------------------------------------

* **token** - токен для доступа к сервису, по-умолчанию *null*
* **key** - ключ для доступа к сервису, по-умолчанию *null*
* **type** - тип подставляемых объектов, по-умолчанию *null*
* **parentType** - тип родительского объекта, по-умолчанию *null*
* **parentId** - идентификатор родительского объекта, по-умолчанию *null*
* **limit** - количество отображаемых в выпадающем списке объектов, по-умолчанию *10*
* **oneString** - включить ввод адреса одной строкой, по-умолчанию *false*
* **withParents** - получить объект вместе с родителями, по-умолчанию *false*
* **parentInput** - селектор, DOM элемент или же объект jQuery, в котором
находится поле ввода родительского объекта, по-умолчанию *null*
* **verify** - проверять введённые данные, по-умолчанию *false*
* **spinner** - отображать ajax-крутилку, по-умолчанию *true*
* **current** - текущий, выбранный объект КЛАДР, *только для чтения*

Методы плагина
--------------------------------------------------------------------------------

* **source** *= function( query ) { return objects; }* - функция для получения 
списка объектов отображаемых при автодополнении. В качестве параметра принимает
объекта запроса. По-умолчанию запрашивает данные у сервиса [kladr-api.ru] [1].
Может быть переопределена для получения объектов из другого источника.
* **labelFormat** *= function( obj, query) { return label; }* - функция для 
форматирования значений в списке. В качестве параметров принимает *obj* – объект 
КЛАДР, *query* – объект запроса.
* **valueFormat** *= function( obj, query) { return label; }* – функция для 
форматирования подставляемых в поле ввода значений. В качестве параметров 
принимает *obj* – объект КЛАДР, *query* – объект запроса.
* **showSpinner** *= function ($spinner) {}* - функция выводящая ajax-крутилку.
В качестве параметра принимает jQuery объект ajax-крутилки.
* **hideSpinner** *= function ($spinner) {}* - функция скрывающая ajax-крутилку.
В качестве параметра принимает jQuery объект ajax-крутилки.

События плагина
--------------------------------------------------------------------------------

Во все события в качестве контекста (this) передаётся текущее поле ввода.

В обработчиках событий **...Before** (*openBefore*, *closeBefore*) объявленных как параметр плагина
( *$('').kladr({openBefore: function () {}})* ) можно отменить действие плагина, если в функции
вернуть **false** ( *$('').kladr({openBefore: function () { return false; }})* ).

* **openBefore** *= function() {}* - возникает перед открытием списка объектов. Доступно как событие
*kladr_open_before* поля ввода.
* **open** *= function() {}* - открыт список объектов. Доступно как событие *kladr_open*
поля ввода.
* **closeBefore** *= function() {}* - возникает перед закрытием списка объектов. Доступно как событие
*kladr_close_before* поля ввода.
* **close** *= function() {}* - закрыт список объектов. Доступно как событие *kladr_close*
поля ввода.
* **sendBefore** *= function(query) {}* - возникает перед отправкой запроса к сервису.
В параметре передаётся объект запроса. Доступно как событие *kladr_send_before* поля ввода.
* **send** *= function() {}* - отправлен запрос к сервису. Доступно как событие *kladr_send*
поля ввода.
* **received** *= function() {}* - получен ответ от сервиса. Доступно как событие *kladr_received*
поля ввода.
* **selectBefore** *= function() {}* - возникает перед изменением текущего объекта. Доступно как событие
*kladr_select_before* поля ввода.
* **select** *= function(obj) {}*  - выбран объект в списке. В параметре передаётся
текущий, выбранный объект КЛАДР. Доступно как событие *kladr_select* поля ввода.
* **checkBefore** *= function() {}*  - возникает перед проверкой введённых пользователем данных.
Вызывается только если параметр *verify* = *true*. Доступно как событие *kladr_check_before* поля ввода.
* **check** *= function(obj) {}* - проверен введённый пользователем объект.
Вызывается только если параметр *verify* = *true*. В параметре передаётся текущий объект КЛАДР.
Доступно как событие *kladr_check* поля ввода.

Структура папок, файлов
--------------------------------------------------------------------------------

* **jquery.kladr.js** - Плагин
* **jquery.kladr.min.js** - Минимифицированный код плагина
* **jquery.kladr.css** - Стили
* **jquery.kladr.min.css** - Минимифицированные стили
* **jquery.kladr.images** - Изображения плагина
* **kladr** - Состовляющие плагина (ядро и плагин для автодополнения) по отдельности
* **examples** - Примеры использования

Примеры
--------------------------------------------------------------------------------

Автодополнение для ввода адреса одной строкой

`````javascript
$('input').kladr({
	oneString: true
});
`````

Автодополнение городами России

`````javascript
$('input').kladr({
    type: $.kladr.type.city
});
`````

Изменении подписи при выборе района

`````javascript
$('input').kladr({
    type: $.kladr.type.district,
    select: function(obj){
        $('label').text(obj.type);
    }
});
`````

Более подробрые примеры можно найти в папке *examples*

Лицензия
--------------------------------------------------------------------------------
Решение распространяется под лицензией «Общественное достояние» (Public Domain)
и может быть свободно используемо любым лицом без выплат авторских вознаграждений.

[1]: http://kladr-api.ru/        "КЛАДР API"
[2]: https://kladr-api.ru/api.php        "API"
