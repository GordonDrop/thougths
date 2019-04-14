#js

Примеры кода, слишком мелкие для отдельной страницы.

## Debounce

```
// откладываем выполнения действия, чтобы вызов fn не происходил слишком часто
function debounce(fn, ms) {
  let timer = null;
  
  return function() {
    const onComplete = () => {
      fn.apply(this, arguments);
      timer = null;
    };

    if (timer) {
      clearTimeout(timer);
    }

    timer = setTimeout(onComplete, ms);
  }
}
```

## Throttle

```
function throttle(fn, ms) {
  let savedCtx, saveArgs;
  let isThrottled = false;
  
  return function () {
    if (isThrottled) {
      saveArgs = arguments;
      savedCtx = this;
      return;
    }

    fn.apply(this, arguments);
    isThrottled = true;

    setTimeout(() => {
      if (saveArgs) {
        fn.apply(savedCtx, saveArgs);
        saveArgs = savedCtx = null;
      }
      isThrottled = false;
    }, ms);
  }
}
```