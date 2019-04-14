\#js \#event_loop

# Async JS, event loop

## SYNCHRONOUS Tasks

Пример 1:

```
console.log(1);
console.log(2);
console.log(3);
// 1 2 3
```

Выполнение происходит по кадрам. В стек попадает одна функция, выполняет и выкидывается. И так выполнение пока есть код.

Пример 2:

```
function Three() {
    return 'some';
}

function Two() {
    Tree();
}

function One() {
    Two();
}

One();
```

В данном примере стек будет формироваться так:

- в стек будет помешена ф-ция One()
- ф-ция Two(), помешается наверх стека
- ф-ция Three(), помешается наверх стека

Стек:
Three()
Two()
One()

- При выполнение Three() ф-ция выкидывается из стека, затем Two(), затем One()

Стековые кадры:

Three()
Two()     Two()
One()     One()     One()   <empty>

## ASYNCHRONOUS Tasks

Пример 3:

```
console.log(1);
setTimeout(() => console.log(3), 1000);
console.log(2);

// Output: 1 2 3
```

Пример аналогичен в плане выполнения синр кода Примеру 1, но с одной разницей - функция внутри setTimeout выполнится не сразу.

При выполнении setTimeuot функция будет помещена в callback queuee, и когда в стеке будет пуст, то исполнится эта ф-ция.

Пример 4:
```
function a() {
    setTimeout(
        () => { console.log('1'); },
        500
    );

    for(let i = 0; i < 10e7; i++) { Math.random(); }
    console.log('2');

    setTimeout(
        () => { console.log('3'); },
        0
    )
}
a();
```

У таймеров, как у синхронного кода - он выполняется как стек вызовов пуст. Здесь возможно два варианта вывода в зависимости от времени выполения цикла.

Если время выполнения цикла в стеке займет больше чем 500ms:
// output
2 1 3

1 будет выведен первым, так как его время выполнения подошло и он попал в очередь первым

// output
2 3 1

1 будет выведен последним, так как его время выполнения не подошло

Пример 5:

Если мы изменим пример, то мы сможем увидеть, что коллбеки будет вызваны в порядке очереди:
- сначала синхронная двойка
- затем 1 3

```
function a() {
    setTimeout(
        () => { console.log('1'); },
        0
    );

    for(let i = 0; i < 10e7; i++) { Math.random(); }
    console.log('2');

    setTimeout(
        () => { console.log('3'); },
        0
    )
}
a();
```

## Event loop, таски, микротаски

Event loop работает с тасками и микротасками.

Также может быть несколько источников тасков(очередей), что гарантирует очередность в рамках одной очереди:
https://www.w3.org/TR/2014/REC-html5-20141028/webappapis.html#event-loops

Таски планируются так, чтобы браузер мог распределить их так, чтобы не ухудшить perfomance. Между таски браузер может редндерить.

Микротаски выполняются между тасками. Микротаски, которые появляются во время микротасков также добавляются в очередь и выполняются.

Если в общем, то микротаски выполняются:
- выполняются после каждого колбека, до тех пор пока нет другого скрипта по середние выполнения
- в конце каждого таска

Пример 1:

В данном пример есть один большой таск, это полное выполнение всего синхронного кода. Далее происходит выполнение микротасков (промисы). За ним следует коллбек таймаута.

```
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');

// output
script start
script end
promise1
promise2
setTimeout
```

**Что где?**

Согласно спеке setTimeout ставит в очередь таск. Мутации это микротаски.
Колбеки на DOM события это таски.

Тута есть примерчик, когда мы превращаем асинхронный код в синхронный, и соотв-но меняются порядок испол-я:

Источник:
[https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)


## Еще немного про таски

### Таски:

I/O bound
- таймаут
- xhr/fetch
- сеть(чтение db)
- чтение файлов

CPU bound - не делимые

- for
- JSON.parse
- Hashing

Виды:
- user interactions
- timeouts
- postMessage

Итого:
- Приоретизация происходит через различные очереди
- Решается все на стороне браузера
- Реализация event loop в браузерах старая, и не всегда соотв-т спецификации

### Микротаски

Цикл выполнения:

1. Выбрать свободную таску
2. Выполнить
3. Выполнить микротаск чекпоинт
4. Обновить рендеринг(если нужно)
5. Вернутсья на 1

На шаг 3 выполняются все микротаски из очереди. Можно порождать новые.

Выполняются также после очистки стека.

Пример 1: Этот пример займет стек навечно

```
function loopPromise() {
    return Promise.resolve().then(loopPromise);
}
```

Пример 2:

```
function onClick() {
  console.log('click');

  setTimeout(function() {
    console.log('timeout');
  }, 0);

  Promise.resolve().then(function() {
    console.log('promise');
  });
}

inner.addEventListener('click', onClick);
outer.addEventListener('click', onClick);

// output
click
promise
timeout
click
promise
timeout

```

Источник:
[https://events.yandex.ru/lib/talks/6393/](https://events.yandex.ru/lib/talks/6393/)