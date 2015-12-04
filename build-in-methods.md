# 原生对象方法的兼容性
这个文档中包含原生对象方法的兼容性和polyfill。不涉及ES6引入的新方法，并且Object对象只包含了两个常用的方法。

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

## Array.prototype.filter
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.filter ) {
  Array.prototype.filter = function(cb, context) {
    const arr = this,
          filteredArr = [];

    context = context || this;

    for ( let i = 0, len = arr.length; i < len; i++ ) {
      if ( cb.call(context, arr[i], i, arr) ) {
        filteredArr.push(item[i]);
      }
    }

    return filteredArr;
  };
}
```

## Array.prototype.forEach
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.forEach ) {
  Array.prototype.forEach = function(cb, context) {
    const arr = this;

    context = context || this;

    for ( let i = 0, len = arr.length; i < len; i++ ) {
      cb.call(context, arr[i], i, arr);
    }
  };
}
```

## Array.prototype.indexOf
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.indexOf ) {
  Array.prototype.indexOf = function(searchValue, fromIndex = 0) {
    const arr = this;

    for ( let i = fromInex, len = arr.length; i < len; i++ ) {
      if ( arr[i] === searchValue ) {
        return i;
      }
    }

    return -1;
  };
}
```

## Array.prototype.lastIndexOf
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.indexOf ) {
  Array.prototype.indexOf = function(searchValue, fromIndex) {
    const arr = this;

    fromIndex = fromIndex || arr.length - 1;

    for ( let i = fromIndex; i > 0; i-- ) {
      if ( arr[i] === searchValue ) {
        return i;
      }
    }

    return -1;
  };
}
```

## Array.prototype.map
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.map ) {
  Array.prototype.map = function(cb, context) {
    const arr = this,
          mapedArr = [];

    context = context || this;

    for ( let i = 0, len = arr.length; i < len; i++ ) {
      mapedArr[i] = cb.call(context, arr[i], i, arr);
    }
  };

  return mapedArr;
}
```

## Array.prototype.reduce
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.reduce ) {
  Array.prototype.reduce = function(cb, initialValue) {
    const arr = this;
    let startIndex = 0;

    if ( arguments.length === 1 ) {
      initialValue = arr[0];
      startIndex = 1;
    }

    let reducedValue = initialValue;

    for ( let i = startIndex, len = arr.length; i < len; i++ ) {
      reducedValue = cb.call(this, reducedValue, arr[i], i, arr);
    }

    return reducedValue;
  };


}
```

## Array.prototype.reduceRight
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.reduceRight ) {
  Array.prototype.reduceRight = function(cb, initialValue) {
    const arr = this,
          len = arr.length;
    let startIndex = len - 1;

    if ( arguments.length === 1 ) {
      initialValue = arr[len - 1];
      startIndex = len - 2;
    }

    let reducedValue = initialValue;

    for ( let i = startIndex; i >= 0; i-- ) {
      reducedValue = cb.call(this, reducedValue, arr[i], i, arr);
    }

    return reducedValue;
  };
}
```

## Array.prototype.some
兼容性： IE9+

polyfill，使用普通的`for`循环。

```javascript
if ( !Array.prototype.some ) {
  Array.prototype.some = function(cb, context) {
    const arr = this;

    context = context || this;

    for ( let i = 0, len = arr.length; i < len; i++ ) {
      if ( cb.call(context, arr[i], i, arr) ) {
        return true;
      }
    }

    return false;
  };
}
```

# String
## String.prototype.trim
兼容性： IE9+

polyfill，使用`String.prototype.replace`实现。

```javascript
if ( !String.prototype.trim ) {
  String.prototype.trim = function() {
    return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/, '');
  };
}
```

# Object
Object对象的方法只涉及到常用的，即`Object.create`和`Object.keys`。

## Object.create
兼容性： IE9+

polifill，使用构造函数实现。

```javascript
if ( !Object.create ) {
  Object.create = function(obj) {
    const fn = function() {},
          hasOwn = Object.prototype.hasOwnProperty;    

    if ( obj !== null && Object.prototype.toString.call(obj) !== '[object Object]' ) {
      throw TypeError('Object prototype must be an Object or null');
    }

    fn.prototype = obj;

    const newObj = new fn();

    if ( arguments[1] ) {
      const properties = Object(arguments[1]);

      for ( let key in properties ) {
        if ( hasOwn.call(properties, key) ) {
          newObj[key] = properties[key];
        }
      }
    }

    return newObj;
  };
}
```

## Object.keys
兼容性： IE9+

polyfill，使用`for...in`实现。
```javascript
if ( !Object.keys ) {
  Object.keys = function(obj) {
    const hasOwn = Object.prototype.hasOwnProperty,
          keys = [];

    if ( obj !== null && Object.prototype.toString.call(obj) !== '[object Object]' ) {
      return keys;
    }

    for ( key in obj ) {
      if ( hasOwn.call(obj, key) ) {
        keys.push(key);
      }
    }

    return keys;
  }
}
```
