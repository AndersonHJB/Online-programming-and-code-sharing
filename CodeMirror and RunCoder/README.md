
## CodeMirror编写自己的在线代码编辑器


### 前提

写这个的目的是因为之前项目里用到过 CodeMirror，觉得作为一款在线代码编辑器还是不错，也看到过有些网站用到过在线代码编辑，当然我不知道他们是用什么做的，这里我把公司项目里用到的那部分抽出来，单独写篇博客，并把抽出来的那部分代码提交到 GitHub 去（），以防日后可能会再次用到（没准毕业设计里可能用的到）。
<!-- more -->
### 简单介绍

CodeMirror 是一款在线的支持语法高亮的代码编辑器。官网： [http://codemirror.net/](http://codemirror.net/)

可能光看官网，第一眼觉得那些在线编辑器有点**丑**，反正第一眼给我的感觉就是这样子，但是经过自己的细调，也能打造出一款精美的在线代码编辑器。

官网可以把它下载下来。

下载后，解压开得到的文件夹中，lib 下是放的是核心库和核心 css，mode 下放的是各种支持语言的语法定义，theme 目录下是支持的主题样式。一般在开发中，添加 lib 下的引用和 mode 下的引用就够了。

![mark](http://ohfk1r827.bkt.clouddn.com/blog/171209/cJHjjadJ9E.png)

### 如何使用

下面两个是使用 Code Mirror 必须引入的：

```html
<link rel="stylesheet" href="codemirror-5.31.0/lib/codemirror.css"/>
<script src="codemirror-5.31.0/lib/codemirror.js"></script>
```

接下来要引用的就是在 mode 目录下编辑器中要编辑的语言对应的 js 文件，这里以 Groovy 为例：

```html
<!--groovy代码高亮-->
<script src="codemirror-5.31.0/mode/groovy/groovy.js"></script>
```

如果你想让 Java 代码也支持代码高亮，则需要引入我从网上下载下来的 clike.js（我已经放到我的 GitHub 去了）

```html
<!--Java代码高亮必须引入-->
<script src="codemirror-5.31.0/clike.js"></script>
```

引用的文件用于支持对应语言的语法高亮。

然后前面说了第一次进入 Code Mirror 官网，觉得那些编辑器比较丑，那可能是主题比较丑，我这里推荐一款还不错的主题，只需按照如下引入即可：

```html
<!--引入css文件，用以支持主题-->
<link rel="stylesheet" href="codemirror-5.31.0/theme/dracula.css"/>
```

如果你还想让你的编辑器支持代码行折叠，请按照如下进行操作：

```html
<!--支持代码折叠-->
<link rel="stylesheet" href="codemirror-5.31.0/addon/fold/foldgutter.css"/>
<script src="codemirror-5.31.0/addon/fold/foldcode.js"></script>
<script src="codemirror-5.31.0/addon/fold/foldgutter.js"></script>
<script src="codemirror-5.31.0/addon/fold/brace-fold.js"></script>
<script src="codemirror-5.31.0/addon/fold/comment-fold.js"></script>
```

是不是这样引入就好了呢，当然不是啦

### 创建编辑器

在实际项目中，一般都不会直接把 body 整个内容作为编辑器的容器。而最常用的，是使用 textarea。这里我在 <body> 里使用个 textarea，

```html
<!-- begin code -->
<textarea class="form-control" id="code" name="code"></textarea>
<!-- end code-->
```

接下来就是创建编辑器了。

```javascript
//根据DOM元素的id构造出一个编辑器
var editor = CodeMirror.fromTextArea(document.getElementById("code"), {
});
```

是不是有点单调？

没错，我还可以在里面给他设置些属性：（充分利用我一开始引入的那些文件）

```javascript
mode: "text/groovy",    //实现groovy代码高亮
mode: "text/x-java", //实现Java代码高亮
lineNumbers: true,	//显示行号
theme: "dracula",	//设置主题
lineWrapping: true,	//代码折叠
foldGutter: true,
gutters: ["CodeMirror-linenumbers", "CodeMirror-foldgutter"],
matchBrackets: true,	//括号匹配
//readOnly: true,        //只读
```

如果需要查看更多属性，可以去官网查找，目前我只用到这些属性！

---

如果你要设置代码框的大小该怎么做呢？

```javascript
editor.setSize('800px', '950px');     //设置代码框的长宽
```

另外，如果你想给代码框赋值，该怎么办呢？

```javascript
editor.setValue("");    //给代码框赋值
editor.getValue();    //获取代码框的值
```

如果你再想在其他地方设置新的属性，可以像下面这样写：

```javascript
editor.setOption("readOnly", true);	//类似这种
```

### 总结

上面就大概讲了下 Code Mirror 怎么使用。

我自我感觉还是可以的哈！

里面所有涉及的代码在 GitHub 里可以下载：[https://github.com/AndersonHJB/Online-programming-and-code-sharing)
