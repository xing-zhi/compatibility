# DOM基本操作
本部分包括基本的DOM操作。

# 获取元素
获取元素涉及兼容性的方法主要有3个：`querySelector`、`querySelectorAll`和`getElementsByClassName`。

## querySelector和querySelectorAll方法
IE8+，但是IE8只支持CSS2级选择器。

## getElementsByClassName方法
IE9+，但是IE8中可以轻松的使用`querySelectorAll(<className>)`方法实现该功能。

```javascript
function getElementsByClassName(className, node = document) {
  if ( node.getElementsByClassName ) {
    return node.getElementByClassName(className);
  } else {
    const selector = className.splite(' ').map((x) => `.${x}`).join('');
    return node.querySelectorAll(selecotr);
  }
}
```

# 获取元素的尺寸和位置
元素的位置包括在视口中的位置和文档中的位置，而两者之间的转换通过页面滚动偏移量完成。

## 获取元素的尺寸和在视口中的位置
W3C标准的`getBoundingClientRect`方法返回包含元素的尺寸（`width`和`height`）和在视口中的位置（`left`、`right`、`top`和`bottom`）的对象，但是IE8-返回的对象不包含`width`和`height`属性。

为了兼容IE8-，在使用`width`和`height`属性时需要检查这两个属性是否存在，同时提供后备。

```javascript
const el = document.querySelector('.target'),
      box = el.getBoundingClientRect(),
      width = box.width || box.right - box.left,
      height = box.height  || box.bottom - box.top;
```

## 获取页面滚动偏移量
获取页面滚动偏移量的标准方法是查询`window.pageXOffset`和`window.pageYOffset`属性，这两个属性只存在于IE9+。同时，这两个属性是`window.scrollX`和`window.scrollY`的别名，但是`window.scrollX`和`window.scrollY`的兼容性是IE Edge，为了更好的兼容性，使用`window.pageXOffset`和`window.pageYOffset`。

```javascript
console.log(window.pageXOffset === window.scrollX);    // => true
console.log(window.pageYOffset === window.scrollY);    // => true
```

在IE8-中，需要通过查询文档根节点的`scrollLeft`和`scrollTop`属性获取页面滚动偏移量。

兼容IE8的获取页面滚动偏移量的方法如下：
```javascript
function getPageOffsets() {
  if ( window.pageXOffset ) {
    return {
      x: window.pageXOffset,
      y: window.pageYOffset
    };
  }

  const documentEl = document.documentElement;

  return {
    x: documentEl.scrollLeft,
    y: documentEl.scrollTop
  };
}
```

# 事件
JavaScript主要通过事件和文档进行交互。

## 事件流
IE8-只支持事件冒泡，不支持事件捕获；包括IE9+的所有现代浏览器同时支持事件冒泡和事件捕获。

## 添加和移除事件处理程序
DOM2级事件定义了`addEventListener`和`removeEventListener`用于添加和移除事件处理程序。

默认情况下，使用`addEventListener`方法添加的事件处理程序在事件冒泡阶段执行，如果需要在事件捕获阶段执行，设置第3个参数为`true`。

支持这两个方法的浏览器为IE9+，所以对于IE8需要使用IE专有的方法`attachEvent`和`detachEvent`方法。

IE专有方法具有以下差异：

+ 添加事件处理程序时，要在事件类型前添加`on`前缀，比如注册`click`事件需要使用`onclick`
+ 事件处理程序在全局作用域中运行，事件处理程序中的`this`对象为`window`（IE10+才支持严格模式，无法通过指定严格模式使`this`为`undefined`），而不是注册的元素对象
+ 添加多个事件处理程序时，按添加顺序相反的顺序执行，而`addEventListener`添加多个事件处理程序时是按添加顺序执行
+ 事件处理程序只能添加到事件冒泡阶段

需要兼容IE8时，添加和移除事件处理程序的方法如下：

```javascript
const eventHandler = {
  add(el, type, handler) {
    if ( el.addEventListener ) {
      el.addEventListener(type, handler);
    } else if ( el.attachEvent ) {
      el.attachEvent(`on${type}`, handler);
    }

    throw new Error('本方法不支持该浏览器，请使用其他方法注册事件处理程序');
  },
  remove(el, type, handler) {
    if ( el.removeEventListener ) {
      el.removeEventListener(type, handler);
    } else if ( el.detachEvent ) {
      el.detachEvent(`on${type}`, handler);
    }

    throw new Error('本方法不支持该浏览器，请使用其他方法注册事件处理程序');
  }
};
```

因为IE专有方法存在的问题，如果不需要兼容IE8-，总是使用DOM2级方法添加和移除事件处理程序。

## 事件处理程序中的`this`
通过IE的`attachEvent`方法添加是事件处理程序中，`this`对象为`window`对象。所以在需要兼容IE8时，不要在事件处理程序中使用`this`。
