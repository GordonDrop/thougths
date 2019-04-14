\#js \#async \#await

# async function

```
async function name([param[, param[, ... param]]]) {
   statements
}
```

Объявляет аснихр ф-цию, к-я возвращает неявно возвращает Promise.resolve().

Пример 1.1:
```
async function a() {}
a
// async function a() {}

Object.getPrototypeOf(async function(){}).constructor
// ƒ AsyncFunction() { [native code] }
```

Пример 1.2

Результат выполнения всегда промис P. Промис создается на начало выполнения. Затем выполняется тело ф-ции, выполнения может закончиться через return или throw. В конце происходит возврат промиса P. Или же иногда это может быть await, тогда выполнение может продолжиться позже.`

```
async function asyncFunc() {
    console.log('asyncFunc()'); // (A)
    return 'abc';
}
asyncFunc().then(x => console.log(`Resolved: ${x}`)); // (B)
console.log('main'); // (C)

// Output:
// asyncFunc()
// main
// Resolved: abc
```

Пример 2: async, await and error handling

Асинх ф-я может содержать await выражение. Если использовать его вне async ф-ции, будет выброшено исключение. await выр-е остановить выполнение async fn до resolve или reject.

```
function getProcessedData(url) {
  return downloadData(url) // returns a promise
    .catch(e => {
      return downloadFallbackData(url)  // returns a promise
    })
    .then(v => {
      return processDataInWorker(v); // returns a promise
    });
}

async function getProcessedData(url) {
  let v;
  try {
    v = await downloadData(url);
  } catch(e) {
    v = await downloadFallbackData(url);
  }
  return processDataInWorker(v);
}
```

Пример 3:
Последовательное выполнение, каждый промис пока не выполнится

```
async function a() {
    const p1 = async () => // some async stuff
    const p2 = async () => // some async stuff

    await p1();
    await p2();
}
```

Пример 4:
Конкурентное выполнение, выполняются одновременно, но важен результат, в котором рез-ты используем.

```
async function a() {
    const p1 = async () => // some async stuff
    const p2 = async () => // some async stuff

    const result1 = p1();
    const result1 p2();

    console.log(await result1);
    console.log(await result2);

}
```

Пример 4:
Параллельное выполнение, выполняем одновременно, каждый результат используется независимо от другого.

```
async function a() {
    const p1 = async () => // some async stuff
    const p2 = async () => // some async stuff

    await Promise.all([
        (async() => console.log(await p1()))(),
        (async() => console.log(await p2()))()
    ]);
}
```

Пример 4 аналогичен:
```
var parallelPromise = function() {
  console.log('==PARALLEL with Promise.then==');
  resolveAfter2Seconds().then((message)=>console.log(message));
  resolveAfter1Second().then((message)=>console.log(message));
}
```

Пример 5 - это непонятно, надо разобраться.
Edit: Выглядит как завершение таска, значит можно запустить микротаски какие есть.

```
Promise.resolve().then(()=>console.log(2));

(async()=>{
  console.log(1);
  await undefined;
  console.log(3);
})();

// output: 1 2 3
```

Пример 6 - что тут ваще происходит??? O_o

```
async function async1() {
    console.log("a");
    await async2(); 
    console.log("b");
}

async function async2() {
   console.log( 'c');
}

console.log("d");

setTimeout(function () {
    console.log("e");
},0);

async1();

Promise.resolve()
    .then(() => console.log("f"))
    .then(() => console.log("g"));

console.log('h');

d
a
c
h
b
f
g
e
```

## Реалзизация в браузере и node

Результат выполнения след кода в браузере может отличаться от результата выполнения в node10+.

```
const p = Promise.resolve();

(async () => {
  await p; console.log('after:await');
})();

p.then(() => console.log('tick:a'))
 .then(() => console.log('tick:b'));

// Chrome ~70
// after:await
// tick:a
// tick:b

// node 10
// tick:a
// tick:b
// after:await
 ```

Источник:
[async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
[Faster async functions and promises](https://v8.dev/blog/fast-async)
[http://2ality.com/2016/10/async-function-tips.html](http://2ality.com/2016/10/async-function-tips.html)