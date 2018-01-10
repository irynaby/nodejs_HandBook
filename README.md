# Руководство по NodeJS

**NodeJS** - это среда выполнения JS, которая построена на основе движка JavaScript Chrome V8, который позволяет транслировать вызовы на языке JavaScript в машинный код. 

### Установка
[скачать](https://nodejs.org/en/)

обновить установленную версию до самой последней:
```javascript
npm install npm@latest -g
```
установка express:
```javascript
npm install express
```
 _Express_ - это легковесный веб-фреймворк для упрощения работы с Node.js.
 
После установки NodeJS нам становится доступным такой инструмент как REPL. REPL (Read Event Printed Loop) представляет возможность запуска выражений на языке JavaScript в командной строке или терминале.

Так, запустим командную строку (на Windows) или терминал (на OS X или Linux) и введем команду **node**. После ввода этой команды мы можем выполнять различные выражения на JavaScript:
![alt text](http://blog.codeship.io/wp-content/uploads/2014/05/nodejs-guide-1.jpg "")

### Module
Node.js использует **модульную систему**. _Модуль_ - блок кода, который может использоваться повторно в других модулях.

Встроенные модули и функциональности в nodejs [тут](https://nodejs.org/api/).

Для загрузки модулей применяется функция _require()_ .
```javascript
var http = require("http");
```

Подключаемые модули кэшируются.

Определим файл простейшего сервера. Для этого в корневую папку проекта добавим новый файл app.js:
 ```javascript
 // получаем модуль Express
var express = require("express");
// создаем приложение
var app = express();
 
// устанавливаем обработчик для маршрута "/"
app.get("/", function(request, response){
 
    response.end("Hello from Express!");
});
// начинаем прослушивание подключений на 3000 порту
app.listen(3000);
 ```
Первая строка получает установленный модуль express, а вторая создает объект приложения.

В _Express_ мы можем связать обработку запросов с определенными маршрутами. Например, "/" - представляет главную страницу или корневой маршрут. Для обработки запроса вызывается функция app.get(). Первый параметр функции - маршрут, а второй - функция, которая будет обрабатывать запрос по этому маршруту.
И чтобы сервер начал прослушивать подключения, надо вызвать метод app.listen(), в который передается номер порта.

Запустим сервер командой **node app.js**. И в адресной строке браузера введем адрес http://localhost:3000/.

### События
специальные объекты - **эмиттеры** для генерации различных событий, которые обрабатываются специальными функциями - обработчиками или слушателями событий. Все объекты, которые генерируют события, представляют экземпляры класса _EventEmitter_.

С помощью функции _eventEmitter.on()_ к определенному событию по имени цепляется функция обработчика. Причем для одного события можно указать множество обработчиков. Когда объект EventEmitter генерирует событие, происходит выполнение всех этих обработчиков.

Рассмотрим применение объекта EventEmitter и событий. Для этого определим следующий файл app.js:
  ```javascript
var Emitter = require("events");
var emitter = new Emitter();
var eventName = "greet";
emitter.on(eventName, function(data){
    console.log(data);
});
 
emitter.emit(eventName, "Привет пир!");
   ```
 Для генерации события и вызова связанных с ним обработчиков выполняется функция _emitter.emit()_, в которое передается название события.
 
 #### Наследование от EventEmitter
 ```javascript 
 var util = require("util");
var EventEmitter = require("events");
 
function User(){
}
util.inherits(User, EventEmitter);
 
var eventName = "greet";
User.prototype.sayHi = function(data){
    this.emit(eventName, data);
}
var user = new User();
// добавляем к объекту user обработку события "greet"
user.on(eventName, function(data){
    console.log(data);
});
 
user.sayHi("Мне нужна твоя одежда...");
```
Здесь определена функция конструктора User, которая представляет пользователя. Для прототипа User определяется метод sayHi, в котором генерируется событие "greet".

Но чтобы связать объект _User_ с _EventEmitter_, надо вызвать функцию _util.inherits(User, EventEmitter);_. Благодаря этому мы можем через метод on() добавить к событию объекта user какой-нибудь обработчик, который будет вызван при выполнении метода _user.sayHi()_.

### Stream
Stream представляет поток данных. Потоки бывают различных типов, среди которых можно выделить потоки для чтения и потоки для записи.

При создании сервера в первой главе мы уже сталкивались с потоками:
 ```javascript
 var http = require("http");
http.createServer(function(request, response){
}).listen(3000);
 ```
 Параметры request и response, которые передаются в функцию и с помощью которых мы можем получать данные о запросе и управлять ответом, как раз представляют собой потоки: request - поток для чтения, а response - поток для записи.
 
 
### Pipe
**Pipe** - это канал, который связывает поток для чтения и поток для записи и позволяет сразу считать из потока чтения в поток записи. 

Рассмотрим другую проблему - архивацию файла. Здесь нам надо сначала считать файл, затем сжать данные и в конце записать сжатые данные в файл-архив. Pipes особенно удобно применять для подобного набора операций:
```javascript
 var fs = require("fs");
var zlib = require("zlib");
 
var readableStream = fs.createReadStream("hello.txt", "utf8");
 
var writeableStream = fs.createWriteStream("hello.txt.gz");
 
var gzip = zlib.createGzip();
 
readableStream.pipe(gzip).pipe(writeableStream);
```
Для архивации подключается модуль zlib. Каждый метод pipe() в цепочке вызовов возвращает поток для чтения, к которому опять же можно применить метод pipe() для записи в другой поток.
 
 
 
 
 ### Полезные линки
* [Руководство для начинающих в Node.js](https://proglib.io/p/beginners-guide-to-node-js/)
* [Введение в Node JS](https://metanit.com/web/nodejs/1.1.php)
* [MDN Учебник Express](https://developer.mozilla.org/ru/docs/Learn/Server-side/Express_Nodejs/skeleton_website)
