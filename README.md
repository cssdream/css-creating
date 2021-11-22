# CSS 创作指南（Beta）

CSS 是一种领域语言（DSL），层叠与继承赋予了 CSS 优雅多姿的无限创造力。正是由于它如此「简单」，
我们需要一些规范来使其变得更加可依赖。在使用 CSS 的时候不要把它当做一种编程语言，应该把它当做
绘画或者创作。所以本文档不仅仅是一份 CSS 书写规范，更像是 CSS 创作的调色盘。

> 「作为成功的项目的一员，很重要的一点是意识到只为自己写代码是很糟糕的行为。如果将有成千上万人
使用你的代码，那么你需要编写最具明确性的代码，而不是以自我的喜好来彰显自己的智商。」  
—— Idan Gazit

## 目录

1. [缩进与换行](#indentation)
2. [注释](#comments)
3. [基本格式](#basic-format)
4. [垂直对齐](#vertical-alignment)
5. [省略](#ellipsis)
6. [缩写](#shorthand)
7. [书写顺序](#order)
8. [选择器使用规则](#selector-rule)
9. [字体](#fonts)
10. [其他](#other)
11. [预处理工具](#preprocessors)
12. [后处理工具](#postprocessors)
13. [代码组织](#organization)
14. [构建部署](#build)

[许可](#license)

<a name="indentation"></a>
## 一、缩进与换行

用空格好还是 TAB 好？4个空格还是2个空格好？这是永远的圣战，累觉不爱，本文档不做详细说明。  

但是无论如何，项目中应该保持统一的缩进风格，以利于代码的阅读，同时可以避免在 git 等版本管理工具中造
成冗余的 diff 信息，而且千万不要空格和制表符（TAB）混用。

本文档规定：

* **使用2个空格缩进。**
* **使用 Unix 风格换行符（LF）** 
  保证跨平台的一致性，[更多](https://github.com/cssmagic/blog/issues/22)。

* **删除行尾多余的空格**  
  因为这些空格通常是不必要的（JSHint 中会通过 ```trailing``` 来检测是否存在多余的空格）。

* **文件末尾增加一个空行**  
  没有空行在合并多个文件时可能会把上一个文件的最后一行与下一个文件的第一行合并为一行，特别是，JS 中如果没有这个空行而恰巧末尾没有写分号，整个文件可能就会报错了。

  扩展阅读：   
  * [为什么 C 语言源程序最后一行要是一个空行？](http://www.zhihu.com/question/20018991)     
  * [Why should files end with a newline?](http://stackoverflow.com/questions/729692/why-should-files-end-with-a-newline)

### 1. 如何保证统一的缩进风格？

Sublime Text 在新建工程的时候会生成 ```xxx.sumlime-project``` 文件，可以配置一些基本缩进和排除目录等，但遗憾的是无法与其他编辑器通用。

[cube.css](https://github.com/thx/cube/blob/gh-pages/cube.sublime-project) 中的示例：

```js
{
  "folders": [{
    "path": ".",
    "folder_exclude_patterns": ["node_modules", "_site"] // 排除目录
  }],
  "settings": {
    "tab_size": 4,
    "translate_tabs_to_spaces": true, // tab 转换为空格
    "ensure_newline_at_eof_on_save": true, // 保存时文件末尾自动增加一个空行
    "trim_trailing_white_space_on_save": true // 删除行尾多余的空格
  }
}
```

[EditorConfig](http://editorconfig.org/) 是一个帮助开发者在不同的编辑器中保持统一编码
风格的插件，支持了大部分流行的编辑器。它包括两部分：代码风格规则定义(```.editorconfig``` 文件)和支持此规则的编辑器插件。


### 2. 快速开始

 1. 在项目根目录新建一个 ```.editorconfig``` 文件，保存为 utf-8 格式。Windows 用户由于无法直接新建一个只有扩展名的文件，新建的时候在末尾多加一个点 即可（```.editorconfig.``` ），也可以在命令行（CMD）中使用 ```echo.>.editorconfig``` 来创建。    
   ![Windows 中创建 .editorconfig 文件示例](http://gtms03.alicdn.com/tps/i3/TB1shUbGFXXXXbfXpXXXdvPSXXX-380-287.gif)

 2. 编辑 ```.editorconfig``` 文件

 ```ini
 # css-creating coding style
 root = true
 
 # 为所有文件设置风格
 [*]
 charset = utf-8
 indent_style = space
 indent_size = 2
 end_of_line = lf
 trim_trailing_whitespace = true
 insert_final_newline = true
 
 # 为 Markdown 文件保留行尾空格
 [*.md]
 trim_trailing_whitespace = false

 ＃ 匹配多种类型的文件（逗号前后不能有空格）
 [*.{css,scss,sass}]
 indent_size = 2

 # 匹配多个指定的文件（逗号前后不能有空格）
 [{package.json,.travis.yml}]
 indent_style = space
 indent_size = 2
 ```

 4. 安装编辑器插件

<table>
  <tr>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jetbrains#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/appCode.png" title="AppCode">
        <div>AppCode</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/sindresorhus/atom-editorconfig#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/atom.png" title="Atom">
        <div>Atom</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/kidwm/brackets-editorconfig/">
        <img width="150" height="150" src="http://editorconfig.org/logos/brackets.png" title="Brackets">
        <div>Brackets</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-codeblocks#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/codeblocks.png" title="Code::Block">
        <div>Code::Block</div>
      </a>
    </td>
  </tr>

  <tr>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-emacs#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/emacs.png" title="Emacs">
        <div>Emacs</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-geany#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/geany.png" title="Geany">
      <div>Geany</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-gedit#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/gedit.png" title="Gedit">
        <div>Gedit</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/RReverser/github-editorconfig#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/github.png" title="GitHub (code viewer and editor)">
        <div>GitHub</div>
      </a>
    </td>
  </tr>

  <tr>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jetbrains#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/intellijIDEA.png" title="inteltdJ">
        <div>inteltdJ</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jedit#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/jedit.png" title="jEdit">
        <div>jEdit</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-notepad-plus-plus#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/notepad.png" title="Notepad++">
        <div>Notepad++</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jetbrains#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/phpStorm.png" title="PHPStorm">
        <div>PHPStorm</div>
      </a>
    </td>
  </tr>

  <tr>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jetbrains#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/pyCharm.png" title="PyCharm">
        <div>PyCharm</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jetbrains#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/rubyMine.png" title="RubyMine">
        <div>RubyMine</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/sindresorhus/editorconfig-sublime#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/sublimetext.png" title="Subtdme Text">
        <div>Subtdme Text</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/Mr0grog/editorconfig-textmate#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/textmate.png" title="TextMate">
        <div>TextMate</div>
      </a>
    </td>
  </tr>

  <tr>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-vim#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/vim.png" title="Vim">
        <div>Vim</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-visualstudio#readme">
        <img width="150" height="150" src="https://editorconfig.org/logos/visualstudio-pro.png" title="Visual Studio">
        <div>Visual Studio</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig">
        <img width="150" height="150" src="https://user-images.githubusercontent.com/2784308/49776537-566fdb80-fd37-11e8-8480-a27c0faf2b41.png" title="Visual Studio Code">
        <div>Visual Studio Code</div>
      </a>
    </td>
    <td align="center">
      <a traget="_blank" href="https://github.com/editorconfig/editorconfig-jetbrains#readme">
        <img width="150" height="150" src="http://editorconfig.org/logos/webStorm.png" title="WebStorm">
        <div>WebStorm</div>
      </a>
    </td>
  </tr>
</table>


### 3. EditorConfig 文档

 1. 通配符规则：
    
    ```*``` 匹配任意字符串，但不包括 ```/```

    ```**``` 匹配任何字符串

    ```?``` 匹配任何单字符

    ```[name]``` 匹配任何括号中的单字符

    ```[!name]``` 匹配任何非括号中的单字符

    ```{s1,s2,s3}``` 匹配任何给出的多个字符串

    说明：

    * ```[]``` 匹配规则是从当前目录算起。
    * ```?``` 只能匹配且必须有类似 f1.js、f2.js、f12.js 的文件，通过f??.js只能匹配到 f12.js，而f**.js可以匹配到所有。
    * ```{s1,s2,s3}``` 之间不能有空格。```{f1.js, f2.js, f3.js}``` 无法匹配```f2.js``` 和 ```f3.js```。
    * ```*``` 和 ```**``` 可以匹配空字符串，例如 ```f*.js``` 可以匹配到 ```f.js```。
    * 多个匹配之间的规则如果不冲突是可以合并的。
    * 优先级问题，如果两个匹配所定义的规则冲突，则会以最靠近打开文件的```.editorconfig``文件为准。
    如果同一个文件中匹配定义冲突，则会以最后定义的为准。**所以在定义规则的时候，须先定义通用规则，后定义特殊规则。**
 
 2. 属性规则（所有属性不区分大小写）

    
| 属性                      | 值                                              | 说明  |
| :--------                 | :-----                                          | :----  |
| root                      | true, false                                     | 设置是否是当前项目的根目录 |
| indent_style              | tab, space                                      | 设置缩进格式 |
| indent_size               | number                                          | 设置缩进大小 |
| tab_width                 | number                                          | 设置 tab 表示的空格数，默认等于indent_size，无需设定 |
| end_of_line               | lf(Unix \n), cr(Mac OS \r), crlf(Windows \r\n)  | 设置行尾换行符格式 |
| charset                   | atin1, utf-8, utf-8-bom, utf-16be, utf-16le 等  | 设置字符编码 |
| trim_trailing_whitespace  | true, false                                     | 设置是否自动删除行尾多余空格 |
| insert_final_newline      | true, false                                     | 设置是否在保存文件时自动在行尾插入空行 |

<a name="comments"></a>
## 二、注释

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

每个 CSS 文件的头部或区块的开始处应当包含 [Doxygen](http://zh.wikipedia.org/wiki/Doxygen) 风格的注释，以阐明该文件或这段代码的
作用、作者、最后更新时间等信息。


[Doxygen](http://zh.wikipedia.org/wiki/Doxygen) 风格的注释以 /** 开始，每行以 * 号开头。

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

 [Doxygen](http://zh.wikipedia.org/wiki/Doxygen) 风格的注释内如果包含其他代码，不写开头的 * 号，以方便复制代码。

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

### 4. CSS 预处理工具中的单行注释

Sass， LESS， Stylus 中可以使用单行注释。

```
// 注释内容

```

### 5. clean-css 等压缩工具中的注释

clean-css 是一个 CSS 压缩工具，为了保留 CSS 文件的版权信息等特殊需求，支持以下形式的注释

```
/*!
  这里是版权信息或者重要的注释，压缩后不会被删除
*/
```

<a name="basic-format"></a>
## 三、基本格式

### 1. CSS文件头部声明 `@charset`

为了避免 HTML 和 CSS 文件编码不同时造成中文解析乱码，造成的不必要的麻烦，CSS 文件头部统一加上文件对应的编码，例如文件编码为 `UTF-8` 时：

```css
@charset "UTF-8";
/* 开始书写样式 */
```

需要注意的是：

* `@charset` 前面不能有任何字符。
* `@charset` 必须出现在 CSS 文件的最开始。

>注：在使用 `SASS` 时如果不指定 `@charset` 也可能造成乱码。

### 2. 统一使用小写。

* 字体名称以及特殊的 CSS 属性/值（```translateX```等）不要求强制小写
* 颜色值如果是16进制，推荐小写，更加容易辨识。
* 如果是关键字色值，推荐使用更加直观的[颜色关键字](http://dev.w3.org/csswg/css-color-3/#svg-color)。

不推荐的写法：

```css
.Foo{
  BACKGROUND: #CCC;
  color: currentColor;
  border-color: #F00; /* 红色 */
  transform: translateX(20px);
}
```

推荐的写法：

```css
.foo{
  background: #ccc;
  color: currentColor;
  border-color: red;
  transform: translateX(20px);
}
```


### 3. 保持空格

留白是一种艺术，合理的留白（空格）可以更好的阅读代码

* 规则集的左大括号前保留一个空格
* 属性值前增加个空格(使用 [Emmet](https://emmet.io/) 会自动生成这个空格)
  ```css
  .selector {
    width: 200px;
    font-size: 22px;
  }
  ```

* 逗号后面保留一个空格。
  ```css
  .foo {
    color: rgba(0, 0, 0, .5);
    transition: color .3s, width .5s cubic-bezier(.6, 0, .2, 1);
    transform: matrix(0, 1, 1, 1, 10, 10);
  }

  @keyframes flash {
    0%, 50%, 100% {
      opacity: 1;
    }

    25%, 75% {
      opacity: 0;
    }
  }
  ```

* 属性值中的 `/` 分隔符前后保留一个空格。
  ```css
  .foo {
    font: 12px / 1.5 sans-serif;
    background: center / cover;
    grid-area: 1 / 1 / 4 / 2;
    grid-column: 2 / 3;
    grid-template: 100px 1fr / 50px fit-content(100px);
  }
  ```

* `calc()` 函数中的 `+, -, *, /` 运算符前后保留一个空格。
  
  错误的语法

  ```css
  .foo {
    width: calc(100%/3-2*1em+2*1px);
    margin: calc(1rem -2px) calc(1rem- 1px);
  }
  ```

  正确的语法

  ```css
  .foo {
    width: calc(100%/3 - 2*1em + 2*1px);
    margin: calc(1rem - 2px) calc(1rem - 1px);
  }
  ```

  注意：`*, /` 虽然不用加前后空格，但是为了统一，建议都加空格。

  推荐的写法：

  ```css
  .foo {
    width: calc(100% / 3 - 2 * 1em + 2 * 1px);
    margin: calc(1rem - 2px) calc(1rem - 1px);
  }
  ```

* Media Queries 中的 `>, <, >=, <=` 标识符前后保持一个空格。
  ```css
  @media (width >= 500px) and (width <= 1200px) {
    
  }

  @custom-media --only-medium-screen (width >= 500px) and (width <= 1200px);
  ```

* 其他未来 CSS 新增的语法以此类推，保留合适的空格。

### 4. 每个声明前保留一级缩进

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

### 5. 右大括号保持与该规则集第一个字符对齐

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

### 6. 多个选择器和声明都独占一行

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

### 7. 每个规则集之间保留一个空行

不推荐的写法：

```css
.selector1 {
  display: block;
  width: 100px;
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
  width: 100px;
}

.selector2 {
  padding: 10px;
  margin: 10px auto;
}
```

### 8. 统一使用双引号`""`

不推荐的写法：

```css
.foo:before {
  content: '';
}

input[type='checkbox'] {
  position: absolute;
}
```

推荐的写法：

```css
.foo:before {
  content: "";
}

input[type="checkbox"] {
  position: absolute;
}
```

### 9. 每增加一级花括号嵌套，则增加一级缩进

例如：

```css
@supports (animation: 1s foo both) {
  @keyframes foo {
    50% {
      transform: scale(1.2);
    }

    80% {
      transform: scale(0);
    }
  }
}
```

<a name="vertical-alignment"></a>
## 四、垂直对齐

### 1. `grid-template-areas` 属性值保持换行，并使用空格保持每列垂直对齐。

例如：

```css
.foo {
  grid-template-areas: "header header"
                       "nav    main"
                       "footer ....";

}
```

### 2. `grid`、`grid-template` 属性值保持换行，并使用空格保持每列垂直对齐。

例如：

```css
.foo {
  grid-template: [header-left] "header header" 30px [header-right]
                 [main-left]   "nav    main"   1fr  [main-right]
                 [footer-left] "nav    footer" 30px [footer-right]
                 / 120px 1fr;
  grid: "hd hd hd hd   hd   hd   hd   hd   hd"   minmax(100px, auto)
        "sd sd sd main main main main main main" minmax(100px, auto)
        "ft ft ft ft   ft   ft   ft   ft   ft"   minmax(100px, auto)
        / 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
}
```

### 3. 包含多个前缀的声明不强制对齐。

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

如果使用 CSS 预处理器或后处理器，推荐以冒号对齐，使代码更加美观。[autoprefixer](https://github.com/postcss/autoprefixer#visual-cascade) 中默认开启这种风格，请保证 ```cascade``` 参数为 true。

```css
.selector {
  -webkit-transition: .3s ease;
     -moz-transition: .3s ease;
      -ms-transition: .3s ease;
       -o-transition: .3s ease;
          transition: .3s ease;
}
```


### 4. 以逗号分隔的多属性值推荐放在多行，属性值开头保持一级缩进。

比如多个`box-shadow`、`background`、@font-face 中的 `src` 等，这有助于提高代码的可读性，且易于生成有效的 Diff。

```css
.selector {
  box-shadow: 
    1px 1px 5px #000,
    0 0 6px blue,
    2px 0 3px 5px #ccc inset;
  background-image:
    linear-gradient(to top right, green, blue),
    linear-gradient(130deg, pink, lightblue),
    linear-gradient(to right, blue, red);
  background-size:
    100px 20px,
    30px, 100%;
    cover;
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

<a name="ellipsis"></a>
## 五、省略

* 不推荐省略 0 值单位，原因如下：
  1. CSS 规范中可以省略单位只有 `[<length-percentage>](https://drafts.csswg.org/css-values-3/#typedef-length-percentage)`，其他比如角度单位 `deg` 在 Chrome 中可以省略，
  这是浏览器的 Bug。
  注：可以省略的单位包括：`%|em|ex|ch|rem|vw|vh|vmin|vmax|cm|mm|q|in|pt|pc|px` 以及 [CSS Values 4]() 新增的：`cap|ic|lh|rlh|vi|vb`
  
  2. 大部分编辑中都可以通过快捷键快速加减数值，如果省略了单位就会失去这个便捷性。

* 如无必要，省略 url 中的引号。
* 省略 ```font-family``` 内中文字体名称的引号。
* 不强制要求缩写属性，`font`，`background`，`margin`，推荐使用工具自动合并，比如 [clean-css](https://github.com/jakubpawlowicz/clean-css)、[cssnano](https://github.com/ben-eb/cssnano)。
* 不强制要求缩写颜色中的16进制写法。
* 不要省略选择器内最后一个声明末尾的分号。  
多人协作时，如果他人新增了其他代码很可能没有注意到上一行末尾没有写分号，导致 CSS 解析错误。

不推荐的写法：

```css
.selector {
  background: url("../foo.png");
  transform: rotate3d(1, 1, 1, 0);
  width: 100px
}
```

推荐的写法：

```css
.selector {
  background: url(../foo.png);
  transform: rotate3d(1, 1, 1, 0deg);
  width: 100px;
}
```

<a name="shorthand"></a>
## 六、缩写规则

待更新……

<a name="order"></a>
## 七、书写顺序

### 1. 不强制要求声明的书写顺序。

如果团队规范有要求，建议使用工具来自动化排序，比如 [CSScomb](http://csscomb.com/)，或者使用 @wangjeaf 开发的  [ckstyle](https://github.com/wangjeaf/ckstyle-node/)
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

### 2. 无前缀属性一定要写在最后

由于 CSS 后面的属性会覆盖前面的，无前缀属性写在最后可以保证浏览器一旦支持了，则用标准的无前缀属性来渲染。

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

<a name="selector-rule"></a>
## 八、选择器使用规则

### 1. 可以使用 `*` 通用选择器。

`*` 通用选择器效率低是一个误区，如有必要可以使用。测试文章[关于css通配符性能问题不完全测试](http://i.wanz.im/2012/01/03/performance_testing_about_css_universal_selector/)

例如：

```css
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box; 
}
```

### 2. 不要在选择器末尾使用 `*` 通用选择器。

CSS 选择器匹配规则是从右往左，例如：

```css
.mod .foo * {
  border-radius: 6px;
}
```

### 3. 如果是页面唯一的元素请使用 ID 选择器。

引用[为后代选择器和ID选择器而辩护](http://hax.iteye.com/blog/1850571)

> 我一直以来所主张的使用 id 的方式，其实就是 HTML5 新增元素的前身。2000 年时，我们没有 footer 元素，为了给该div中的内容赋以结构上的意义，我们这样写：div id="footer"。今天，根据人们访问我们网站所用的浏览器和设备，我们可以选择用 HTML5 的 footer 元素替代老方式。但若是我们不能使用 HTML5 元素，使用 id 也没有什么不对的。

但应避免使用多个 ID 选择器。

不推荐的写法：

```css
#header #search {
  float: right;
}
```

推荐的写法：

```css
#search {
  float: right;
}
```

### 4. 避免重复修饰选择器

在一定意义上，这会降低选择器性能。

不推荐的写法：

```css
div#search {
  float: right;
}

ul.nav {
  overflow: hidden;
}
```

推荐的写法：

```css
#search {
  float: right;
}

.nav {
  overflow: hidden;
}
```

### 5. 所有[组合选择器](https://drafts.csswg.org/selectors-3/#combinators)（`>, +, ~, >>`）前后保留一个空格（以空格表示的后代选择器除外）

不推荐的写法：

```css
.foo>.bar+div~#baz {
  color: blue;
}
```

推荐的写法：

```css
.foo > .bar + div ~ #baz {
  color: blue;
}
```

<a name="fonts"></a>
## 九、字体

待更新

<a name="other"></a>
## 十、其他

* 如果需要 CSS Hacks，需详细注明解决什么问题。
* 尽量避免使用 IE 中的 CSS filters。
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
/**
 * 这里 p 的行高直接继承父元素的行高，最终
   p { line-height: 50px; }
 */
p {
  font-size: 40px;
}
```

```css
.box {
  line-height: 150%;
  font-size: 20px;  
}
/**
 * p 的行高计算过程为：
 * 1. 先计算出父元素的行高（150% * 20px = 30px）
 * 2. 然后 p 继承父元素的行高，最终
   p { line-height: 30px; }
 */
p {
  font-size: 40px;
}
```

```css
.box {
  line-height: 1.5;
  font-size: 20px;  
}
/**
 * p 的行高计算过程为：
 * 1. 先继承父元素的 1.5（1.5 * 40px = 60px）
 * 2. 然后乘以 p 的 font-size，最终
   p { line-height: 60px; }
 */
p {
  font-size: 40px;
}
```

<a name="preprocessors"></a>
## 十一、预处理工具

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

<a name="postprocessors"></a>
## 十二、后处理工具

随着 PostCSS 的不断完善，各种插件如雨后春笋般的涌现出来，其中不乏一些很优秀的插件。

1. 面向未来的 CSS
    * [Autoprefixer](https://github.com/postcss/autoprefixer) 根据 [caniuse](http://caniuse.com/) 的数据自动增加前缀，解放双手的利器（🌟🌟🌟 推荐👍 ）
    * [postcss-cssnext](http://cssnext.io/) 支持大量浏览器未实现的标准特性（CSS 变量，嵌套等）（🌟🌟 推荐👍 ）


2. 格式化工具
    * [stylefmt](https://github.com/morishitter/stylefmt) 支持 CSS、SCSS 等多种语法，支持 stylelint（🌟🌟🌟 推荐👍 ）
    * [perfectionist](https://github.com/ben-eb/perfectionist) 支持一些 `stylefmt` 不支持的语法（🌟🌟 推荐）
    * [sublime-perfectionist](https://github.com/yisibl/sublime-perfectionist)
    * [sublime-stylefmt](https://github.com/dmnsgn/sublime-stylefmt)

3. Linter 工具
    * [stylelint](https://github.com/stylelint/stylelint)（🌟🌟🌟 推荐👍 ）
    * [CSSLint](https://github.com/CSSLint/csslint)
    * [scss-lint](https://github.com/brigade/scss-lint)

4. 压缩代码工具
    * [cssnano](https://github.com/ben-eb/cssnano) 后起之秀，支持规则更多（🌟🌟🌟 推荐👍 ）
    * [clean-css](https://github.com/jakubpawlowicz/clean-css) 老牌压缩工具，速度更快（🌟🌟🌟 推荐）
    * [CSSWring](https://github.com/hail2u/node-csswring)
    * [postcss-csso](https://github.com/lahmatiy/postcss-csso)
  
    附：[CSS 压缩工具对比](http://goalsmashers.github.io/css-minification-benchmark/)
5. 删除页面未使用 CSS
    * [uncss](https://github.com/giakki/uncss) 使用 PhantomJS 和 `document.querySelector` 动态筛除未使用的 CSS（🌟🌟🌟 推荐👍 ）
    * [css-razor](https://github.com/tscanlin/css-razor) 仅仅静态检测 HTML 中未使用的 CSS

<a name="organization"></a>
## 十三、代码组织

待更新……

<a name="build"></a>
## 十四、构建部署

待更新……


<a name="license"></a>
## 许可

MIT License

Copyright (c) 2013-2017 一丝(@yisibl)

新浪微博： http://weibo.com/jieorlin/
