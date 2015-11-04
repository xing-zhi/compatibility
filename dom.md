# DOM基本操作
本部分包括基本的DOM操作。

# 获取元素
获取元素涉及兼容性的方法主要有3个：`querySelector`、`querySelectorAll`和`getElementsByClassName`。

## querySelector和querySelectorAll方法
IE8+，但是IE8只支持CSS2级选择器。

## getElementsByClassName方法
IE9+，但是IE8中可以轻松的使用`querySelectorAll(<className>)`方法实现该功能。

{% highlight javascript linenos %}
function getElementsByClassName(className, node = document) {
  if ( node.getElementsByClassName ) {
    return node.getElementByClassName(className);
  } else {
    const selector = className.splite(' ').map((x) => `.${x}`).join('');
    return node.querySelectorAll(selecotr);
  }
}
{% endhighlight %}

# 获取元素的尺寸和位置
元素的位置包括在视口中的位置和文档中的位置，而两者之间的转换通过页面滚动偏移量完成。

## 获取元素的尺寸和在视口中的位置
W3C标准的`getBoundingClientRect`方法返回包含元素在文档中位置（`left`、`right`、`top`和`bottom`）和尺寸（`width`和`height`）的对象，但是IE8-返回的对象不包含`width`和`height`属性。

为了兼容IE8-，在使用`width`和`height`属性时需要检查这两个属性是否存在，同时提供后备。
{% highlight javascript linenos %}
const el = document.querySelector('.target'),
      box = el.getBoundingClientRect(),
      width = box.width || box.right - box.left,
      height = box.height  || box.bottom - box.top;
{% endhighlight %}

## 获取页面滚动偏移量
获取页面滚动偏移量的标准方法是查询`window.pageXOffset`和`window.pageYOffset`属性，这两个属性只存在于IE9+。同时，这两个属性是`window.scrollX`和`window.scrollY`的别名，但是`window.scrollX`和`window.scrollY`的兼容性是IE Edge，为了更好的兼容性，使用`window.pageXOffset`和`window.pageYOffset`。
{% highlight javascript linenos %}
console.log(window.pageXOffset === window.scrollX);    // => true
console.log(window.pageYOffset === window.scrollY);    // => true
{% endhighlight %}

在IE8-中，需要通过查询文档根节点的`scrollLeft`和`scrollTop`属性获取页面滚动偏移量。

兼容IE8的获取页面滚动偏移量的方法如下：
{% highlight javascript linenos %}
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
{% endhighlight %}

