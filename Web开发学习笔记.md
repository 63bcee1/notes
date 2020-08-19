

# Web开发学习笔记

## HTML

> HTML（超文本标记语言），定义了网页内容的结构和含义，表现与展示效果用CSS 实现，功能与行为用Javascript实现。
>
> HTML 元素通过“标签”（tag）将文本从文档中引出，标签由在“`<`”和“`>`”中包裹的元素名组成，HTML 标签里的元素名不区分大小写。也就是说，它们可以用大写，小写或混合形式书写。
>
> **元素=开始标签+结束标签+内容**

### 块级元素

> 在页面中以块的形式呈现，相对于其他元素会出现在新的一行，其后的元素也会被挤到下一行。块级元素不会被嵌套进内联元素，但是可以嵌套在其他块级元素中。

### 内联元素

> 通常出现在块级元素中，并环绕文档内容的一小部分，不会导致文档换行。

空元素

> 不是所有元素都拥有开始标签结束标签和内容，一些元素只有一个标签，用于标识嵌入或者插入一些东西，例如`img`

### 属性

> 属性包含了元素的额外信息，但是不会现实在实际的内容中。
>
> 属性必须包含如下内容：

> 1. 一个空格，在属性和元素名称之间，如果已经有多个属性，就与前一个属性之间有一个空格。
> 2. 属性名称后跟着一个等号
> 3. 属性值由一对引号""引起来



> <a>标签是一个锚，被它包裹的内容将会成为一个超链接，href属性用于超链接的web地址；title属性声明额外的信息，鼠标悬停到超链接上，title信息将会显示；target用于指定链接如何呈现出来，target="_blank"将会在新的标签页中显示，如果希望停留在当前页，忽略这个属性。

### 剖析HTML文档

> `<!DOCTYPE html>`: 声明文档类型.
>
> `<html></html>`: `<html>`元素。这个元素包裹了整个完整的页面，是一个根元素
>
> `<head></head>`: `<head>元素`. 这个元素是一个容器，它包含了所有你想包含在HTML页面中但不想在HTML页面中显示的内容。这些内容包括你想在搜索结果中出现的关键字和页面描述，CSS样式，字符集声明等等。
>
> 而<meta charset="utf-8">: 这个元素设置文档使用utf-8字符集编码，utf-8字符集包含了人类大部分的文字。
>
> `<title></title>`: 设置页面标题，出现在浏览器标签上，当你标记/收藏页面时它可用来描述页面。
>
> `<body></body>`: `<body>`元素。 包含了你访问页面时所有显示在页面上的内容，文本，图片，音频，游戏等等。

**HTML空白**-在HTML中，无论连续使用多少 空白符，换行符，渲染的时候只会渲染为单独的空格符

**HTML特殊字符**-字符 `<`, `>`,`"`,`'` 和 `&` 是特殊字符

| 原义字符 | 等价字符引用 |
| -------- | ------------ |
| <        | \&lt;        |
| >        | \&gt;        |
| "        | \&quot;      |
| ‘        | \&apos;      |
| &        | \&amp;       |

**HTML注释**-注释用特殊的记号\<!--和-->包括起来

### HTML头部

> HTML头部是\<head>元素中的内容
>
> \<title>用于给文档添加标题
>
> 元数据：\<meta>元素，就是描述数据的数据。<meta charset="utf-8">，指定文档中字符编码；mate的name和content属性，name用于指定meta元素的类型，说明包含什么什么类型的信息，content指定了元素的实际内容；description搜索引擎显示的结果页中。

### 统一资源定位符

统一资源定位符（英文：**U**niform **R**esource **L**ocator，简写：URL）是一个定义了在网络上的位置的一个文本字符串。

### 绝对URL

