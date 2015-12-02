# 原生对象方法的兼容性
这个文档中包含原生对象方法的兼容性。

# 函数方法
## Function.prototype.bind
兼容性：IE9+

polyfill，使用`Function.prototype.apply`和闭包。

{% highlight javascript linenos %}
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
{% endhighlight %}

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
