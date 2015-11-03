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

