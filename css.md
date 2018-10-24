#HTML的相关问题
__________________________________________
### 1.CSS 选择器的优先级是如何计算的？
import > 内联样式 > ID选择器 > 类选择器 + 属性选择器 + 伪类选择器 > 标签选择器 伪元素选择器。

### 2. 重置（reset） 和 标准化（normal） css的区别？ 你会选择哪种？ 为什么？
1.重置即所有标签的默认样式都统一，无每个标签的特殊化，个性化样式；
2.标准化是去掉部分的默认样式，保留一些个性化的，同时对于各个浏览器做了相应的增强处理； 比较著名的框架，如： noramlize.css
3.我会选择标准化的方式，先使用normalize的方式重置浏览器初始样式，再针对具体元素重置样式。

### 3.float浮动的工作原理？
float属性的新增使元素脱离原来的流体特性，但不同于absolute的流体特性，直接脱离了文档的流体特性，是另一种流体特性，使元素可以相对于父容器的padding范围内靠左还是靠右布局。
清除浮动的方法原理：
1.利用clear:both来清除；
2.利用容器的BFC特性；

利用clear:both特性中可以分两种，
        1.一种直接在浮动元素的相邻的兄弟元素增加节点添加属性clear:both;但这种还是没有解决margin重叠的问题;
        2.浮动的父元素使用伪元素，产生与浮动元素的相邻的兄弟节点，并赋予clear: both;属性; 
        e.g. 
        ```
        .clearfix {
          content: "";
          display: block;
          clear: both;
        }
        ```

导致margin重叠问题主要原因还是处于同一个BFC(Block Formatting Context)中，解决的方法也是产生新的BFC作用域，
产生BFC的方法
1.float: left | right | both;
2.overflow: hidden | auto | scroll;
3.display: table-cell | inline-block | table-caption;
4.position: absolute | fixed;

利用新产生BFC的方法：
父容器设置：overflow | position | float | display上述数值

但这样会改变元素的默认属性，不通用

结合上述两种，得出一个比较好的清除浮动的方法

```
.clearfix::before,
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
```

### 4.请阐述z-index属性，并说明如何形成层叠上下文（stacking context）？
1.z-index控制重叠元素的垂直方向叠加顺序。z-index只会影响position属性不为static的元素。
2.没有定义z-index的元素，元素按照他们出现在dom中的顺序堆叠。非静态的定位的元素始终位于静态元素定位的上方，不管HTML中的结构。
3.层叠上下文是包含一组图层的元素。在一组层叠上下文中，其子元素是相对于父元素的而不是document根节点，每个元素的上下文完全独立于其兄弟元素。即如果A位于B的上方，但A中的子元素C比B有更高的z-index的值，但C还是在B下面。
4.每个层叠元素应该是自包含的：当发生上下文层叠的时候，其会在他的父元素的上线文中进行层叠，一般这个上线文是不会改变的，少数的css属性会触发新的层叠上下文。

### 5.请阐述块格式化上下文（Block Formatting Context）及其工作原理？
1.BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

HTML页面都是由一个个盒子组成的，而每个盒子的内部元素都是相互独立的，有自己的作用域。
发生BFC的条件：
1.float: left | right | both;
2.position: absolute | fixed;
3.overflow不为visible;
4.display: table-cell | table-caption | inline-block | flex;

相邻的块级盒在垂直方向会发生margin合并

### 6.有哪些清除浮动的技术，都适用哪些情况？
1.使用clear: both;
2.利用BFC;

1.利用clear: both;
  a) 在浮动元素后新增元素，并赋予clear属性清除浮动;
  b) 利用伪元素，产生与浮动元素相邻的兄弟节点，并赋予clear: both;属性;
  但这种还是没解决margin合并的问题

2.利用BFC
  a) 在浮动元素的父节点赋予新属性，使得其产生新的BFC
  但这样会改变默认样式

  综合上面两种，比较好的清浮动的方法
  ```
  .clearfix::before,
  .clearfix::after{
    content: '';
    display: table;
    clear: both;
  }
  ```

### 7.请解释什么是雪碧图（css sprites），以及如何实现？
1.利用图片生成器将多张图片合并成一张图片，从而降低http请求的数量，提升网页加载速度。
2.具体实现在css中，可以利用css属性：width hegith background-image background-position background-size实现图片的显现

  补：提前加载资源，防止在需要时才在开始下载引发的问题，比如只出现在:hover伪类中的图片，不会出现闪烁。

### 8.解决不同浏览器的样式兼容性问题思路？
1.开始写样式的时候可以使用normalize.css或reset.css这些css类库重置部分浏览器默认样式；
2.使用autoprefixer增加不同浏览器的前缀；
3.针对有问题的浏览器写特殊的样式，使用服务端渲染的页面加载；
4.参考已经处理过这些问题的内库，例如bootstrap

