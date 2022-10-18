# HTML笔记总结



## 头部
```html
<!DOCTYPE html> <!--表示这是HTML5-->
<html>
	<head>
		<title> 标题 </title>
		<meta charset=utf-8> <!--编码格式为 UTF-8-->
        <base href="http://xxx/images/" target="_blank">
        <link rel="stylesheet" type="text/css" href="mystyle.css">
        <style type="text/css">
            body {background-color:yellow}
            p {color:blue}
		</style>
	</head>

	<body>
		浏览器显示的正文
	</body>
</html>
```

| 标签       | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| \<head\>   | 包含了所有的头部标签元素，可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息 |
| \<title\>  | 1. 定义了浏览器工具栏的标题<br />2. 当网页添加到收藏夹时，显示在收藏夹中的标题<br />3. 显示在搜索引擎结果页面的标题 |
| \<base\>   | 描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接，通常用于链接到样式表 |
| \<link\>   | 标签定义了文档与外部资源之间的关系                           |
| \<style\>  | 定义了HTML文档的样式文件引用地址，也可以直接添加样式来渲染 HTML 文档 |
| \<meta\>   | 描述了一些基本的元数据，数据也不显示在页面上，但会被浏览器解析 |
| \<script\> | 用于加载脚本文件                                             |

**meta标签使用实例：**

```html
<!--为搜索引擎定义关键词-->
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">

<!--为网页定义描述内容-->
<meta name="description" content="为网页定义描述内容">

<!--定义网页作者-->
<meta name="author" content="Guan">

<!--每30秒钟刷新当前页面-->
<meta http-equiv="refresh" content="30">
```





## 正文

```html
<h1>这是一个标题。</h1>
<h2>这是一个标题。</h2>
...
<h6>这是一个标题。</h6>

<hr><!--水平分割线-->

<p>这是一个段落<br/>这是一个段落<br/>这是一个段落</p>
```

| 标签      | 作用                                       |
| --------- | ------------------------------------------ |
| \<h1~h6\> | `<h1>`定义最大的标题。`<h6>`定义最小的标题 |
| \<hr/\>   | 水平线，可用于分隔内容                     |
| \<p\>     | 段落，浏览器会自动地在段落的前后添加空行   |
| \<br/\>   | 换行                                       |





## 文本

| 标签       | 作用                               |
| :--------- | :--------------------------------- |
| \<b\>      | **定义粗体文本**                   |
| \<strong\> | **定义加重语气**（和加粗效果一致） |
| \<i\>      | *定义斜体字*                       |
| \<em\>     | *定义着重文字*（和斜体效果一致）   |
| \<small\>  | <small>定义小号字</small>          |
| \<sub\>    | ~定义下标字~                       |
| \<sup\>    | ^定义上标字^                       |
| \<ins\>    | <ins>定义插入字</ins>              |
| \<del\>    | ~~定义删除字~~                     |
| \<code\>   | <code>定义计算机代码</code>        |
| \<kbd\>    | <kbd>定义计算机代码样本</kbd>      |
| \<q>       | <q>定义短的引用语</q>              |





## CSS

**内联样式**

```html
<p style="color:blue; margin-left:20px;">这是一个段落。</p>
```

**内部样式**

```html
<head>
    <style type="text/css">
        body {background-color:yellow;}
        p {color:blue;}
    </style>
</head>
```

**外部样式表**

```html
<head>
	<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```





## 多媒体

### 链接

```html
<a href="https://www.xxx.com/">链接文本</a>

<!--在新窗口打开文件-->
<a href="url" target="_blank" rel="noopener noreferrer">xxx</a>

<!--文档内跳转（书签）-->
<a id="tips">有用的提示部分</a>
<a href="#tips">访问有用的提示部分</a>
<a href="https://www.xxx.com/aaa.html#tips">访问有用的提示部分</a>

<!--图像链接-->
<a href="http://www.example.com/"><img src="URL" alt="替换文本"></a>
<!--邮件链接-->
<a href="mailto:webmaster@example.com">发送e-mail</a>
```

