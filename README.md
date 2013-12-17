# CSS 创作指南（Beta）

CSS 是一种领域语言（DSL），层叠与继承赋予了 CSS 优雅多姿的无限创造力。正是由于它如此「简单」，
我们需要一些规范来使其变得更加可依赖。在使用 CSS 的时候不要把它当做一种编程语言，应该把它当做
画画或者创作。所以本文档不仅仅是一份 CSS 书写规范，更像是 CSS 创作的调色盘。

> 「作为成功的项目的一员，很重要的一点是意识到只为自己写代码是很糟糕的行为。如果将有成千上万人
使用你的代码，那么你需要编写最具明确性的代码，而不是以自我的喜好来彰显自己的智商。」  
—— Idan Gazit


## 目录

1. [缩进](#indentation)
2. [注释](#comments)
3. [书写格式](#format)
4. [细节美化](#detail)
5. [省略](#ellipsis)
6. [书写顺序](#order)
7. [选择器](#selector)
8. [其他](#other)
9. [预处理工具](#preprocessors)
10. [代码组织](#organization)
11. [构建部署](#build)

[许可](#license)

<a name="indentation"></a>
## 1. 缩进

在项目中应该有一个统一的缩进风格，以避免不必要的麻烦，千万不要缩进时空格和制表符（TAB）混用。

* 推荐缩进使用4个空格。
* 删除行尾冗余的空格。

提示：通过编辑器的全局设置保持统一的风格

<a name="comments"></a>
## 2. 注释

良好的注释使代码更具有可读性和可维护性，尤其是多人协作的项目，不要让团队其他成员来猜测一段
代码的意图。

注释风格应当相对简洁，做如下规范：

* 区块注释放在单独一行。
* 保持注释内代码的缩进。
* 为了注释文字更具有可读性，合理控制每行的字数，推荐每行80个字符（40个汉字）以内。

提示：通过扩展 [emmet](http://emmet.io/) 等工具（例如[emmet-plus](https://github.com/yisibl/emmet-plus)）可以快速输出统一的注释风格。

CSS 文件中有如下几种注释：

### 1. 普通注释，注释文字左右各留一个空格。

```
/* 普通注释 */
```

### 2. 区块注释

注释横线的长度为80个字符，作为换行参考。

一级区块注释 

```
/* ==========================================================================
   一级区块注释
   ========================================================================== */
```

二级区块注释 

```
/* --------------------------------------------------------------------------
   二级区块注释
   -------------------------------------------------------------------------- */
```
### 3. Doxygen 风格的注释

每个 CSS 文件的头部或区块的开始处应当包含 [Doxygen](http://zh.wikipedia.org/wiki/Doxygen/) 风格的注释，以阐明该文件或这段代码的
作用、作者、最后更新时间等信息。


[Doxygen](http://zh.wikipedia.org/wiki/Doxygen/) 风格的注释以 /** 开始，每行以 * 号开头。

```
/**
 * Doxygen 风格的注释示例
 * @file：    文件信息
 * @author:   一丝
 * @update:   2013-11-5
 * @note:     注解
 * @doc:      相关文档
 *
 * 这里是更详细的描述，当然我们要把字数控制在每行80个字符（40个汉字）以内。如果
 * 一行写不下，需要另起一行。
 */
```

 [Doxygen](http://zh.wikipedia.org/wiki/Doxygen/) 风格的注释内如果包含其他代码，不写开头的 * 号，以方便复制代码。

```
 /**
 * Doxygen 风格的注释包含代码
 * 
 * 例如我们可以在注释中嵌入 HTML 代码，同样保持代码的缩进。
 *
    <div class="mod">
        <p>这个模块名叫 mod</p>
    </div>
 */
```
<a name="format"></a>
## 3. 书写格式

### 1. 统一使用小写。

包括颜色中的 16 进制写法，字体名称以及 `translatex`等。

```css
.Foo{
    font-family: 'Helvetica Neue', Tahoma, 'Hiragino Sans GB', sans-serif;
    color: #FFF;
    BACKGROUND: #CCC;
    -webkit-transform: translateX(20px);
}
```

推荐的写法：

```css
.foo{
    font-family: 'helvetica neue', tahoma, 'hiragino sans gb', sans-serif;
    color: #fff;
    background: #ccc;
    -webkit-transform: translatex(20px);
}
```

### 2. 保持空格

* 规则集的左大括号前保留一个空格
* 属性值前增加个空格(使用 Emmet 会自动生成这个空格)
* 逗号后面保留一个空格。

合理的空白（空格）可以更好的阅读代码，最终推荐的效果如下：

```css
.selector {
    width: 200px;
    font-size: 22px;
    color: rgba(0, 0, 0, .5);
    transition: color .3s, width .5s cubic-bezier(.6, 0, .2, 1);
    transform: matrix(0, 1, 1, 1, 10, 10);
}
```


### 3. 每个声明前保留一级缩进

不推荐的写法：

```css
h3 {
font-weight: bold;
}
```

推荐的写法：

```css
h3 {
    font-weight: bold;
}
```

### 4. 右大括号保持与该规则集第一个字符对齐

不推荐的写法：

```css
h3{
    font-weight: bold;
    }
```

推荐的写法：

```css
h3 {
    font-weight: bold;
}
```

### 5. 多个选择器和声明都独占一行

不推荐的写法：

```css
h1, h2, h3 {
    font-weight: normal; line-height: 1.5;
}
```

推荐的写法：

```css
h1,
h2,
h3 {
    font-weight: normal;
    line-height: 1.5;
}
```

### 6. 每个规则集之间保留一个空行

不推荐的写法：

```css
.selector1 {
    display: block;
    width: 100px
}
.selector2 {
    padding: 10px;
    margin: 10px auto;
}
```

推荐的写法：

```css
.selector1 {
    display: block;
    width: 100px
}

.selector2 {
    padding: 10px;
    margin: 10px auto;
}
```

<a name="detail"></a>
## 4. 细节美化

### 1. 选择器内只有一个声明时可以写在一行。

这样可以使得代码显得更加紧凑。

```css
h1 {font-size: 32px;}

h2 {font-size: 26px;}

h3 {font-size: 22px;}
```

### 2. 包含多个前缀的声明不强制对齐。

通常可以写作这样：

```css
.selector {
    -webkit-transition: .3s ease;
    -moz-transition: .3s ease;
    -ms-transition: .3s ease;
    -o-transition: .3s ease;
    transition: .3s ease;
}
```

在CSS预处理中推荐以冒号对齐，使代码更加美观。

```css
.selector {
    -webkit-transition: .3s ease;
       -moz-transition: .3s ease;
        -ms-transition: .3s ease;
         -o-transition: .3s ease;
            transition: .3s ease;
}
```


### 3. 较长的属性值推荐放在多行，属性值开头保持一级缩进。

比如多个` box-shadow` 和背景渐变等，这有助于提高代码的可读性，且易于生成有效的 Diff。

```css
.selector {
    box-shadow: 
        1px 1px 5px #000,
        0 0 6px blue,
        2px 0 3px 5px #ccc inset;
    background-image:
        linear-gradient(to top right, green, blue),
        linear-gradient(to right, blue, red);
}

@media
    only screen and (-o-min-device-pixel-ratio: 2/1), /* Opera */
    only screen and (min--moz-device-pixel-ratio: 2), /* Firefox 16 之前 */
    only screen and (-webkit-min-device-pixel-ratio: 2), /* WebKit */
    only screen and (min-resolution: 192dpi), /* 不支持dppx的浏览器 */
    only screen and (min-resolution: 2dppx) /* 标准 */
{
    .selector {

    }
}

@font-face {
    font-family: 'FontName'; /* IE9 */
    src: url('FileName.eot');
    src:
        url('FileName.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
        url('FileName.woff') format('woff'), /* Chrome,Firefox */
        url('FileName.ttf') format('truetype'), /* Chrome,Firefox,Opera,Safari,Android, iOS 4.2+ */
        url('FileName.svg#FontName') format('svg'); /* iOS 4.1- */
}
```

### 4. `@keyframes` 内的关键帧保留一级缩进。

```css
@keyframes foo {
    50% {
        -webkit-transform: scale(1.2);
        -moz-transform: scale(1.2);
        -ms-transform: scale(1.2);
        -o-transform: scale(1.2);
        transform: scale(1.2);
    }
}
```

<a name="ellipsis"></a>
## 5. 省略

* 如无必要，省略0值单位。
* 如无必要，省略小数前面的 0。
* 如无必要，省略 url 中的引号。
* 省略`font-family`内中文字体名称的引号。
* 不强制要求缩写属性，`font`，`background`，`margin` 等。
* 不强制要求缩写颜色中的16进制写法。
* 不建议省略选择器内最后一个声明末尾的分号。  
多人协作时，如果他人新增了其他代码很可能没有注意到上一行末尾没有写分号，导致不必要的麻烦。

不推荐的写法：

```css
.selector {
    display: block;
    width: 100px
}
```

推荐的写法：

```css
.selector {
    display: block;
    width: 100px;
}
```
<a name="order"></a>
## 6. 书写顺序

### 1. 不强制要求声明的书写顺序。

如果团队有需求，建议使用工具来自动化排序，比如 [CSScomb](http://csscomb.com/)。  
推荐以声明的特性作为分组，不同分组间保留一个空行，例如：

```css
.dialog {
    /* 定位 */
    margin: auto;
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;    

    /* 盒模型 */
    width: 500px;
    height: 300px;
    padding: 10px 20px;

    /* 皮肤 */
    background: #FFF;
    color: #333;
    border: 1px solid;
    border-radius: 5px;
}
```

### 2. 标准的属性写在最后

不推荐的写法：

```css
.foo {
    -webkit-border-radius: 6px;
    border-radius: 6px;
    -moz-border-radius: 6px;
}
```

推荐的写法：

```css
.foo {
    -webkit-border-radius: 6px;
    -moz-border-radius: 6px;
    border-radius: 6px;
}
```

<a name="selector"></a>
## 7. 选择器

### 1. 可以使用 `*` 通用选择器。

* 通用选择器效率低是一个误区，如有必要可以使用。测试文章[关于css通配符性能问题不完全测试](http://i.wanz.im/2012/01/03/performance_testing_about_css_universal_selector/)

例如：

```css
* {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
```

### 2. 不要在选择器末尾使用 `*` 通用选择器。

CSS选择器匹配规则是从右往左，例如：

```css
.mod .foo * {
    border-radius: 6px;
}
```
### 3. 如果是页面唯一的元素请使用 ID 选择器。

引用[为后代选择器和ID选择器而辩护](http://hax.iteye.com/blog/1850571)

    我一直以来所主张的使用id的方式，其实就是HTML5新增元素的前身。2000年时，我们没有footer元素，为了给该div中的内容赋以结构上的意义，我们这样写：div id="footer"。今天，根据人们访问我们网站所用的浏览器和设备，我们可以选择用HTML5的footer元素替代老方式。但若是我们不能使用HTML5元素，使用id也没有什么不对的。

但是避免在同一个选择器使用多个 ID

不推荐的写法：

```
#header #search {
    float: right;
}
```

推荐的写法：

```
#search {
    float: right;
}
```

<a name="other"></a>
## 8. 其他

* 如果需要 CSS Hacks，需详细注明解决什么问题。
* 尽量避免使用 IE 中的 CSS filters。
* 统一使用双引号「""」,如`content: ""`。
* 选择器中的属性值也加上双引号，如`input[type="checkbox"]`。
* `font-weight`普通字重使用`normal`，加粗使用`bold`。大部分字体只有两个字重，所以  
不建议使用容易混淆的数值表示方法。
* 如无特别精确的要求，推荐使用不带单位的`line-height`，这样当前元素的行高只与自身`font-size`成比例关系，使排版更加灵活。例如`line-height:1.5`
<strong>line-height: 1.5 ≠ line-height: 150%</strong> 

```html
<div class="box">
    <p>line-height</p>
</div>
```

```css
.box {
    line-height: 50px;
    font-size: 20px;
}

p {
    font-size: 40px;
}

/**
 * p 的行高计算过程为：
 * 直接继承父元素的行高，最终
   p { line-height: 50px; }
 */
```

```css
.box {
    line-height: 150%;
    font-size: 20px;
}

p {
    font-size: 40px;
}

/**
 * p 的行高计算过程为：
 * 1. 先计算出父元素的行高（150% * 20px = 30px）
 * 2. 然后 p 继承父元素的行高，最终
   p { line-height: 30px; }
 */
```

```css
.box {
    line-height: 1.5;
    font-size: 20px;
}

p {
    font-size: 40px;
}

/**
 * p 的行高计算过程为：
 * 1. 先继承父元素的 1.5（1.5 * 40px = 60px）
 * 2. 然后乘以 p 的 font-size，最终
   p { line-height: 60px; }
 */
```

<a name="preprocessors"></a>
## 9. 预处理工具

不同的 CSS 预处理工具有着不同的特性、功能以及语法。编码习惯应当根据使用的预处理工具进行扩展，
以适应其特有的功能。推荐在使用 SCSS 时遵守以下指导。

* 将嵌套深度限制在1级。对于超过2级的嵌套，给予重新评估。这可以避免出现过于详实的 CSS 选择器。
* 避免大量的嵌套规则。当可读性受到影响时，将之打断。推荐避免出现多于20行的嵌套规则出现。
* 始终将@extend语句放在声明块的第一行。
* 如果可以的话，将@include语句放在声明块的顶部，紧接着@extend语句的位置。
* 考虑在自定义函数的名字前加上x-或其它形式的前缀。这有助于避免将自己的函数和 CSS 的原生函数混淆，  
或函数命名与库函数冲突。

```css
.selector {
    @extend .other-rule;
    @include clearfix();
    @include box-sizing(border-box);
    width: x-grid-unit(1);
    // 其他声明
}
```

<a name="organization"></a>
## 10. 代码组织

<a name="build"></a>
## 11. 构建部署

使用 grunt 部署 CSS

<a name="license"></a>
## 许可

MIT License
Copyright (c) 2013 一丝
