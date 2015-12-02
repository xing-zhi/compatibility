# 原生对象方法的兼容性
这个文档中包含原生对象方法的兼容性。

# 函数方法
## Function.prototype.bind
兼容性：IE9+

polyfill，使用`Function.prototype.apply`和闭包。

```javascript
if ( !Function.prototype.bind ) {
  Function.prototype.bind = function(context) {
    var slice = Array.prototype.slice,
        argsToBind = slice.call(arguments, 1),
        fn = this;

    return function() {
      var args = argsToBind.concat(slice(arguments));

      return fn.apply(context, args);
    };
  };
}
```

# 数组方法
## Array.isArray
兼容性：IE9+

polyfill，使用`Object.prototype.toString`方法。

```javascript
if ( !Array.isArray ) {
  Array.isArray = function(arr) {
    return Object.prototype.toString.call(arr) === '[object Array]';
  };
}
```

## Array.prototype.every
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.every ) {
  Array.prototype.every = function(cb, context) {
    const arr = this;

    context = context || this;

    for ( let i = 0, len = arr.length; i < len; i++ ) {
      if ( !cb.call(context, arr[i], i, arr) ) {
        return false;
      }
    }

    return true;
  };
}
```

## Array.prototype.every
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.every ) {
  Array.prototype.every = function(cb, context) {
    const arr = this;

    context = context || this;

    for ( let i = 0, len = arr.length; i < len; i++ ) {
      if ( !cb.call(context, arr[i], i, arr) ) {
        return false;
      }
    }

    return true;
  };
}
```
