>**NodeJS** - это _среда выполнения JS_, которая построена на основе движка JavaScript Chrome V8, который позволяет транслировать вызовы на языке JavaScript в машинный код.
---

## Содержание
* [Интерактивная оболочка Node.js](#интерактивная-оболочка-Node.js)
* [Модульная система](#модульная-система)
* [Cтиль кода](#Cтиль-кода)

---

## Интерактивная оболочка Node.js                           _[↑](#cодержание)_

После установки NodeJS становится доступна интерактивная оболочка (или **REPL** == Read Event Printed Loop). REPL (Read Event Printed Loop) предоставляет возможность запуска выражений на языке JavaScript в командной строке или терминале.
запустим командную строку (на Windows) или терминал (на OS X или Linux) и введем команду node. После ввода этой команды мы можем выполнять различные выражения на JavaScript: 
```js
node
> console.log('Hello World');
Hello World
```
или создаем hello.js и запускаем его:
```js
$ node hello.js
Hello World
```

Xтобы выйти из интерактивной оболочки, необходимо просто нажать Ctrl + C.

## Модульная система                              _[↑](#cодержание)_

>**Модуль** - блок кода, который может использоваться повторно в других модулях.

Встроенные модули и функциональности в nodejs тут. Подключаемые модули кэшируются.

## Cтиль кода                           _[↑](#cодержание)_

* **Табуляция vs Пробелы** - 2 пробела
* **Точка с запятой** - пользуйтесь точками с запятыми!
* **Концевые пробелы** - подчищать конечные пробелы
* **Длина строки** - ограничивать строки 80-ю символами
* **Кавычки** - одиночные кавычки везде, кроме случаев редактирования JSON.
_Хорошо:_
```js
var foo = 'bar';
```
_Плохо:_
```js
var foo = "bar";
```
* **Фигурные скобки** - Располагайте открывающую скобку на той же линии, что и выражение.
_Хорошо:_
```js
if (true) {
  console.log('winning');
}
```
_Плохо:_
```js
if (true)
{
  console.log('losing');
}
```
* **Объявление переменных** - Объявляйте по одной переменной с каждым ключевым словом var.
_Хорошо:_
```js
var keys = ['foo', 'bar'];
var values = [23, 42];

var object = {};
while (items.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```
_Плохо:_
```js
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (items.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

* **Имена переменных и свойств** - нижний верблюжий регистр. 
_Хорошо:_
```js
var adminUser = db.query('SELECT * FROM users ...');
```
_Плохо:_
```js
var admin_user = d.query('SELECT * FROM users ...');
```

* **Имена классов** - верхний верблюжий регистр
_Хорошо:_
```js
function BankAccount() {
}
```
_Плохо:_
```js
function bank_Account() {
}
```

* **Константы** - в верхнем регистре

Node.js / V8 поддерживает расширение const от mozilla, но, к сожалению, оно не применимо к членам класса, равно как и часть любого стандарта ECMA.

_Хорошо:_
```js
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```
_Плохо:_
```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

* **Создание Объекта / Массива** - Используйте завершающие запятые и короткие объявления на одной строке. Ключи помещайте в кавычки только тогда, когда интерпретатор может не понять код (составные ключи и т.п.).
_Хорошо:_
```js
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};
```
_Плохо:_
```js
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

* **Оператор равенства** - Используйте тройной оператор равенства, так как только он будет работать так, как ожидается.
_Хорошо:_
```js
var a = 0;
if (a === '') {
  console.log('winning');
}
```
_Плохо:_
```js
var a = 0;
if (a == '') {
  console.log('losing');
}
```

* **Расширение прототипов** - Не расширяйте прототипы всех подряд объектов, особенно встроенных.
_Хорошо:_
```js
var a = [];
if (!a.length) {
  console.log('winning');
}
```
_Плохо:_
```js
Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
  console.log('losing');
}
```

* **Условия** - Все нетривиальные условия должны присваиваться переменной.
_Хорошо:_
```js
var isAuthorized = (user.isAdmin() || user.isModerator());
if (isAuthorized) {
  console.log('winning');
}
```
_Плохо:_
```js
if (user.isAdmin() || user.isModerator()) {
  console.log('losing');
}
```

* **Оператор return** - Возвращайте значение из функции как можно раньше. Тем самым будет исключаться большая вложенность if-else конструкций.
_Хорошо:_
```js
function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
```
_Плохо:_
```js
function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```
_Лучше:_
```js
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

* **Именованные замыкания** - давайте замыканиям имена.
_Хорошо:_
```js
req.on('end', function onEnd() {
  console.log('winning');
});
```
_Плохо:_
```js
req.on('end', function() {
  console.log('losing');
});
```

* **Вложенные замыкания** - Не используйте вложенные замыкания
_Хорошо:_
```js
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```
_Плохо:_
```js
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```

* **Функции обратного вызова (callbacks)**

Так как Node.js прежде всего сосредоточен на неблокирующем вводе/выводе, то функции в основном возвращают свой результат, используя callback’и. В ядре Node.js принято соглашение, что первый параметр любого callback’а всегда является необязательным объектом-ошибкой (error object).

Вам также следует пользоваться этим подходом для своих callback’ов.

* **EventEmitter’ы**

Node.js включает в себя, среди прочего, простой класс EventEmitter, который может быть подключен из модуля ‘events’:
```js
var EventEmitter = require('events').EventEmitter;
```
В основном, это простая реализация шаблона Observer
