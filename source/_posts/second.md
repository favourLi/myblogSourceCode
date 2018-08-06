---
title: '|| && 的巧用'
date: 2018-06-12 15:43:19
tags:
    - '||'
    - '&&'
---

### 前言
今天看代码的时候，看到了下面这行代码

    const PORT = process.env.PORT && Number(process.env.PORT)

>{% asset_img 1.png %}__“还能这样玩？？？”__

然后去google了一下才发现 js 运算符 || && 的妙用。
<!-- more -->

### 正文

先出一道题

{% asset_img 2.jpg %}

如图：
假设对成长速度规定如下：
成长速度为5则显示1个箭头；
成长速度为10则显示2个箭头；
成长速度为12则显示3个箭头；
成长速度为15则显示4个箭头；
其他都显示为0个箭头。

差一点的 if else： （我现在就处于这个阶段。。。汗）

    var arrow = 0;
    if (speed == 5) {
    	arrow = 1
    } else if (speed == 10) {
    	arrow = 2
    } else if (speed == 12) {
    	arrow = 3
    } else if (speed == 15) {
    	arrow = 4
    } else {
    	arrow = 0
    }


好一点的 switch case

    switch(speed) {
      case 5: arrow = 1;
      break;
      case 10: arrow = 2;
      break;
      case 12: arrow = 3;
      break;
      case 15: arrow = 4;
      break;
      default: arrow = 0;
      break
    }

但是如果改一下要求：
成长速度为>12显示4个箭头；
成长速度为>10显示3个箭头；
成长速度为>5显示2个箭头；
成长速度为>0显示1个箭头；
成长速度为<=0显示0个箭头。

那么 switch case 用起来也不太方便了。

这时候就到了我们今天的重点了。

    var arrow = (speed == 5 && 1) || (speed == 10 && 2) || (speed == 12 && 3) || (speed == 15 && 4) || 0;

第二个需求

    var arrow = (speed > 12 && 4) || (speed > 10 && 3) || (speed > 5 && 2) || (speed > 0 && 1) || 0;

首先来复习一下 js 里面的基础知识.在 js 逻辑运算中, 0, "", null, undefined, NaN都会在判断的时候隐式转化为 false ,其他都为 true.
这里随便提下，经常看到别人代码里 if(!!attr)，刚开始挺疑惑为什么不写成 if(attr) 的方式，后来了解到这是一种更严谨的写法，!! 的作用是把一个其他类型的变量转成boolean类型。
下面主要介绍下 || 和 &&.
几乎所有计算机语言中 || 和 && 都遵循“短路”原则，即 && 判断式中第一个表达式为假则不会判断第二个表达式，|| 正相反。
js 也遵循上述规则，但是比较有意思的是返回的值。

    var attr = true && 4 && 'aaa';

运行的结果，attr的值就为‘aaa’。

再来看看||：

    var attr = attr || {};

这个运算经常用来判断一个变量是否已定义，否则的话就给它一个初始值，这再给函数参数定义默认值的时候比较常见。

### 用法

    if (a >= 5) {
      alert('你好')
    }

可以写成

    a >= 5 && alet('你好')

这样一行表达式就可以搞定。但是需要权衡的是，js 中 || 和 && 的特性在帮我们精简了代码的同时，也降低了代码的可读性。

我们可以不使用这些代码，但是我们一定要能看懂，因为这些技巧已经被广泛应用，尤其是在 jquery 等 js 框架里的代码。

最后让我们看一段 jquery 里面的代码：

    var wrap = 
    // option or optgroup
    !tags.indexOf("<opt") && [ 1, "<select multiple='multiple'>", "</select>" ] ||

    !tags.indexOf("<leg") && [ 1, "<fieldset>", "</fieldset>" ] ||

    tags.match(/^<(thead|tbody|tfoot|colg|cap)/) && [ 1, "<table>", "</table>" ] ||

    !tags.indexOf("<tr") && [ 2, "<table><tbody>", "</tbody></table>" ] ||

    // <thead> matched above
    (!tags.indexOf("<td") || !tags.indexOf("<th")) && [ 3, "<table><tbody><tr>", "</tr></tbody></table>" ] ||

    !tags.indexOf("<col") && [ 2, "<table><tbody></tbody><colgroup>", "</colgroup></table>" ] ||

    // IE can't serialize <link> and <script> tags normally
    !jQuery.support.htmlSerialize && [ 1, "div<div>", "</div>" ] ||

    [ 0, "", "" ];

    // Go to html and back, then peel off extra wrappers
    div.innerHTML = wrap[1] + elem + wrap[2];

    // Move to the right depth
    while ( wrap[0]-- )
        div = div.lastChild;

这段代码是作者用来处理 `$(html)` 时，有些代码必须要约束的， 如 `<option>` 必须在 `<select></select>` 标签对里面。还有作者很巧妙的一点就是 `!tags.indexOf('<opt')`,很简单的就实现了 startWith 的功能，没有多余的代码。jquery 中还有很多这种精妙的代码，大家都可以去学习学习。