| 属性   | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| target | 定义被链接的文档在何处显示<br />\_blank：新窗口打开<br /> \_parent：在父窗口中打开链接<br /> \_self：默认，当前页面跳转<br />\_top：在当前窗体打开链接，并替换当前的整个窗体(框架页) |
| rel    | 规定当前文档与目标 URL 之间的关系。仅在 href 属性存在时使用  |

注意：链接文本不必一定是文本，图片或其他 HTML 元素都可以成为链接；请始终将正斜杠添加到子文件夹，如：`https://www.runoob.com`请写成`https://www.runoob.com/`

### 图像

```html
<img src="xxx.jpg/url" alt="图形失效时显示的文字" width="304" height="228">
```

### 音频

```html
<!--使用embed元素-->
<embed height="50" width="100" src="horse.mp3">

<!--使用object元素-->
<object height="50" width="100" data="horse.mp3"></object>

<!--使用audio元素-->
<audio>
  <source src="horse.mp3" type="audio/mpeg"> <!--mpeg文件用于Internet Explorer、Chrome以及Safari-->
  <source src="horse.ogg" type="audio/ogg"> <!--OGG文件用于Firefox和Opera-->
  Your browser does not support this audio format.
</audio>

<!--使用超链接-->
<a href="horse.mp3">Play the sound</a>
```

### 视频

```html
<!--使用embed元素-->
<embed src="intro.swf" height="200" width="200">

<!--使用object元素-->
<object data="intro.swf" height="200" width="200"></object>

<!--使用video元素-->
<video width="320" height="240">
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    <source src="movie.webm" type="video/webm">
    您的浏览器不支持 video 标签。
</video>

<!--使用超链接-->
<a href="intro.swf">Play a video file</a>
```





## 列表

### 无序列表

```html
<!--不排序的列表，不会标数字只是用圆点代替，ul缩进每层标记不一样会，有实心和空心等不同的标记-->
<!--disc: 实心圆	circle:空心圆	square:实心方块-->
<ul type="disc">
    <li>内容</li>
    <li>内容</li>
</ul>
```

### 有序列表

```html
<!--自动加数字列表-->
<ol start=开始的数字>
    <li>内容</li>
    <li>内容</li>
    <li>内容</li>
</ol>
```

### 自定义列表

```html
<!--是项目及其注释的组合，解释内容会缩进换行-->
<dl>
    <dt>词条名称</dt>
    <dd>解释...</dd>
</dl>
```

**提示：**列表项内部可以使用段落、换行符、图片、链接以及其他列表等等

### 表格

```html
<thead></thead> <!--表头，当表格数量很多的时候向下翻页的时候表头不会动（冻结窗口）-->
<tbody><tbody> <!--中间内容-->
<tfoot></tfoot> <!--脚注，也有冻结窗口的效果-->
```

```html
<!--使用例子-->
<table border="1"> <!--border表示单元格边框的粗细，如果没有，则不显示单元格边框-->
    <caption>标题</caption> <!--表格的标题-->

    <thead> <!--thead 中的内容会被当做表头（与<th>表示的表头有区别）-->
        <tr> <!--表格的第一行-->
            <th>第一个格子</th> <!--<th></th>表示表头，<td></td>表示非表头的内容-->
            <th>第二个格子</th>
            <th>第...个格子</th>
        </tr>
    </thead>

    <tr>  <!--第二行-->
        <td>第一个格子</td>
        <td>第二个格子</td>
        <td>第...个格子</td>
    </tr>

    <tr> <!--第三行-->
        <td colspan="3">XXX</td> <!--XXX会延展三个表格的位置（合并）-->
    </tr>
</table>
```





## 表单

