**前言：**
最近看到有面试题目都会问：请说出几种使用css完成垂直水平居中的方法？正好看css基础的时候也正好看到一篇文章是讲完全居中的，这边对于文章中的内容做个小结。

----------
## 一、使用absolute(Absolute Centering)

#### 1.1 具体代码如下：
```
.container {
    position: relative;
}

.absolute_center {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    width: 50%;
    height: 50%;
    margin: auto;
    padding: 20px;
    overflow: auto;
}
```
```
<div class="container">
    <div class="absolute_center">
        <ul>
            <li>
                该方法的核心思想是是使用绝对定位布局，使当前元素脱离正常的流体特性，而使用absolute的流体特性
            </li>
        </ul>
    </div>
</div>
```
@import "./absolute_center1.png"{width="50%"}

### 1.2 该方法的核心思想是：

    使用absolute进行定位布局，脱离正常的块状元素流体特性，而通过absolute的流体特性完成垂直水平居中。

**这里有两个基本知识点需要知道：**

    1.流体特性：
    块状水平元素，如div元素，在默认情况下（非浮动、绝对定位等），水平方向会自动填满外部的容器；如果有margin-left/margin-right, padding-left/padding-right, border-left-width/border-right-width等，实际内容区域会响应变窄；
    
    2.absolute流体特性：
    默认是不具有流体特性的，而是在特定条件下才具有，而这个条件就是"对立方向同时发生定位的时候"，即left与right为水平方向定位，top与bottom为垂直方向定位，而当对立方向同时具有定位数值的时候，absolute的流体特性就发生了。

### 1.3 优缺点：
    优：
    1.兼容性好，absolute流体特性IE7就兼容了，可兼容所有现代浏览器；
    2.没有额外的标记html元素，样式简单；
    3.内容的宽度以及高度写法支持使用百分比以及min-/max-写法；
    4.对图像文件也同样支持；

    缺：
    1.必须定义内容的高度；
    2.必须增加overflow属性来阻止如果内容的文本高度超出外层容器时出现的溢出情况；
----------
## 二、负值外补侗法(negative margins)
这可能是目前最常用的方法，在元素的高度以及宽度是固定数值的时候，通过设置该元素为相对布局脱离文档流，并设置`top: 50%; left: 50%;`，使用`margin-left`以及`margin-top`使元素完全居中。
### 2.1 具体代码如下：
```
.container {
    position: relative;
    width: 100%;
    height: 100%;
    background-color: aliceblue;
}

.is-Negative {
    position: absolute;
    width: 300px;
    height: 200px;
    padding: 20px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -170px;
    margin-top: -120px;
    background-color: cornsilk;
}
```
```
<div class="container">
    <div class="is-Negative">
    </div>
</div>
```
@import "./absolute_center2.png"{width="50%;"}


### 2.2 该方法的核心思想是：

    使用相对布局，并使用top以及left使内容置于容器中心部位，再使用margin来控制偏移量。

**这里有有个注意点：**
    
    当使用box-sizing:border-box属性的时候，偏移量是不需要计算border以及padding的。

### 2.3 优缺点：
    优：
    1.兼容性好，包括IE6-IE7
    2.没有额外的标记html元素，代码量少；

    缺：
    1.非响应式的，不能配合百分比以及min-/max-使用；
    2.必须增加overflow属性来阻止如果内容的文本高度超出外层容器时出现的溢出情况；
    3.当元素使用box-sizing:border-box和使用默认的content-box偏移量是不一样的，需要重新计算；
----------
## 三、使用Transforms

#### 3.1 具体代码如下：
```
.container {
    position: relative;
    width: 100%;
    height: 100%;
    background-color: aliceblue;
}

.is-Transformed { 
    width: 50%;
    margin: auto;
    position: absolute;
    top: 50%; 
    left: 50%;
    padding: 20px;
    -webkit-transform: translate(-50%,-50%);
        -ms-transform: translate(-50%,-50%);
            transform: translate(-50%,-50%);
    background-color: darkseagreen;
}
```
```
<div class="container">
    <div class="is-Transformed">
        <ul>
            <li>
                该方法的核心思想是使用相对布局，并使用top以及left使内容置于容器中心部位，再使用translate来控制偏移量
            </li>
        </ul>
    </div>
</div>
```
@import "./absolute_center3.png"{width="50%"}

### 3.2 该方法的核心思想是：

    使用相对布局，并使用top以及left使内容置于容器中心部位，再使用transform来控制偏移量。

### 3.3 优缺点：
    优：
    1.内容宽度以及高度自适应，可以不指定内容的宽度以及高度;
    2.代码量少

    缺：
    1.兼容性差了点，由于transform不兼容IE8,所以只支持IE9及其以上的现代浏览器；
    2.为了兼容各种浏览器，需要些额外的css前缀；
    3.如果元素有使用transform属性，可能会影响到其他的变换效果；
    4.在有些时候会出现边缘模糊的情况，这是浏览器渲染的问题，尤其是使用了transform-style: preserve-3d属性的时候
