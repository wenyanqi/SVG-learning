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




