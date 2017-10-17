---
title: Susy 2 教程 — 入门篇
---
![](http://images.vrm.cn/2017/06/12/161558_c56d152a_104617.png)


## 关于Susy

Susy 是一款进行**栅格布局**的辅助工具，它让开发者摆脱了冗杂的数学计算，同时降低了样式与结构的耦合程度。它的能力正如官网的简介一样强大：

>Your markup, your design, your opinions, out math.


## 栅格布局

栅格设计的特点是重视比例、秩序、连续感和现代感。 以及对存在于版面上的元素进行规划、组合、保持平衡或者打破平衡，以便让信息可以更快速、更便捷、更系统和更有效率的读取；另外最重要的一点是，负空间的规划（即：留白）也是栅格系统设计当中非常重要的部分。—— **维基百科**

> 参考文献：http://sagashen.lofter.com/post/11545c_785887


## 安装与使用

Susy 是基于 Sass 开发的，严格上讲并不是一个框架，而是类库，起初，它的设计初衷是作为 Compass 平台的一部分。建议大家在 Compass 的环境下使用 Susy。

> Susy 并非一定要基于 Compass，它还支持 Webpack，Gulp，Grunt等自动化工具编译，详细可以查看[官网文档](http://susydocs.oddbird.net/en/latest/install/#webpack-and-npm)

```shell
#终端指令
gem install susy

#如果已有compass项目
compass install susy

#如果是新初始化compass项目
compass create --using susy <project name>
```



默认你项目的 `项目名/sass/` 文件夹下会生成一个 `_grids.scss` 文件

![输入图片说明](http://images.vrm.cn/2017/06/12/161637_3347f58a_104617.png)

打开 `_grids.scss` 文件，已经引入了 `susy`

```css
// Requirements
// ============

@import "susy";

$susy: (
  columns: 12,
  gutters: 1/4,
);
```


小试牛刀
-----------
```css
// sass
$susy: (
  container:960px,
  columns: 12,  // The number of columns in your grid
  gutters: 1/4, // The size of a gutter in relation to a single column
);

.container{
	@include container;
}

.sidebar{
	@include span(2);
}

.main{
	@include span(10)
}
```
 
```css
// output
.container {
  max-width: 960px;
  margin-left: auto;
  margin-right: auto;
}

.container:after {
  content: " ";
  display: block;
  clear: both;
}

.sidebar {
  width: 15.25424%;
  float: left;
  margin-right: 1.69492%;
}

.main {
  width: 83.05085%;
  float: left;
  margin-right: 1.69492%;
}

```

## 专业词释义

#### isolate 布局
> 针对不同浏览器对小数点的四舍五入处理修正，如图：

![输入图片说明](http://images.vrm.cn/2017/06/12/161704_0d77196c_104617.png)


#### gutter , row , container
> 沟，行，全局

![输入图片说明](http://images.vrm.cn/2017/06/12/161713_983f0dce_104617.png)


## 设置篇

### 默认配置

`$susy` 是 Susy载入后自动读取的一个全局配置变量。



```css
$susy: (
  // 从左至右
  flow: ltr,
  // 可选值： fluid | static , 网格的计算单位，fluid是百分号%，static是作用域的倍数em					
  math: fluid,					   
  // 可选值： float | isolate, float是浮动形式，不解释，isolate是一种百分比溢出的修复.
  // 详细看http://tylertate.com/blog/2012/01/05/subpixel-rounding.html
  output: float,
  // 可选值：before | after | split | inside | inside-static , 
  // before,after对应margin-right,margin-left,
  // split是padding-left&padding-right,
  // inside-static也是padding,但是单位是px,                    
  gutter-position: after,      
  // 可选值： <length> | auto , 总区域宽度，auto则以父元素的宽度为值     
  container: auto,                  
  // 可选值： left | center | right | <length> [*2]，总区域的水平对齐方式
  container-position: center,		
  // 列数
  columns: 4,						
  // 沟值
  gutters: .25,						
  // 可选值：<length> | false/null ，设定每列宽度，这个比较鸡肋，绝大部分情况下不用设置，除非需求奇葩。
  column-width: false,				
  // 可选值：border-box | content-box
  global-box-sizing: content-box,	
  // 可选值：from | to ,建议不要改这个值，当你改成from的时候，last方法将不起作用
  last-flow: to,					
  // debug，通过gradient制作的背景grid条，很棒的debug工具
  debug: (							
    image: hide,
    color: rgba(#66f, .25),
    output: background,
    toggle: top right,
  ),
  // 自定义 mixins
  use-custom: (                     
    background-image: true,
    background-options: false,
    box-sizing: true,
    clearfix: false,
    rem: true,
  )
);
```
### Layout [function/mixin]

可以转换map配置，有`mixin`和`function`两种用法：

#### `function`用法
- Format:	layout($layout)
- $layout:`<layout>`

```css
// input
$map: layout(auto 12 .25 inside fluid isolate);

// 终端打印一下
@debug $map;

//output
$map: (
  container: auto,
  columns: 12,
  gutters: .25,
  gutter-position: inside,
  math: fluid,
  output: isolate,
);

```

#### `mixin`用法
- Format:	layout($layout, $clean)
- $layout:	`<layout>`
- $clean:	`<boolean>`

```
@include layout(12 1/4);
// 默认情况下，这些新设置将添加到现有的全局设置中。使用$clean参数从头建立新的设置。
```

### With-layout [mixin]
可以通过此mixin设置多个栅格系统，而互相之间不冲突。

- Format:	with-layout($layout, $clean) { @content }
- $layout:`<layout>`
- $clean:	`<boolean>`
- @content:`Sass content block`

```css
@include with-layout(8 static) {
  // Temporary 8-column static grid...
}

// Global settings are restored...
```

### Susy-Get [function]
获取配置参数

- Format:	susy-get($key, $layout)
- $key:	Setting name
- $layout:	`<layout>`

```css
$large: layout(80em 24 1/4 inside);
$large-container: susy-get(container, $large);
// debug output : 80em
```

未完，待续……
