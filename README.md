# SVG学习笔记

------
## 入门
### 简单的示例
```xml
<svg version="1.1"
    baseProfile="full"
    width="300" height="200"
    xmlns="http://www.w3.org/2000/svg">
    <rect width="100%" height="100%" fill="red"/>

    <circle cx="150" cy="100" r="80" fill="green" />

    <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">SVG</text>
</svg>
```
效果如下：

![demo1](img/demo1.png)

#### 注意
> * 属性version和属性baseProfile属性是必不可少的，供其它类型的验证方式确定SVG版本
> * 作为XML的一种方言，SVG必须正确的绑定命名空间

### SVG文件的基本属性
1. SVG文件全局有效的规则是“后来居上”，越后面的元素越可见。
2. web上的svg文件可以直接在浏览器上展示，或者通过以下几种方法嵌入到HTML文件中：
    - 如果HTM是XHTML并且声明类型为application/xhtml+xml，可以直接把SVG嵌入到XML源码中
    - 如果HTML是HTML5并且浏览器支持HTML5，同样可以直接嵌入SVG。
    - 可以通过object元素引用SVG文件：
        ```xml
        <object data="image.svg" type="image/svg+xml" />
        ```
    - 可以使用iframe元素引用SVG文件：
        ```xml
        <iframe src="image.svg"></iframe>
        ```

### SVG坐标定位
> * 这种坐标系统是：以页面的左上角为(0,0)坐标点，坐标以像素为单位，x轴正方向是向右，y轴正方向是向下

下面的代码定义的画布尺寸为200\*200，viewBox属性定义了画布上可以显示的区域：从（0，0）开始，150\*100 ，于是形成了放大两倍的效果
```xml
<svg width="200" height="200" viewBox="0 0 100 100">
```
源代码：[demo1.svg](demo/demo1.svg)

2015年12月21日

## 填充和边框
### Fill和Stroke属性
#### 1. 上色
> * fill属性设置对象内部的颜色
> * stroke设置绘制对象的线条的颜色
> * fill-opacity控制填充色的不透明度
> * stroke-opacity控制描边的不透明度

```xml
<rect x="10" y="10" width="100" height="100" stroke="blue" fill="purple" fill-opacity="0.5" stroke-opacity="0.8" />
```
效果如下：

![demo2](img/demo2.png)

#### 2. 描边
除了颜色属性，还有其他一些属性用来控制绘制描边的方式
> * stroke-width属性定义了描边的宽度，描边是以路径为中心线绘制的
> * stroke-linecap属性控制边框终点的形状
    - butt用直边结束线段，它是常规做法，线段边界90度垂直于描边的方向，贯穿它的终点
    - square的效果差不多，但是会稍微超出实际路径范围，超出的大小有stroke-width控制
    - round表示边框的终点是圆角，圆角的半径也是由stroke-width控制的
> * stroke-linejoin属性控制两条描边线段之间，用什么方式连接
    - miter是默认值，表示用方形画笔在连接处形成尖角
    - round表示圆角连接，实现平滑效果
    - bevel连接处会形成一个斜接
> * stroke-dasharray属性可以将边框定义为虚线

##### 2.1 stroke-linecap示例
```xml
<line x1="40" x2="120" y1="20" y2="20" stroke="black" stroke-width="20" stroke-linecap="butt" />
<line x1="40" x2="120" y1="60" y2="60" stroke="black" stroke-width="20" stroke-linecap="square" />
<line x1="40" x2="120" y1="100" y2="100" stroke="black" stroke-width="20" stroke-linecap="round" />
```
效果如下：

![demo2-1](img/demo2-1.png)

##### 2.2 stroke-linejoin示例
```xml
<polyline points="40 260 80 220 120 260" stroke="black" stroke-width="20" stroke-linecap="butt" fill="none" stroke-linejoin="miter" />
<polyline points="40 340 80 300 120 340" stroke="black" stroke-width="20" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
<polyline points="40 420 80 380 120 420" stroke="black" stroke-width="20" fill="none" stroke-linecap="square" stroke-linejoin="bevel"/>
```
效果如下：

![demo2-2](img/demo2-2.png)

##### 2.3 stroke-dasharray示例
```xml
<path d="M 10 500 Q 50 400 100 500 T 190 500" stroke="black" stroke-linecap="round" stroke-dasharray="5,15,5" fill="none" />
<path d="M 10 500 L 190 500" stroke="red" stroke-linecap="round" stroke-width="1" stroke-dasharray="5,5" fill="none"/>
```
效果如下：

![demo2-3](img/demo2-3.png)

> * stroke-dasharray属性的参数，是一组用逗号分割的数字组成的数列。注意，和path不一样，这里的数字必须用逗号分割，虽然也可以插入空格，但是数字之间必须用逗号分开。每一组数字，第一个用来表示实线，第二个用来表示空白。所以在上面的例子里，第二个路径会先画5px实线，紧接着是5px空白，然后又是5px实线，从而形成虚线。如果你想要更复杂的虚线模式，你可以定义更多的数字。上面例子里的第一个，就定义了3个数字，这种情况下，数字会循环两次，形成一个偶数的虚线模式。所以该路径首先是5px实线，然后是10px空白，然后是5px实线，接下来循环这组数字，形成5px空白、10px实线、5px空白。然后这种模式会继续循环.

### 使用CSS
> * 除了定义对象的属性外，你也可以通过CSS来样式化填充和描边。语法和在html里使用CSS一样，只不过你要把background-color、border改成fill和stroke。注意，不是所有的属性都能用CSS来设置。上色和填充的部分一般是可以用CSS来设置的，比如fill，stroke，stroke-dasharray等，但是不包括下面会提到的渐变和图案等功能。另外，width、height，以及路径的命令等等，都不能用css设置
> * 可以通过内联样式或者引用外部样式文件来设置