```html
<form action="/form.asp">
    <!--文本框标题-->
    <label for="male">Male</label>
    <!--文本-->
    name: <input type="text" name="firstname"> <br>
    <!--密码-->
	password: <input type="password" name="pwd"> <br>
    <!--单选按钮（二选一）-->
    <input type="radio" name="sex" value="male">Male <br>
	<input type="radio" name="sex" value="female">Female <br>
    <!--复选按钮-->
    <input type="checkbox" name="vehicle" value="aaa">aaa <br>
	<input type="checkbox" name="vehicle" value="bbb">bbb <br>
    <!--提交按钮-->
    <input type="submit" value="Submit">
    
    <!--带包围框的表单-->
    <fieldset>
    	<legend>Personalia:</legend> <!--框的标题-->
        Name: <input type="text"><br>
        Email: <input type="text"><br>
        Date of birth: <input type="text">
  	</fieldset>
    
    <!--下拉框-->
    <select>
        <option value="volvo">Volvo</option>
        <option value="saab">Saab</option>
        <option value="mercedes">Mercedes</option>
        <option value="audi">Audi</option>
	</select>
</form>
```

| 属性         | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| \<form\>     | 包含表单元素的区域，表单本身并不可见                         |
| \<input\>    | 输入类型是由类型属性（type）定义的<br />text：普通文本<br />password：密码<br />radio：单选<br />checkbox：复选<br />submit：提交<br />file：文件 |
| \<textarea\> | 用于输入多行文本的文本框                                     |
| \<fieldset\> | 定义了一组相关的表单元素，并使用外框包含起来                 |
| \<legend\>   | 定义了\<fieldset\>元素的标题                                 |
| \<select\>   | 下拉选项列表                                                 |
| \<optgroup\> | 选项组                                                       |
| \<option\>   | 有选项的下拉列表                                             |
| \<output\>   | 定义一个计算结果                                             |
| \<datalist\> | 指定一个预先定义的输入控件选项列表                           |
| \<button\>   | 按钮                                                         |





## 布局和框架

| 属性     | 作用                                                     |
| :------- | :------------------------------------------------------- |
| \<div\>  | 定义了文档的区域，块级(block-level)，会产生换行          |
| \<span\> | 用来组合文档中的行内元素，内联元素(inline)，不会产生换行 |



### iframe

把另外的网页插入到你的网页中显示
```html
<iframe src="http..."></iframe>
```



## 其他

### 锚点

```html
<p id="flag"></p> <!--给当前段落添加锚点，名称为flag-->
<a href="#flag">CCC</a> <!--点击会跳到相应的锚点，要与<p></p>一起使用，可以跳到其他html文件中-->
```



### 区域跳转

```html
<img src="" usemap="#map">
<map name="map">
    <area shape="rect" coords="0,0,50,50" href="http..."> <!--在图片左上角坐标为(0,0)(50,50)的长方形区域内点击鼠标就会跳到链接的http页面-->
    <area shape="circle" coords="75,75,25" href="http..."/> <!--在图片上坐标为(75,75)半径为25的区域内，点击就会跳到相应的链接页面-->
</map>
```





## 参考内容

### 转义字符

| 显示结果 | 描述        | 实体名称           |
| :------- | :---------- | :----------------- |
|          | 空格        | \&nbsp;            |
| <        | 小于号      | \&lt;              |
| >        | 大于号      | \&gt;              |
| &        | 和号        | \&amp;             |
| "        | 引号        | \&quot;            |
| '        | 撇号        | \&apos; (IE不支持) |
| ￠       | 分          | \&cent;            |
| £        | 镑          | \&pound;           |
| ¥        | 人民币/日元 | \&yen;             |
| €        | 欧元        | \&euro;            |
| §        | 小节        | \&sect;            |
| ©        | 版权        | \&copy;            |
| ®        | 注册商标    | \&reg;             |
| ™        | 商标        | \&trade;           |
| ×        | 乘号        | \&times;           |
| ÷        | 除号        | \&divide;          |

### 字符集

如：ASCII、符号、Emoji、制表符、几何图形

https://www.runoob.com/charsets/html-charsets.html

### 内置颜色名

https://www.runoob.com/tags/html-colorname.html

### URL编码

包含编码参考手册和编码函数体验

https://www.runoob.com/tags/html-urlencode.html

### HTTP状态码

https://www.runoob.com/tags/html-httpmessages.html