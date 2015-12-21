# SVG学习笔记
------
## 2015-12-21
### 简单的示例[^code]
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
> * 属性version和属性baseProfile属性是必不可少的，供其它类型的验证方式确定SVG版本
> * 作为XML的一种方言，SVG必须正确的绑定命名空间 