> **绝对URL**：指向由其在Web上的绝对位置定义的位置，包括 [protocol](https://developer.mozilla.org/zh-CN/docs/Glossary/Protocol)（协议） 和 [domain name](https://developer.mozilla.org/zh-CN/docs/Glossary/域名)（域名）。像下面的例子，如果`index.html`页面上传到`projects`这一个目录。并且`projects`目录位于web服务站点的根目录，web站点的域名为`http://www.example.com`，那么这个页面就可以通过`http://www.example.com/projects/index.html`访问（或者通过`http://www.example.com/projects/`来访问，因为在没有指定特定的URL的情况下，大多数web服务会默认访问加载`index.html`这类页面）

### 相对URL

> **相对URL**：指向与您链接的文件相关的位置，更像我们在前面一节中所看到的位置。例如，如果我们想从示例文件链接`http://www.example.com/projects/index.html`转到相同目录下的一个PDF文件，URL就是文件名URL——例如`project-brief.pdf`——没有其他的信息要求。如果PDF文件能够在`projects`的子目录`pdfs`中访问到，相对路径就是`pdfs/project-brief.pdf`（对应的绝对URL是`http://www.example.com/projects/pdfs/project-brief.pdf`）

在下载超链接时，通过 **download**提供一个默认的保存文件名。

### 描述列表dl

> 用于标记一组项目及其相关描述，术语或者定义等。description list
>
> 每一项用dt元素闭合，description term，每个描述用dd闭合，description description
>
> 浏览器默认会在dd和dt之间产生缩进。

### 块引用

> 如果一个块级内容，从其他地方被引用，应该把它用&lt;blockquote&gt;元素包裹起来，并且在cite属性里，用URL指向引用的资源。

### 行内引用

> 行内引用使用q元素，也有cite属性。

### 缩略语

> abbr通常用来包裹一个缩写或者缩写语，在title属性中提供解释。

### 联系方式

> address元素，仅用于包含联系方式

### 上标和下标

> \<sup>和\</sup>表示上标
>
> \<sub>和\<sub>表示下标

### 代码展示

> code用于标记计算机通用代码
>
> pre用于保留空白符，如果要想空白符在渲染时不被忽略，就包含在pre标签中
>
> var用于标记变量名
>
> kbd用于标记电脑键盘输入
>
> samp用于标记程序输出

### 时间和日期

> time用于标记时间和日期
>
> ```html
> <time datetime="2016-01-20">2016年1月20日</time>
> ```

## 文档与网站架构

文档的基本组成部分

### 页眉

> 横跨页面顶部，大的标题或者标志，网站的一般性信息，通常存在于所有网页。

### 导航栏

> 导航栏，指向网站各个主要区段，通常用菜单按钮，超链接，标签页表示，类似于标题栏，通常应该在所有网页中保持一致。

### 主内容

> 当前网页所独有的内容，因页面而异。

### 侧边栏

> 一些外围信息、链接、引用、广告等。
>
> 还可以存在辅助导航系统

### 页脚

> 横跨底部的狭长区域，一般用于放置公共信息，如版权声明，联系方式，字体较小，内容为次要内容。

### 图片

> img元素用于把图片放到网页上，它是一个空元素，至少需要一个src属性来让他生效，src属性值指向图片的相对路径或者绝对路径。
>
> alt属性是对图片的文字描述，用于图片无法显示或者不能被看到的情况。
>
> height和width用于指定图片的高度和宽度
>
> title用于提供进一步的信息，鼠标悬停到图片上，会显示的内容
>
> 在HTML5中，figure和figcaption用于给图片创造一个语义容器，在标题和图片之间建立清晰的关联。
>
> ```html
> <figure>
>   <img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"
>      alt="一只恐龙头部和躯干的骨架，它有一个巨大的头，长着锋利的牙齿。"
>      width="400"
>      height="341">
>   <figcaption>曼彻斯特大学博物馆展出的一只霸王龙的化石</figcaption>
> </figure>
> ```
>
> 也可以用CSS的 background-image属性放置背景图片。



### 音频和视频

**video**

> 用于嵌入一段视频，src属性指向视频资源；controls属性标识用户可以控制视频和音频的回放功能。
>
> 当不使用src时，可以使用source标签，每个source都有一个type属性，表示视频格式，浏览器会播放第一个与视频格式相匹配的资源，
>
> width和height，用于控制视频的尺寸
>
> autoplay，视频和音频会自动播放，即使网页的其他部分还没有加载完
>
> loop，视频和音频会循环播放
>
> muted，使用时会默认关闭声音
>
> poster，指向一个图像URL，在视频播放前显示
>
> preload，用于缓冲较大的文件，三个值可选，none：不缓冲；auto：页面加载后缓冲媒体文件；metadata：仅缓存文件元数据

**audio**

> 与video的使用几乎完全一样
>
> 

## CSS

> 用于设计风格和布局
>
> 一门基于规则的语法，用于定义网页中特定元素的一组规则

### CSS语法

> 语法由选择器起头，它选择了需要添加样式的HTML元素
>
> 接着输入一对大括号{}，内部定义了一个或者多个形式为**属性:值**这样的声明，每个声明都指定了所选择的属性，后面是属性的值。

```css
h1 {
    color: red;
    font-size: 5em;
}

p {
    color: black;
}
```

### CSS概念

> CSS有一些最基本的规则，层叠，继承，优先级，这些决定CSS如何应用到HTML中。

#### 层叠

> 当两条同级别的的规则作用于同一个元素时，写到最后面的，就是实际使用的规则

#### 优先级

> 选择器的越具体，作用的优先级越高

#### 继承

> 作用于父元素上的一些属性可以被子元素继承，而一些则不能

##### 控制继承

> CSS为了控制继承，提供了四个通用的值。
>
> inhert，会使子元素和父元素属性相同，就是开启继承。
>
> initial，属性和浏览器的默认属性相同，如果浏览器没有设置，那么就会是inhert
>
> unset，将属性重置为自然值，如果是自然继承，就是inhert，否则就是initial

**!important**，用于修改特定属性的值，可以覆盖普通规则的层叠。覆盖**!important**的方法就是另一个**!important，**优先级更高，或者优先级相同且位置靠后。

### CSS选择器

> 用于指定网页上需要样式化的HTML元素。

#### 类型，类和ID选择器

> ```css
> # 指向所有h1元素
> h1 { }
> # 指向所有class为box的元素
> .box { }
> # 指向一个id为unique的元素
> #unique { }
> ```

#### 标签属性选择器

> 根据元素上某个标签的属性的存在以选择元素，或者判断元素属性的特定值来选择元素
>
> ```css
> # 指向所有有title属性的a元素
> a[title] { }
> # 指向href="https://example.com"的a元素
> a[href="https://example.com"] { }
> ```



#### 伪类与伪元素

用于样式化一个元素的特定状态

```css
# a元素鼠标悬停的时候被选中
a:hover { }
```

##### 伪类

| 选择器                                                       | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`:active`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:active) | 在用户激活（例如点击）元素的时候匹配。                       |
| [`:any-link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:any-link) | 匹配一个链接的`:link`和`:visited`状态。                      |
| [`:blank`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:blank) | 匹配空输入值的[``元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)。 |
| [`:checked`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:checked) | 匹配处于选中状态的单选或者复选框。                           |
| [`:current`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:current) | 匹配正在展示的元素，或者其上级元素。                         |
| [`:default`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:default) | 匹配一组相似的元素中默认的一个或者更多的UI元素。             |
| [`:dir`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:dir) | 基于其方向性（HTML`dir`属性或者CSS`direction`属性的值）匹配一个元素。 |
| [`:disabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:disabled) | 匹配处于关闭状态的用户界面元素                               |
| [`:empty`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:empty) | 匹配除了可能存在的空格外，没有子元素的元素。                 |
| [`:enabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:enabled) | 匹配处于开启状态的用户界面元素。                             |
| [`:first`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first) | 匹配[分页媒体](https://developer.mozilla.org/en-US/docs/Web/CSS/Paged_Media)的第一页。 |
| [`:first-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-child) | 匹配兄弟元素中的第一个元素。                                 |
| [`:first-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-of-type) | 匹配兄弟元素中第一个某种类型的元素。                         |
| [`:focus`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus) | 当一个元素有焦点的时候匹配。                                 |
| [`:focus-visible`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus-visible) | 当元素有焦点，且焦点对用户可见的时候匹配。                   |
| [`:focus-within`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus-within) | 匹配有焦点的元素，以及子代元素有焦点的元素。                 |
| [`:future`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:future) | 匹配当前元素之后的元素。                                     |
| [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover) | 当用户悬浮到一个元素之上的时候匹配。                         |
| [`:indeterminate`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:indeterminate) | 匹配未定态值的UI元素，通常为[复选框](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox)。 |
| [`:in-range`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:in-range) | 用一个区间匹配元素，当值处于区间之内时匹配。                 |
| [`:invalid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid) | 匹配诸如`<input>`的位于不可用状态的元素。                    |
| [`:lang`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:lang) | 基于语言（HTML[lang](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/lang)属性的值）匹配元素。 |
| [`:last-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-child) | 匹配兄弟元素中最末的那个元素。                               |
| [`:last-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-of-type) | 匹配兄弟元素中最后一个某种类型的元素。                       |
| [`:left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:left) | 在[分页媒体](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Pages)中，匹配左手边的页。 |
| [`:link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:link) | 匹配未曾访问的链接。                                         |
| [`:local-link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:local-link) | 匹配指向和当前文档同一网站页面的链接。                       |
| [`:is()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:is) | 匹配传入的选择器列表中的任何选择器。                         |
| [`:not`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:not) | 匹配作为值传入自身的选择器未匹配的物件。                     |
| [`:nth-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child) | 匹配一列兄弟元素中的元素——兄弟元素按照an+b形式的式子进行匹配（比如2n+1匹配元素1、3、5、7等。即所有的奇数个）。 |
| [`:nth-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-of-type) | 匹配某种类型的一列兄弟元素（比如，`<p>`元素）——兄弟元素按照an+b形式的式子进行匹配（比如2n+1匹配元素1、3、5、7等。即所有的奇数个）。 |
| [`:nth-last-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-child) | 匹配一列兄弟元素，从后往前倒数。兄弟元素按照an+b形式的式子进行匹配（比如2n+1匹配按照顺序来的最后一个元素，然后往前两个，再往前两个，诸如此类。从后往前数的所有奇数个）。 |
| [`:nth-last-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-of-type) | 匹配某种类型的一列兄弟元素（比如，`<p>`元素），从后往前倒数。兄弟元素按照an+b形式的式子进行匹配（比如2n+1匹配按照顺序来的最后一个元素，然后往前两个，再往前两个，诸如此类。从后往前数的所有奇数个）。 |
| [`:only-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:only-child) | 匹配没有兄弟元素的元素。                                     |
| [`:only-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:only-of-type) | 匹配兄弟元素中某类型仅有的元素。                             |
| [`:optional`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:optional) | 匹配不是必填的form元素。                                     |
| [`:out-of-range`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:out-of-range) | 按区间匹配元素，当值不在区间内的的时候匹配。                 |
| [`:past`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:past) | 匹配当前元素之前的元素。                                     |
| [`:placeholder-shown`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:placeholder-shown) | 匹配显示占位文字的input元素。                                |
| [`:playing`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:playing) | 匹配代表音频、视频或者相似的能“播放”或者“暂停”的资源的，且正在“播放”的元素。 |
| [`:paused`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:paused) | 匹配代表音频、视频或者相似的能“播放”或者“暂停”的资源的，且正在“暂停”的元素。 |
| [`:read-only`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:read-only) | 匹配用户不可更改的元素。                                     |
| [`:read-write`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:read-write) | 匹配用户可更改的元素。                                       |
| [`:required`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:required) | 匹配必填的form元素。                                         |
| [`:right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:right) | 在[分页媒体](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Pages)中，匹配右手边的页。 |
| [`:root`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:root) | 匹配文档的根元素。                                           |
| [`:scope`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:scope) | 匹配任何为参考点元素的的元素。                               |
| [`:valid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:valid) | 匹配诸如`<input>`元素的处于可用状态的元素。                  |
| [`:target`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target) | 匹配当前URL目标的元素（例如如果它有一个匹配当前[URL分段](https://en.wikipedia.org/wiki/Fragment_identifier)的元素）。 |
| [`:visited`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited) | 匹配已访问链接。                                             |

##### 伪元素

| 选择器                                                       | 描述                                                 |
| :----------------------------------------------------------- | :--------------------------------------------------- |
| [`::after`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after) | 匹配出现在原有元素的实际内容之后的一个可样式化元素。 |
| [`::before`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::before) | 匹配出现在原有元素的实际内容之前的一个可样式化元素。 |
| [`::first-letter`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-letter) | 匹配元素的第一个字母。                               |
| [`::first-line`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-line) | 匹配包含此伪元素的元素的第一行。                     |
| [`::grammar-error`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::grammar-error) | 匹配文档中包含了浏览器标记的语法错误的那部分。       |
| [`::selection`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::selection) | 匹配文档中被选择的那部分。                           |
| [`::spelling-error`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::spelling-error) | 匹配文档中包含了浏览器标记的拼写错误的那部分。       |

#### 后代选择器

> 用空格组合多个选择器，空格被称为组合符，读取的顺序是从右向左读

#### 子代选择器

> 用大于号表示，只会在选择直接子元素的时候匹配
>
> 继承关系更远的后代则不会匹配

#### 邻接选择器

> 用加号表示，选择在在继承关系上同级的紧随元素。

#### 通用兄弟选择器

> 用~表示，选择一个元素的兄弟元素，即使不相邻。

### 盒模型

> 在CSS中，所有元素都被一个个盒子包围着。
>
> 在CSS中，广泛使用两种盒子，**块级盒子**和**内联盒子**

块级盒子的行为表现

> 盒子会在内联的方向上扩展，并占据父容器的所有可用空间，绝大多数情况下会和父容器一样宽
>
> 每个盒子都会换行
>
> width和height属性可以发挥作用
>
> 内边距（padding），外边距（margin），边框（border）会将其他元素从当前盒子周围推开
>
> 

内联盒子的行为表现

> 盒子不会换行
>
> width和height属性不起作用
>
> 垂直方向上的内边距，外边距和边框会被应用，但是不会把其他处于inline状态的盒子推开
>
> 水平方向上的内边距，外边距和边框会被应用，而且会把其他处于inline状态的盒子推开

通过设置盒子的display属性，可以控制盒子的外部显示类型。

#### 盒模型组成部分

一个块级盒子，需要如下组成部分：

- **Content box**: 这个区域是用来显示内容，大小可以通过设置 `width`和 `height`
- **Padding box**: 包围在内容区域外部的空白区域； 大小通过 `padding`相关属性设置。
- **Border box**: 边框盒包裹内容和内边距。大小通过 `border`相关属性设置。
- **Margin box**: 这是最外面的区域，是盒子和其他元素之间的空白区域。大小通过 `margin` 相关属性设置。

##### 边框和背景

###### 背景

背景颜色

> `background-color`属性定义了CSS中任何元素的背景颜色。

背景图片

> `backgroun-image`属性允许在元素的背景中显示图像

背景平铺

> `background- repeat`用于控制图像的平铺行为。
>
> no-repeat，不重复
>
> repeat-x，水平重复
>
> repeat-y，垂直重复
>
> repeat，在两个方向上重复

背景大小

> `background-szie`设置大小或者百分比，调整背景图像的大小。
>
> cover，浏览器将使背景图像足够大，使它完全覆盖盒子区域，同时仍然保持宽高比，有可能会跳出盒子区域。
>
> contain，使图像的大小适合盒子内

背景图像位置

> `background-position`，指定背景图像在盒子中的位置，使用坐标，默认值是(0,0)

###### 边框

> boder，设置边框颜色，宽度和样式

###### 圆角

> border-radius，使用长度或者百分比，设置每个角的样式。

##### 文本

> 控制文本的书写方向属性，被称为书写模式

###### 书写模式

> writing-mode，三个备选值：
>
>  horizontal-tb，块流从上至下，对应的文本方向是横向。
>
> vertical-rl，块流从右向左，对应的文本方向是纵向。
>
> vertical-lr，块流从左向右，对应的文本方向是纵向。

##### 溢出

> overflow，控制元素溢出的方式
>
> 默认是visible，默认会看到溢出的内容
>
> 设置为hidden时，会隐藏掉溢出
>
> scroll，内容溢出时，会显示滚动条
>
> 还可以设置方向上的溢出：
>
> overflow-x，overflow-y。

### 文字

#### 字体



