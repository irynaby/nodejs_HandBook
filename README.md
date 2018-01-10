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


### Полезные линки
* [Руководство для начинающих в Node.js](https://proglib.io/p/beginners-guide-to-node-js/)
* [Введение в Node JS](https://metanit.com/web/nodejs/1.1.php)


Node.js использует **модульную систему**. _Модуль_ - блок кода, который может использоваться повторно в других модулях.

Встроенные модули и функциональности в nodejs [тут](https://nodejs.org/api/).

Для загрузки модулей применяется функция _require()_ .
```javascript
var http = require("http");
```

Подключаемые модули кэшируются.