### 9.如何为功能受限的浏览器提供页面？ 使用什么样的技术和流程？
1.优雅降级：保障老版的浏览器访问页面同样没有样式的问题；
2.渐进增强：使用新的浏览器支持的特性属性，增强用户体验；

3.查阅caniuse.com查看属性兼容性
4.使用autoprefixer自动添加浏览器前缀；
5.使用Modernizr进行特性检测（就是一个JavaScript类库来检测用户的UA，然后进行判断是否支持css的新特性);

### 10.有什么不同的方式可以隐藏内容（使其仅适用于屏幕阅读器）？
1.使用display: none;属性；
2.使用visiable: hidden属性；
3.使用opacity；
4.使用position: absolute; 四个方位值的任意一个为 -99999px;

5.对于block元素，使用text-indent: -9999px顶格写;
6.WAI-ARIA（网页无障碍应用技术）：如何增加网页可访问性的 W3C 技术规范。

### 11.你使用过栅格系统吗？偏爱哪一个？
使用过
基本上都是
1.结构：container row column 2.断点类型： 使用@media标签
bootstrap是目前市面上最流行的栅格化系统。

bootstrap 
fundation
toast
Semantic

### 12.你是否使用过媒体查询或移动优先的布局？

mobile first
@media 响应式布局

### 13.编写高效的 CSS 应该注意什么？
1.选择器的层级尽量少，因为这会影响到浏览器的匹配速度；
2.css类名尽量按一定的规则进行命名，比较推荐BEM命名法；
3.尽量减少浏览器的重排与重绘，例如改变元素的宽度和高度会导致，可以使用tranfrom属性进行代替；
  a) 不要在dom元素样式改变的时候做查询，例如查询offsetHegiht之类的，会导致渲染队列强制刷新;
  b) 改变dom元素的多个样式尽量合并在一起进行改变；(减少DOM访问，同时把强制渲染队列刷新的风险降为0)
  c) 对于新增的dom多个元素可以可以先将其脱离文档流，操作完成后再带入文档流，这样只会触发一次重排，用fragment元素；（var fragment = document.createDocumentFragment();)
  d) 对于有动画的元素尽量使用相对布局，脱离文档流，这样就不会引起重排乃至重绘；

### 14.使用 CSS 预处理的优缺点分别是什么？
优：
1.管理方便，提高css可维护性；
2.避免写重复代码；
3.嵌套的写法更加舒服，编写的时候更加舒服；
4.将css代码分为多个文件，减少了http请求，原来的虽然也可以，但需要每个css文件都是一个http请求；

缺：
1.需要配置预处理的环境；
2.重新编译需要花费时间；

### 15.如何实现一个使用非标准字体的网页设计？
使用@font-face定义字体，font-family属性引用字体；

### 16.说说伪元素以及它的用途？
伪元素是css的一种类型的选择器，但选择的元素不在dom树。
总共四种:
1.::first-line
2.::first-letter
3.::before
4.::after

### 17.说说你对盒模型的理解，以及如何告知浏览器使用不同的盒模型渲染布局?
1.盒模型是dom树中元素形成的一个矩形块，
2.其组成由 content padding border margin
  其中分为标准盒模型  

  标准盒模型和IE盒模型的区别

  margin重叠问题

### 18.介绍static relative absolute fixed sticky？
static：元素默认属性。按正常的文档流进行布局；
relative：相对对位的元素，绝对定位的元素相对进行定位；
absolute：相对绝对定位的元素，相对static以为第一个父元素进行定位；
fixed：相对窗口进行定位；
sticky: relateive + fixed 默认是相对于文档流进行布局 当超出其设置的范围时(top right bottom left)，为fixed

### 19.解释响应式与移动优先？
移动优先是基于响应式网页的一个好的实践概念。
一般情况下，网页在移动端会比桌面端简单，所以在做响应式网页的时候移动优先会保证很多不必要的代码，达到网站开发由简入繁的步骤。

### 20.响应式设计与自适应设计的区别？
响应式设计是一份代码可以在不同视口的浏览器中进行适配；
自适应设计是准备好几个固定断点的窗口进行开发。

### 21.你有没有使用过视网膜分辨率的图形？当中使用什么技术？
1.使用媒体查询 min-device-pixel-ratio 当dpr为高的分辨率时使用更高分辨率的图片；
2.用JS window.devicePixelRatio检测，修改IMG SRC的值

图像使用svg图标；

### 22.什么情况下，用translate()而不用绝对定位？什么时候，情况相反。
translate()是transfrom属性的一个值，使用translate不会触发重绘，而使用绝对定位会发生重绘，从而影响浏览器渲染的速度。




