源代码：[demo2.svg](demo/demo2.svg)

2015-12-22

## 渐变
有两种类型的渐变：线性渐变和径向渐变。必须给渐变内容指定一个id属性，否则文档内的其他元素就不能引用它。为了使渐变能被重复使用，渐变内容需要定义在**&lt;defs>**标签内部，而不是定义在形状上面。

### 线性渐变
线性渐变沿着直线改变颜色，要插入一个线性渐变，你需要在SVG文件的defs元素内部，创建一个&lt;linearGradient>节点。

基本示例：
```xml
<svg version="1.1"
    width="120"
    height="240"
    xmlns="http://www.w3.org/2000/svg">
    <defs>
        <linearGradient id="Gradient1">
            <stop class="stop1" offset="0%"/>
            <stop class="stop2" offset="50%" />
            <stop class="stop3" offset="100%" />
        </linearGradient>
        <linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1" >
            <stop offset="0%" stop-color="red" />
            <stop offset="50%" stop-color="black" stop-opacity="0" />
            <stop offset="100%" stop-color="blue" />
        </linearGradient>
        <style type="text/css">
            <![CDATA[
            #rect1 {fill: url(#Gradient1);}
            .stop1 {stop-color:red;}
            .stop2 {stop-color:black;stop-opacity:0;}
            .stop3{stop-color:blue;}
            ]]>
        </style>
    </defs>

    <rect id="rect1" x="10" y="10" rx="15" ry="15" width="100" height="100" />
    <rect x="10" y="120" rx="15" ry="15" width="100" height="100" fill="url(#Gradient2)" />
</svg>
```
显示效果：
![demo3-1](img/demo3-1.png)

这些结点通过指定位置的offset（偏移）属性和stop-color（颜色中值）属性来说明在渐变的特定位置上应该是什么颜色；可以直接指定这两个属性值，也可以通过CSS来指定他们的值，该例子中混合使用了这两种方法。例如：该示例中指明了渐变开始颜色为红色，到中间位置时变成半透明的黑色，最后变成蓝色。虽然你可以根据需求按照自己的喜好插入很多中间颜色，但是偏移量应该始终从0%开始（或者0也可以，百分号可以扔掉），到100%（或1）结束。如果stop设置的位置有重合，将使用XML树中较晚设置的值。而且，类似于填充和描边，你也可以指定属性stop-opacity来设置某个位置的半透明度
使用渐变时，我们需要在一个对象的属性fill或属性stroke中引用它，这跟你在CSS中使用url引用元素的方法一样。在本例中，url只是一个渐变的引用，我们已经给这个渐变一个ID——“Gradient”。要想附加它，将属性fill设置为url(#Gradient)即可。现在对象就变成多色的了，也可以用同样的方式处理stroke。
&lt;linearGradient>元素还需要一些其他的属性值，它们指定了渐变的大小和出现范围。渐变的方向可以通过两个点来控制，它们分别是属性x1、x2、y1和y2，这些属性定义了渐变路线走向。渐变色默认是水平方向的，但是通过修改这些属性，就可以旋转该方向。下例中的Gradient2创建了一个垂直渐变。

### 径向渐变
径向渐变与线性渐变相似，只是它是从一个点开始发散绘制渐变。创建径向渐变需要在文档的defs中添加一个&lt;radialGradient>元素

示例：
```xml
<radialGradient id="RadialGradient1">
    <stop offset="0%" stop-color="red" />
    <stop offset="100%" stop-color="blue" />
</radialGradient>
<radialGradient id="RadialGradient2" cx="0.25" cy="0.25" r="0.25">
    <stop offset="0%" stop-color="red" />
    <stop offset="100%" stop-color="blue" />
</radialGradient>
<rect x="10" y="230" rx="15" ry="15" width="100" height="100" fill="url(#RadialGradient1)" />
<rect x="10" y="370" rx="15" ry="15" width="100" height="100" fill="url(#RadialGradient2)" />
```
显示效果：

![demo3-2](img/demo3-2.png)
2015-12-23
径向渐变也是通过两个点来定义其边缘位置，两点中的第一个点定义了渐变结束所围绕的圆环，它需要一个中心点，由cx和cy属性及半径r来定义，通过设置这些点我们可以移动渐变范围并改变它的大小，如上例的第二个<rect>所展示的。

第二个点被称为焦点，由fx和fy属性定义。第一个点描述了渐变边缘位置，焦点则描述了渐变的中心，如下例.
```xml
<radialGradient id="RadialGradient3" cx="0.5" cy="0.5" r="0.5" fx="0.25" fy="0.25">
    <stop offset="0%" stop-color="red" />
    <stop offset="100%" stop-color="blue" />
</radialGradient>
<rect x="10" y="450" rx="15" ry="15" width="100" height="100" fill="url(#RadialGradient3)" />
<circle cx="60" cy="500" r="50" fill="transparent" stroke="white"  stroke-width="2"/>
<circle cx="35" cy="475" r="2" fill="white" stroke="white" />
<circle cx="60" cy="500" r="2" fill="white" stroke="white" />
<text x="38" y="480" fill="white"  font-family="sans-serif" font-size="10pt">(fx,fy)</text>
<text x="63" y="503" fill="white"  font-family="sans-serif" font-size="10pt">(cx,cy)</text>
```
显示效果：

![demo3-3](img/demo3-3.png)

源代码：[demo3.svg](demo/demo3.svg)
2015-12-24



