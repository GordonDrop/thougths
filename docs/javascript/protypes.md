# Прототипы

## Прототип объекта

### скрытое свойство [[Prototype]]

**Прототипное наследование** - если происходить обращение к свойству объекту, но у объекта его нет, то оно будет взято у прототипа.

 Само с-во [[Prototype]] скрытое, но можно установить его след образом:

 - свойство __proto__ - истор getter/setter
 - Object.getPrototypeOf/Object.setPrototypeOf

**Пример 1**
 ```
let animal = { eats: true };
let rabbit = { jumps: true };

rabbit.__proto__ = animal;

alert( rabbit.eats ); // true (**)
alert( rabbit.jumps ); // true
```

**Пример 2**
```
let animal = {
  eats: true,
  walk() { console.log("Animal walk"); }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

rabbit.walk(); // Animal walk
```

### Правила создания прототипов

1. Без циклических ссылок. результат:
`Uncaught TypeError: Cyclic __proto__ value`
2. Скрытое свойство [[Prototype]], либо ссылка на другой объект либо null. Примитивы игнорируются.

### Лукапы

Прототипы только для чтения **свойств**. Операция записи или удаления работают напрямую в объекте.

Функции остаются у прототипа, но это иначе работает, если свойство это setter/getter, то свойство ведет себя как функция setter/getter ищется в прототипе.

### Значение this

Прототипы не влиют на this. Не важно где метод найден, `this` это всегда объект перед точки.

## F.prototype

При вызове `new F()`, если F.prototype это объект, то он будет использован, чтобы установить св-во [[Prototype]] нового объекта.

```
let animal = { eats: true };

function Rabbit(name) {
  this.name = name;
}
Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal // true
rabbit.eats // true
```
**!** F.prototype используется только раз - во время вызова new F. Если создать два объекта одной функцией и каждый раз менять прототип, то каждый объект будет имеет разные прототипы.

### Default F.prototype, constructor property

Каждая функция имеет свойство prototype. По-умолчанию, это объект со свойством constrcutor, указывающим на саму функцию.

```
function Rabbit() {}
/* default prototype
Rabbit.prototype = { constructor: Rabbit };
*/
```

Если ничего не делать, то прототип по умолчанию, будет доступен всем потомкам.

js не проверяет правильный конструктор установлен для прототипа. Его также может и не быть в прототипе, если мы заменим прототип.

На обычных объектах это свойство не работает.

## Нативные прототипы

Св-во prototype широко используется встроенным функции конструкторами.

### Object.prototype

При создании объекта с помощью литерала или вызоав new Object() св-во [[Prototype]] нового объекта будет установлен на Object.prototype по всем правилам.

Но при этом выше по цепочке прототип больше нет никаких дополительных прототипов

```
console.log(Object.prototype.__proto__); // null
```

### Другие встроенные прототипы

Другие встр объекты, такие как Array, Data, Functions, тоже хранят свои методы в прототипах.

Все они наследуются от Object.prototype.

### Примитивы

Это не объекты, но если мы попытаемся обратиться к каком либо их сво-во, то будет создан временный объек обертка с испл-м встр-го конструкторв String, Number, Boolean, они предоставляют методы и исчезают.

Все их методы и св-ва также доступны через св-во prototype конструктора.

### Изменене нативного прототипа

В совр мире js, модификаци прототипов не поощряется, так как нет гарантии, что мы единственные кто модифицируем прототип.

Единственный разрешенный способ модификации - это полифилы.

### Одалживание у прототипа

```
let obj = {
  0: "Hello",
  1: "world!",
  length: 2,
};

obj.join = Array.prototype.join;

alert( obj.join(',') ); // Hello,world!
```

## Prototype methods, objects without __proto__

__proto__ считается устаревшим(он есть только в браузерах).

Современные методы:

- Object.create(proto[, descriptors]) – creates an empty object with given proto as [[Prototype]] and optional property descriptors.

- Object.getPrototypeOf(obj) – returns the [[Prototype]] of obj.

- Object.setPrototypeOf(obj, proto) – sets the [[Prototype]] of obj to proto.

### Исторические причины

Кол-во способ управлять св-м [[P]] много так как:
- Св-во constructro.prototype работает с древних времен

- в 2012 Object.create появилось в стандарте. Можно было создать об-т с прототипом, но нельзя get/set it. В браузеры добавило __proto__ как getter/setter

- В 2015м Object.setPrototypeOf and Object.getPrototypeOf были добавлены в стандарт. __proto__ был уже везде реализован, и это сделало необязательным.

### “Very plain” objects

Любые произвольные ключи можно установить в объекте, кроме __proto__, так св-во __proto__ это либо объект либо null.

Обход проблемы:

1. Исп-те Map
2. __proto__ это способ получить доступ к объекту, это accessor, а не сам [[P]]. В Object.prototype есть get/set __proto__. Если мы создадим объект без прототипа, то у него не будет ссылки на Object.prototype => нет и унаследованного accessor'а __proto__. Минус: нет встроенных методов.

### Получение всех свойств

- Object.keys(obj) / Object.values(obj) / Object.entries(obj)  - получение собств. и только перечисляемых св-в и только тех чьи ключи это строки.

- Object.getOwnPropertySymbols(obj) – символьные имена объекта.

- Object.getOwnPropertyNames(obj) - собственные непречислымые имена

- Reflect.ownKeys(obj) - массив всех собств имен

Проверка
obj.hasOwnProperty(key): true если obj имеет собственное имя key

[Сурс](https://javascript.info/)