----------
## 四、使用Table-Cell
这可能是最好的垂直居中的方案，但存在一个最大的问题，需要额外的html元素，要使用table-cell完成垂直居中效果需要3个元素来完成。
#### 4.1 具体代码如下：
```
.container {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: aliceblue;
}

.container.is-Table { 
  display: table; 
}

.is-Table .Table-Cell {
  display: table-cell;
  vertical-align: middle;
}

.is-Table .Center-Block {
  width: 50%;
  margin: 0 auto;
  padding: 20px;
  background-color: deepskyblue;
}
```
```
<div class="container is-Table">
    <div class="Table-Cell">
        <div class="Center-Block">
            使用table-cell完成垂直水平居中
        </div>
    </div>
</div>
```
@import "./absolute_center4.png"{width="50%"}

### 4.2 该方法的核心思想是：

    使用表格来实现垂直居中，再使用margin: 0 auto;来实现水平居中。

    具体操作步骤如下：
    1.设置父元素为块级表格；
    2.设置子元素为表格单元格；
    3.给子元素添加vertical-align:middle属性,此单元格已实现垂直居中；
    4.设置子元素中的内容的宽度，添加margin: 0 auto;属性，使子元素水平居中。

### 4.3 优缺点：
    优：
    1.内容高度自适应；
    2.如果子元素的内容溢出，会拉伸父元素的高度；
    3.兼容性好，兼容到IE8；

    缺：
    1.完成一个垂直居中效果，需要3个html元素；
----------
## 五、使用Inline-block
这也是一种常用的垂直水平居中的方法，但会存在一个问题：当内容的宽度大于容器宽度-0.25em的时候，会发生内容上移到顶部的方法，所以需要一些小的技巧来避免这样的问题。
#### 5.1 具体代码如下：
```
.container {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: aliceblue;
}

.container.is-Inline { 
  text-align: center;
  overflow: auto;
}

.container.is-Inline:after,
.is-Inline .Center-Block {
  display: inline-block;
  vertical-align: middle;
}

.container.is-Inline:after {
  content: '';
  height: 100%;
  margin-left: -0.25em; /* To offset spacing. May vary by font */
}

.is-Inline .Center-Block {
  background-color: greenyellow;
  padding: 20px;
  max-width: 99%; /* Prevents issues with long content causes the content block to be pushed to the top */
  /* max-width: calc(100% - 0.25em) /* Only for IE9+ */ 
}
```
```
<div class="container is-Inline">
    <div class="Center-Block">
        使用inline-block完成水平垂直居中
    </div>
</div>
```
@import "./absolute_center5.png"{width="50%"}

### 5.2 该方法的核心思想是：

    和table有点类似，设置内容为inline-block块，设置vertical-align: middle;属性使元素垂直方向居中，再父容器设置text-align:center;使子元素水平方向居中；

### 5.3 优缺点：
    优：
    1.内容高度自适应；
    2.如果子元素的内容溢出，会拉伸父元素的高度；
    3.兼容性好，兼容到IE7；

    缺：
    1.依赖margin-left：-0.25em来矫正水平方向居中的误差；
    2.内容的宽度必须小于容器的宽度减去0.25em。
----------
## 六、使用Flexbox
弹性布局是当前移动端比较流行的布局方式，它可以很优雅的完成垂直水平居中效果。
#### 6.1 具体代码如下：
```
.container {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: aliceblue;
}

.container.is-Flexbox {
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-box-align: center;
      -moz-box-align: center;
      -ms-flex-align: center;
  -webkit-align-items: center;
          align-items: center;
  -webkit-box-pack: center;
      -moz-box-pack: center;
      -ms-flex-pack: center;
  -webkit-justify-content: center;
          justify-content: center;
}

.center_block {
  background-color: wheat;
  padding: 20px;
}
```
```
<div class="container is-Flexbox">
  <div class="center_block">
    使用flexbox完成水平垂直居中
  </div>
</div>
```
@import "./absolute_center6.png"{width="50%"}

### 6.2 该方法的核心思想是：

    使用弹性布局，align-items: center;使元素在侧轴方向居中（默认是垂直方向），justify-content: center;使元素在主轴方向居中（默认是水平方向）;

### 6.3 优缺点：
    优：
    1.内容宽度、高度自适应,即便文本溢出也很优雅；
    2.如果子元素的内容溢出，会拉伸父元素的高度；
    3.兼容性好，兼容到IE7；

    缺：
    1.依赖margin-left：-0.25em来矫正水平方向居中的误差；
    2.内容的宽度必须小于容器的宽度减去0.25em。
----------




   