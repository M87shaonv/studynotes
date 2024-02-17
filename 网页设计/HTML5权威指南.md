# HTML基础

```html
<code>  <!--开始标签，使用等宽字体显示-->
    ...元素中间是元素的内容
</code> <!--结束标签-->
```

元素是一种用来向浏览器说明文档内容的工具，元素名不区分大小写

元素的开始和结束标签之间并非一定要有内容。没有内容的元素称为空元素

用HTML元素控制内容呈现形式的做法如今受到强 烈反对。现在的观点是应该用HTML元素说明文档 内容的结构和含义，用CSS控·12制内容呈现给用户的形式

空元素可以更简洁地只用一个标签表示

```html
<code/> <!--开始标签和结束标签合二为一-->
```

有些元素**只能**使用一个标签表示，在其中放置任何内容都不符合HTML规范。这类元素称为**虚元素**

```html
<!--用单个开始标签表示虚元素-->
<hr>
hr是横线，浏览器知道hr是虚元素，所以不会等待其结束标签出现

<!--空元素结构表示虚元素-->
<hr />
```

> 当元素后面没有紧跟着一条注释，且该元素包含着一个非空或者其开始标签未曾省略的`body` 元素就可以省略元素的结束标签

**元素属性**

元素可以使用属性进行配置，属性只能用在开始标签或单个标签上，不能用于结束标签，属性有名称和值两部分

```html
<a href="/apples.html">apples</a>
这个元素用来生成超链接，点击超链接就会加载另一个HTML文档
href是属性名
""内是属性值
```

一个元素可以应用多个属性，这些属性间以一个或几个空格分隔，属性的顺序未作要求，全局属性和元素专有属性可随意交错

**布尔属性**

布尔属性不需要设定值，只需将属性名添加到元素中即可

```html
Enter your name: <input disabled>
input元素输入数据
disabled属性会阻止用户输入数据，去掉即可输入
为布尔属性指定一个空字符串""或属性名称字符串作为其值也可以阻止输入
Enter your name: <input disabled="">
Enter your name: <input disabled="disabled">
```

**自定义属性**

用户自定义属性必须以`data-`开头，也称作者定义属性 ，有时也称扩展属性

**外层结构**

HTML文档的外层结构由两个元素确定：`DOCTYPE` 和`html`

```HTML
<!DOCTYPE HTML> DOCTYPE元素让服务器明白其处理的是HTML文档，使用布尔属性HTML表达
<html>
    ...
</html>
```

**元数据**

元数据部分用来向浏览器提供文档的一些信息，元数据包含在`head`元素内部

```html
<head>
    <title>Example</title> title内容显示在窗口标题栏或选项卡上
</head>
```

`head`元素还能用来规定文档与外部资源的关系、定义内嵌CSS样式、放置和载入脚本程序

**内容**

`body`元素告诉浏览器该向用户显示文档的哪个部分

```html
<!DOCTYPE HTML>
<html>
    <head>
        <!-- metadata goes here -->
        <title>Example</title>
    </head>
    <body>
        <!-- content and elements go here -->
        I like <code>apples</code> and oranges.
    </body>
</html>
```

**元素关系**

HTML文档中元素之间有明确的关系，包含另一个元素的元素是被包含元素的父元素，上例代码中`body`就是`code`的父元素

一个元素可以拥有多个子元素，但只能有一个父元素

`code`是`html`的后代元素，具有同一个父元素的几个元素互为兄弟元素 ，`head` 元素和`body`元素就是兄弟元素

素间关系的重要性在HTML中随处可见，一个元素能以什么样的元素为父元素或子元素是有限制的

HTML5规范将元素分为三大类：

+ **元数据元素**metadata element

  用来构建HTML文档的基本结构，以及就如何处理文档向浏览器提供信息和指示

+ **流元素**flow element

+ **短语元素**phrasing element

  它们的用途是确定一个元素合法的父元素和子元素范围。短语元素是HTML的基本成分

  所有的短语元素都是流元素，但并非所有的流元素都是短语元素

有些元素无法归入上述三种类型，这些元素要么没有什么特别的含义，要么只能用在一些非常有限的情况下

**实体**

HTML文档中有些字符具有特殊含义，如果需要使用这些字符就需要使用实体代码来替代

```html
	实体名 实体编号
<	&lt;	&#60;
>	&gt;	&#62;
&	&amp;	&#38;
©	&copy;	&#168;
™	&trade; &#8482;
```

实体名和实体编号都可以使用

## HTML全局属性

用来配置所有元素共有的行为。全局属性可以用在任何一个元素身上，不过这不一定会带来有意义或有用的行为改变

`accesskey` 属性可以设定一个或几个用来选择页面上的元素的快捷键

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        form用于创建表单的元素
        <form>
<!--Windows系统上是同时按下Alt键和accesskey属性值对应的键-->
            Name: <input type="text" name="name" accesskey="n"/>
            <p/>
            Password: <input type="password" name="password" accesskey="p"/>
            <p/>
            指定类型为提交，它会提交当前表单内的数据
            <input type="submit" value="Log In" accesskey="s"/>
       </form>
    </body>
</html>
```



`class` 属性用来将元素归类，这样做通常是为了能够找出文档中的某一类元素或为某一类元素应用CSS样式

```html
  <body>
    a用于创建超链接的标签      
    <a class="class1 class2" href="http://apress.com">Apress web site</a>
    <p />自关闭写法，用于包含段落文本
href属性指定链接的目标地址，>后是链接可见文本     
    <a class="class2 otherclass" href="http://w3c.org">W3C web site</a>
  </body>
```

一个元素可以被归入多个类别，在`class` 属性值中提供多个用空格分隔的类名即可

`class`属性的一种利用方式是设计CSS样式时以所定义的一个或多个类作为应用目标：

```HTML
<style type="text/css">
    .class2 {
        background-color: grey;
        color: white;
        padding: 5px;
        margin: 2px;
    }

    .class1 {
        font-size: x-large;
    }
</style>
</head>

<body>
    <a class="class1 class2" href="http://apress.com">Apress web site</a>
    <p />
    <a class="class2 otherclass" href="http://w3c.org">W3C web site</a>
</body>
```

用`style`元素定义了两条样式，第一条用于属于 `class2`类的元素，第二条用于属于`class1`类的元素

脚本程序也可以利用`class`属性

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Example</title>
  </head>
  <body>
    <a class="class1 class2" href="http://apress.com">Apress web site</a>
    <p />
    <a class="class2 otherclass" href="http://w3c.org">W3C web site</a>
    <script type="text/javascript">
      var elems =
      document.getElementsByClassName("otherclass");
      for (i = 0; i < elems.length; i++) {
      var x = elems[i];
      x.style.border = "thin solid
      black";
      x.style.backgroundColor = "white";
      x.style.color = "black";
    </script>
  </body>
</html>

```

该例脚本程序找出所有属于`otherclass`类的元素并对其设置了一些样式



`contenteditable`是HTML5中新增加的属性，其用途是让用户能够修改页面上的内容

```html
<body>
  <p contenteditable="true">It is raining right now</p>
</body>
```

contenteditable属性用在一个p元素身上。该属性值设置为`true`时用户可以编辑元素内容，设置为`false`时则禁止编辑。如果未设定其值，那么元素会从父元素处继承该属性的值



dir 属性用来规定元素中文字的方向。其有效值有两个：`ltr` （从左到右的文字）和`rtl` （从右到左的文字）

```html
<body>
  <p dir="rtl">This is right-to-left</p>
  <p dir="ltr">This is left-to-right</p>
</body>
```

`hidden`是个布尔属性，表示相关元素当前毋需关注。 浏览器对它的处理方式是隐藏相关元素

```html
<script>
    var toggleHidden = function () {
        var elem =
            document.getElementById("toggle");
        if (elem.hasAttribute("hidden")) {
            elem.removeAttribute("hidden");
        } else {
            elem.setAttribute("hidden",
                "hidden");
        }
    }
</script>

<body>
    <button onclick="toggleHidden()">Toggle</button>
    <table>
        <tr>
            <th>Name</th>
            <th>City</th>
        </tr>
        <tr>
            <td>Adam Freeman</td>
            <td>London</td>
        </tr>
        <tr id="toggle" hidden>
            <td>Joe
                Smith</td>
            <td>New York</td>
        </tr>
        <tr>
            <td>Anne Jones</td>
            <td>Paris</td>
        </tr>
    </table>
</body>
```

`table`元素包含的一个`tr`元素（代表表格中的一行）设置了`hidden`属性。文档中还有一个`button`元素，按 下它所代表的按钮将会调用定义在`script`元素中的 JavaScript函数`toggleHidden` 。这段脚本程序的作用是：如果那个`tr`元素的`hidden`属性存在就将其删除，否则就添加该属性



`id`属性给元素分配一个唯一的标识符。这种标识符通常用来将样式应用到元素上或在JavaScript程序中用来选择元素

```html
<style>
    #w3clink {
        background: grey;
        color: white;
        padding: 5px;
        border: thin solid black;
    }
</style>

<body>
    <a href="http://apress.com">Apress web
        site</a>
    <p />
    <a id="w3clink" href="http://w3c.org">W3C
        web site</a>
</body> 
```

为了根据`id`属性值应用样式，需要在定义样式时使用 一个以`#`号开头后接`id`属性值的选择器

`id`属性还可以用来导航到文档中的特定位置。倘若有个名为example.html的文档中包含一 个`id`属性值为`myelement`的元素，那么使用 example.html#myelement这个URL即可直接导航至该元素。该URL的末尾部分（#元素id值）称为URL片段标识符



`lang`属性用于说明元素内容使用的语言

```html
<body>
    <p lang="en">Hello - how are you?</p>
    <p lang="fr">Bonjour - comment êtes-vous?</p>
    <p lang="es">Hola - cómo estás?</p>
</body>
```

使用`lang`属性的目的是让浏览器调整其表达元素内容的方式，还可以用来选择指定语言的内容，以便只显示用户所选语言的内容或对其应用样式



`spellcheck`属性用来表明浏览器是否应该对元素的内容进行拼写检查。这个属性只有用在用户可以编辑的元素上时才有意义

```html
<body>
    <textarea spellcheck="true">This is some
    mispelled text</textarea>
</body>
```

`true`启用拼写检查和`false`禁用拼写检查。拼写检查的实现方式则因浏览器而异



`style`属性用来直接在元素身上定义CSS样式（这是在`style`元素或外部样式表中定义样式之外的一种选择）

```html
<body>
    <a href="http://apress.com" style="background: grey; color:white;
    padding:10px">
        Visit the Apress site
    </a>
</body>
```



HTML页面上的键盘焦点可以通过按Tab键在各元素之间切换。用`tabindex`属性可以改变默认的转移顺序

```html
<form>
    <label>Name: <input type="text" name="name" tabindex="1" /></label>
    <p />
    <label>City: <input type="text" name="city" tabindex="0" /></label>
    </p>
    <label>Country: <input type="text" name="country" tabindex="2" /></label>
    </p>
    <input type="submit" tabindex="-4" />
</form>
```

`tabindex`的值为正数时就是按几次Tab键后选中；如果是零，那么该元素会按照其在HTML中出现的顺序参与Tab键导航，上例中`Country`是2，会在跳到`Country`后回到`City`元素；如果是负数则不会被选中



`title`属性提供了元素的额外信息。浏览器通常用这些东西显示工具提示

```html
<body>
    <a title="Apress Publishing" href="http://apress.com">Visit the Apress site</a>
</body>
```

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240212115638011-577277854.png)



# CSS基础

CSS层叠样式表用来规定HTML文档的呈现形式(外观和格式编排)

CSS样式由一条或多条以分号隔开的样式声明组成，每条声明包含着一个CSS属性和该属性的值，二者以冒号分隔

```html
background-color:grey; color:white
两条样式声明，属性:值
```

样式不是定义了就行，它还需要被应用，即告诉浏览器它要影响哪些元素。把样式应用到元素身上的各种方式中，最直接的是使用全局属性`style`定义样式：

```html
<body>
    <a href="http://apress.com" style="background-color:grey; color:white">
        Visit the Apress website
    </a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a href="http://w3c.org">
        Visit the W3C website</a>
</body>
```

`background-color`属性和`color`属性分别设置元素的背景和前景颜色

**使用文档内嵌样式**

直接对元素应用样式有它的用处，但是对于可能大量需要各种样式的复杂文档来说就显得缺乏效率。这样做不仅需要逐个元素设好样式，而且软件更新时还不得不逐个元素仔细搞好样式调整，很容易出错

可以换种方法，用`style`元素（不是`style`属性） 定义文档内嵌样式，通过CSS选择器指示浏览器应用样式

```html
<style type="text/css">
    a {选择器
        background-color: grey;
        color: white
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
    <p>I like <span>apples</span> and oranges.</p>
    <a href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

本例中的选择器为`a` ，它指示浏览器将样式应用到文档中的每一个`a`元素

一个`style`元素中可以定义多条样式

```html
<style type="text/css">
    a {
        background-color: grey;
        color: white
    }

    span {
        border: thin black solid;
        padding: 5px;
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

`span`表示浏览器将把样式应用到文档中所有`span`元素上，实现其`border` 和 `padding` 属性所规定的效果

+ `border`属性设置的是围绕目标元素的边框
+ padding`属性控制的则是目标元素与边框之间的间距

**应用多条样式**

如果有一套样式要用于多个HTML文档，那么与其在每 一个文档中重复定义相同的样式，不如另外创建一个独立的样式表文件。这种文件按惯例以`.css`为文件扩展 名，其中包含着用户的样式定义

```css
a {
    background-color: grey;
    color: white
}

span {
    border: thin black solid;
    padding: 10px;
}
```

使用`link`元素将这些样式导入

```html
<head>
    <title>Example</title>
    <link rel="stylesheet" type="text/css" href="styles.css">
    <!--rel：定义当前文档与被链接资源之间的关系。在这里它表示被链接的资源是一个样式表-->
    <!--type：指定被链接资源的MIME类型。对于CSS样式表，这通常是text/css-->
    <!--href：指定被链接资源的URL-->
    </link>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

文档想要链接多少样式表都行，为每个样式表使用一个`link`元素即可。如果不同样式表中的样式使用了相同的选择器，那么这些样式表的导入顺序很重要，在此情况下得以应用的是后导入的样式

**从其他样式表中导入样式**

一个样式表中想要导入多少别的样式表都可以，用`@import`语句将样式从一个样式表导入另 一个样式表，要注意该语句必须位于样式表顶端，样式表自已的样式定义不能出现在它之前，相同选择器的样式会被覆盖

```css
/*另一个css文件*/
@import "styles.css";
span {
border: medium black dashed;
padding: 5px;
}
```

该语句并不常用，浏览器处理`@import`语句的效率往往不如处理多个`link`元素并靠样式层叠解决问题

**声明样式表的字符编码**

在CSS样式表中可以出现在`@import`语句之前的只有`@charset`语句，用于声明样式表使用的字符编码

```css
@charset "UTF-8";
@import "styles.css";
span {
border: medium black dashed;
padding: 10px;
}
```

如果样式表中未声明所使用的字符编码，那么浏览器将使用载入该样式表的HTML文档声明的编码。 如果HTML文档也没有声明其编码，那么默认使用UTF-8

## 样式的层叠和继承

要想掌握样式表，弄清样式层叠和继承这两个概念是关键

**浏览器根据层叠和继承规则确定显示一个元素时各种样式属性采用的值**

每个元素都有一套浏览器呈现页面时要用到的CSS属性，对于每一个这种属性， 浏览器都需要查看一下其所有的样式来源

三种定义样式的方式：

+ 元素内嵌

+ 文档内嵌

+ 外部样式表

但样式还有另外两个来源

**浏览器样式**

也称用户代理样式，是元素未设置样式时浏览器应用在它身上的默认样式，这 些样式因浏览器而略有差异，不过大体一致

```html
<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

**用户样式**

大多数浏览器都允许用户定义自己的样式表，各种浏览器都有自己管理用户样式的方式

***浏览器要显示元素时求索一个CSS属性值的次序：***

1. 元素内嵌样式：用元素的全局属性`style`定义的 式
2. 文档内嵌样式：定义在`style`元素中的样式
3. 外部样式：用`link`元素导入的样式
4. 用户样式：用户自定义样式
5. 浏览器样式：浏览器应用的默认样式



```css
/*设置color值元素的内嵌样式*/
<a style="color: red"
href="http://apress.com">Visit the Apress
website</a>

/*定义在style元素中的样式*/
<style type="text/css">
a {
color: red;
}
</style>
/*然后是link导入的样式，用户样式和浏览器样式*/
```

**使用重要样式调整层叠次序**

把样式属性值标记为重要`important`，可以改变正常的层叠次序

```html
<style type="text/css">
    a {
        color: black !important;
<!--在样式声明后附上!important即可将对应属性值标记为重要。不管这种样式属性定义在什么地方，浏览器都会给予优先考虑 -->        
    }
</style>

<body>
    <a style="color:red" href="http://apress.com">Visit the Apress
        website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

**根据具体程度和定义次序解决同级样式冲突**

如果有两条定义于同一层次的样式都能应用于同一个元素，而且它们都包含着浏览器要查看的CSS属性值，为了判断该用哪个值，浏览器会评估两条样式的具体 程度，然后选中较为特殊的那条

1.  样式的选择器中`id`值的数目
2. 选择器中其他属性和伪类的数目
3. 选择器中元素名和伪元素的数目

```html
<head>
    <title>Example</title>
    <style type="text/css">
        a {
            color: black;
        }

        a.myclass {
            color: white;
            background: grey;
        }
    </style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a class="myclass" href="http://w3c.org">Visit the W3C website</a>
</body>
```

按a-b-c的形式，其中每一个字母依次代表上述三类特征的统计结果生成一个数字。 它不是一个三位数。如果对某个样式算出的a值最大， 那么它就是具体程度高的那个。只有a值相等时浏览器才会比较b值，此时b值较大的样式具体程度更高。只有a值和b值都分别相等时浏览器才会考虑c值。也就是说，1-0-0这个得分比0-5-5这个得分代表的具体程度更高

如果同一个样式属性出现在具体程度相同的几条样式中，那么浏览器会根据其位置的先后选择所用的值， 再后面的样式将沿用最后一个样式属性

```html
<head>
    <title>Example</title>
    <style type="text/css">
        a.myclass1 {
            color: black;
        }

        a.myclass2 {
            color: white;
            background: grey;
        }
    </style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C website</a>
</body>
```

**继承**

如果浏览器在直接相关的样式中找不到某个属性的值，就会求助于继承机制，使用父元素的这个样式属性值

```HTML
<head>
    <title>Example</title>
    <style type="text/css">
        p {
            color: white;
            background: grey;
            border: medium solid black;
        }
    </style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    没有span属性，其父元素为p
    <p>I like <span>apples</span> and oranges.
    </p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C website</a>
</body>
```

**并非所有CSS属性均可继承**，与元素外观（文字颜色、字体等） 相关的样式会被继承，与元素在页面上的布局相关的 样式不会被继承

在样式中使用`inherit`这个特别设立的值可以强行实施继承，明确指示浏览器在该属性上使用父元素样式中的值

```html
<style type="text/css">
    p {
        color: white;
        background: grey;
        border: medium solid black;
    }

    span {
        border: inherit;
        /* 用于span元素的样式，其border属性值继承自父元素
现在span元素和包含它的p元素均具有边框*/
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C website</a>
</body>
```

## CSS中的颜色

在CSS中设置颜色有好几种方法，最简单的办法是使用规定的颜色名称或者设置红、绿、蓝三种颜色成分的值（十进制或十六进制）

设置颜色成分值时，十进制值以逗号分隔， 十六进制值前面通常要加上一个`#`符号，例如`#ffffff`， 它代表白色

```css
名字	十六进制 十进制
black #000000 0,0,0
white #FFFFFF 255,255,255
```

颜色名称和简单的十六进制数不是表示颜色的唯一方 式，CSS中还可以用一些函数选择颜色

```css
rgb(r, g,b)
/*用RGB模型表示颜色*/
color: rgb(112, 128,144) 
rgba(r,g, b, a) 
/*用RGB模型表示颜色
外加一个用于表示透明度的α值（0代表全透明，1代表完全不透明）*/
color: rgba(112, 128,144, 0.4)
hsl(h, s,l)
/*用HSL模型（色相hue、饱和度saturation和明度lightness）表示颜色*/
color:
hsl(120, 100%,22%)
hsla(h,s, l, a)
/*与HSL模式类似，只不过增加了一个表示透明度的α值*/
color:
hsla(120,100%, 22%,0.4)
```

## CSS中的长度

许多CSS属性要求为其设置长度值 ，`width`属性设置元素的宽度，`font-size`属性设置元素内容的字号

```css
<style type="text/css">
    p {
        background: grey;
        color: white;
        /*元素宽度*/
        width: 5cm;
        /*内容字号*/
        font-size: 20pt;
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and oranges.
    </p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C website</a>
</body>
```

设置长度值时，数字和单位标识符连在一起，二者之间不加空格或其他字符

CSS规定两种类型的长度单位，一种是绝对单位，另一种是与其他属性挂钩的相对单位

***绝对单位***

```css
in /*英寸*/
cm /*厘米*/
mm /*毫米*/
pt /*磅 1磅等于1/72英寸*/
pc /*pica 1pica等于12磅*/
```

一条样式中可以混合使用多种单位，包括混合使用绝对单位和相对单位

***相对单位***

```css
em /* 与元素字号挂钩 */
ex /* 与元素字体的x高度挂钩，1ex大致等于0.5em */
rem /* 与根元素的字号挂钩 */
px /* CSS像素（假定显示设备的分辨率为96dpi） */
% /* 另一属性的值的百分比 */
```
使用相对单位：
```html
<style type="text/css">
    p {
        background: grey;
        color: white;
        font-size: 15pt;
        /*p元素在屏幕上显示出来的高度应为字号的两倍*/
        height: 2em;
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and
        oranges.</p>
    <p style=" font-size:12pt">I also like
        mangos and cherries.</p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

相对单位还可以用来表示另一个相对单位的倍数，使用从另一相对单位推算出来的相对单位：

```html
<style type="text/css">
    html {
        /*将html元素的字号设置为0.2英寸*/
        font-size: 0.2in;
    }

    p {
        background: grey;
        color: white;
        /*rem单位根据html元素（文档的根元素）的字号而定*/
        font-size: 2rem;
        /*2倍字号*/
        height: 2em;
    }
</style>
</head>

<body style="font-size: 14pt">
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and
        oranges.</p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

百分比单位：把一个度量单位表示为其他属性值的百分比

```html
<style type="text/css">
    p {
        background-color: grey;
        color: white;
        font-size: 200%;
        width: 50%;
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and
        oranges.</p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

使用百分比单位会遇到两个麻烦：一是并非所有属性都能用这个单位。二是对于能用百分比单位的属性，百分比挂钩的其他属性各不相同

对于`font-size`属性，挂钩的是元素继承到的`font-size`值；而对于`width`属性，挂钩的则是元素的包含块的宽度

**用算式作值**

允许将CSS属性值设置为算式是CSS3定义的一个引人关注的特性

```html
<title>Example</title>
<style type="text/css">
    p {
        background: grey;
        color: white;
        font-size: 20pt;
        width: calc(80% - 20px);
    }
</style>
</head>

<body>
    <a href="http://apress.com">Visit the
        Apress website</a>
    <p>I like <span>apples</span> and
        oranges.</p>
    <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C
        website</a>
</body>
```

算式包含在关键字`calc`之后的括号之中。在其中可以混合使用各种单位进行基本的算术运算

**其他CSS单位**

*角度单位*

```css
deg /* 度（取值范围：0deg～360deg） */
grad /* 百分度（取值范围：0grad～400grad） */
rad /* 弧度（取值范围：0rad～6.28rad）*/
turn /* 圆周（1turn等于360deg） */
```

*时间单位*

```css
s /*秒*/
ms /*毫秒（1s等于1000ms）*/
```

# JavaScript基础

在HTML文档中定义脚本有几种方法可供选择

+ 定义内嵌脚本：脚本是HTML文档的一部分
+ 定义外部脚本：脚本包含在另一个文件中，通过一个 URL引用
+ 这两种方法都要用到`script`元素

```html
<!--内嵌脚本-->
<body>
    <script type="text/javascript">
        document.writeln("Hello");
    </script>
</body>
```

`script`元素位于文档中其他内容之后，这样一来在脚本执行之前浏览器就已经对其他元素进行了解析

`document.writeln`方法显示脚本的结果，这个方法只是在HTML文档中加入一行文字

JavaScript的基本元素是语句，一条语句代表着一条命令，通常以分号`;` 结尾，但分号可以省略

```html
<body>
    <script type="text/javascript">
        document.writeln("This is a statement");
        document.writeln("This is also a statement");
    </script>
</body>
```

浏览器依次执行每条语句，只是输出两条信息

**定义和使用函数**

可以把几条语句包装在一个函数中，浏览器只有在遇到一条调用该函数的语句时才会执行它

```html
<body>
    <script type="text/javascript">
        function myFunc() {
            document.writeln("This is a statement");
        };
        myFunc();
    </script>
</body>
```

大括号内就是函数的代码块，JavaScript是区分大小写的语言，因此`function`这个关键字必须小写

**定义带参数的函数**

```HTML
<body>
    <script type="text/javascript">
        function myFunc(name, weather) {
            document.writeln("Hello " + name +
                ".");
            document.writeln("It is " +
                weather + " today");
        };
        myFunc("Adam", "sunny");
    </script>
</body>
```

这里为`myFunc`函数添加了两个参数：`name`和`weather`，JavaScript是门弱类型语言，所以定义函数的时候不必声明参数的数据类型

> 调用函数时提供的参数数目不必与函数定义中的参数数目相同。如果提供的参数值更少，那么所有未提供值的参数的值均为`undefined` 
>
> 如果提供的参数值更多，那么多出的值会被忽略
>
> 因此要想定义两个同名但参数数目不同的函数，然后让JavaScript根据 调用函数时提供的参数值数目确定所调用的函数是不可能的
>
> 如果定义了两个同名的函数，那么第二个定义将会取代第一个

**定义返回结果的函数**

```html
<body>
    <script type="text/javascript">
        function myFunc(name) {
            return ("Hello " + name + ".");
        };
        document.writeln(myFunc("Adam"));
    </script>
</body>
```

定义这个函数时不用声明它会返回结果，也不用声明结果的数据类型

## 使用变量和类型

定义变量要使用`var`关键字，在定义的同时还可以像在一条单独的语句中那样为其赋值

+ 定义在函数中的变量称为局部变量 ，只能在该函数范围内使用
+ 在`script`元素中定义的变量称为全局变量 ，可以在任何地方使用，包括在其它脚本中

```html
<body>
    <script type="text/javascript">
        var myGlobalVar = "apples";
        function myFunc(name) {
            var myLocalVar = "sunny";
            return ("Hello " + name + ". Today is " + myLocalVar + ".");
        };
        document.writeln(myFunc("Adam"));
    </script>
    <script type="text/javascript">
        document.writeln("I like " + myGlobalVar);
    </script>
</body>
```

弱类型语言不代表它没有类型，是指不用明确声明变量的类型以及可随意的使用同一变量表示不同类型的值

JavaScript根据赋给变量的值确定其类型，还可以根据使用场景在类型间自由转换

JavaScript定义的基本类型：字符串`string` 、数值`number`、布尔`boolean`

*字符串类型*

`string`值可以用夹在一对双引号或单引号之间的一串字符表示

```javascript
var firstString = "This is a string";
var secondString = 'And so is this';
```

表示字符串时使用的引号要匹配，字符串前端用单引号而后端用双引号是不行的

*布尔类型*

```javascript
var firstBool = true;
var secondBool = false;
```

*数值类型*

实数都用`number`类型表示

```javascript
var firstnumber = 10
var hexvalue = 0xffff
var pi = 3.14
```

定义`number`类型变量时不必言明所用的是哪种数值



**创建对象**

有多种方法可以用来创建对象

```html
<body>
    <script type="text/javascript">
	<!--new Object()创建一个对象并赋给myData变量-->
        var myData = new Object();
        <!--变量名.属性 = 值-->
        myData.name = "Adam";
        myData.weather = "sunny";
        document.writeln("Hello " + myData.name + ". ");
        document.writeln("Today is " + myData.weather + ".");
    </script>
</body>
```

**使用对象字面量**

对象字面量的方式可以一次性定义一个对象及其属性

```js
var myData = {
    name: "Adam",
    weather: "sunny",
};
```

**将函数用做方法**

对象可以添加属性，也能添加函数。属于一个对象的函数称为其方法

```html
<body>
    <script type="text/javascript">
        var myData = {
            name: "Adam",
            weather: "sunny",
            printMessages: function () {
                document.writeln("Hello "
                    + this.name + ". ");
                document.writeln("Today is" + this.weather + ".");
            }
        };
        myData.printMessages();
    </script>
</body>
```

在方法内部使用对象属性时要用到`this`关键字。函数作为方法使用时，其所属对象会以关键字`this`的形式作为一个参数被传给它

**使用对象**

读取和修改属性值

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
    };
    myData.name = "Joe";
    myData["weather"] = "raining";
    document.writeln("Hello " + myData.name + ".");
    document.writeln("It is " + myData["weather"]);
</script>
```

第一种是直接以对象名.属性名赋值的形式更改，第二种用变量名当作索引然后赋值

**枚举对象属性**

枚举对象属性可以使用`for-in`语句

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
        printMessages: function () {
            document.writeln("Hello " + this.name + ". ");
            document.writeln("Today is" + this.weather + ".");
        }
    };
    for (var prop in myData) {
        document.writeln("Name: " + prop + " Value: " + myData[prop]);
    }
</script>
```

`for-in`循环代码块中的语句会对`myData`对象的每一个属性执行一次，在每一次迭代过程中，所要处理的属性名会被赋给`prop`变量

作为方法定义的那个函数也被枚举出来，方 法本身也被视为对象的属性

**增删属性和方法**

就算是用对象字面量生成的对象，也可以为其定义新属性

为对象添加了一个名为`dayOfWeek`的新属性，也可以使用数组索引表示法

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
    };
    myData.dayOfWeek = "Monday";
</script>
```

将属性值设置为一 个函数也能为对象添加新方法：

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
    };
    myData.sayHello = function () {
        document.writeln("Hello");
    };
</script>
```

对象的属性和方法可以用`delete`关键字删除

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
    };
    myData.sayHello = function () {
        document.writeln("Hello");
    };
    delete myData.name;
    delete myData["weather"];
    delete myData.sayHello;
</script>
```

**判断对象是否具有某个属性**

可以用`in`表达式判断对象是否具有某个属性

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
    };
    var hasName = "name" in myData;
    var hasDate = "date" in myData;
    document.writeln("HasName: " + hasName);
    document.writeln("HasDate: " + hasDate);
</script>
```

`hasName`变量的值是true ，`hasDate`变量的值是false 

## JavaScript运算符

```js
++、--：前置或后置自增和自减
+、-、*、/、%
<、<=、>、>=
==、!=
===、!==：等同和不等同，严格相等运算，js不会尝试装换操作数的类型
&&、||
=：赋值
+：字符串连接
?=：三元条件
```

相等运算符会尝试将操作数转换为同一类型以便判断是否相等

如果想判断值和类型是否都相同，那么应该使用等同运算符

js基本类型的比较是值的比较，而js对象的比较则是引用的比较

```html
<!--对象的相等和等同-->
<script type="text/javascript">
    var myData1 = {
        name: "Adam",
        weather: "sunny",
    };
    var myData2 = {
        name: "Adam",
        weather: "sunny",
    };
    var myData3 = myData2;
    <!--myData1和myData2是不同的对象实例-->
    var test1 = myData1 == myData2;
    <!--myData2和myData3引用同一个对象-->
    var test2 = myData2 == myData3;
    var test3 = myData1 === myData2;
    var test4 = myData2 === myData3;
    document.writeln("Test 1: " + test1 +
        " Test 2: " + test2);
    document.writeln("Test 3: " + test3 +
        " Test 4: " + test4);
</script>

<!--基本类型的相等和等同-->
<script type="text/javascript">
    var myData1 = 5;
    var myData2 = "5";
    var myData3 = myData2;
    var test1 = myData1 == myData2;
    var test2 = myData2 == myData3;
    var test3 = myData1 === myData2;
    var test4 = myData2 === myData3;
    document.writeln("Test 1: " + test1 +
        " Test 2: " + test2);
    document.writeln("Test 3: " + test3 +
        " Test 4: " + test4);
</script>
```

**显式类型转换**

***数值转换为字符串***

字符串连接运算符比加法运算符优先级更高，这可能会引起混乱，这是因为js在计算结果时会自动进行类型转换

如果想把多个数值类型变量作为字符串连接起来， 可以用`toString`方法将数值转换为字符串

```html
<script type="text/javascript">
    var myData1 = (5).toString() +
        String(5);
    document.writeln("Result: " +
        myData1);
</script>
```

`(number).toString()`和`String(number)`两种方法都是等价的，都会将`number`类型的值转换为`string`类型

***数值转换字符串的常用方法***

|        方法        |                             说明                             |
| :----------------: | :----------------------------------------------------------: |
|   `toString(n)`    |            以2、8、10、16进制表示数值，默认十进制            |
|    `toFixed(n)`    |              以小数点后有n位数字的形式表示实数               |
| `toExponential(n)` | 以指数表示法表示数值，尾数的小数点前1位数字，小数点后n位数字 |
|  `toPrecision(n)`  |       用n位有效数字表示数值，必要情况下使用指数表示法        |

***字符串转换为数值***

```html
<script type="text/javascript">
    var firstVal = "5";
    var secondVal = "5";
    var result = Number(firstVal) +
        Number(secondVal);
    document.writeln("Result: " +
        result);
</script>
```

`Number`函数解析字符串值的方式很严格，`parseInt`和`parseFloat`函数更为灵活，这两个函数还会忽略数字字符后面的非数字字符

|      方法       |                 说明                 |
| :-------------: | :----------------------------------: |
|   `Number(s)`   | 分析指定字符串，生成一个整数或实数值 |
| `parseInt(s) `  |     分析指定字符串，生成一整数值     |
| `parseFloat(s)` | 分析指定字符串，生成一个整数或实数值 |

## 数组

```html
<script type="text/javascript">
    var myArray = new Array();
    myArray[0] = 100;
    myArray[1] = "Adam";
    myArray[2] = true;
</script>
```

`new Array()`创建一个新的数组并赋给变量

+ 创建数组的时候不需要声明数组中元素的个数，js数组会自动调整大小以便容纳所有元素
+ 不必声明数组所含数据的类型。js数组可以混合包含各种类型的数据

***数组字面量***

使用数组字面量，可以在一条语句中创建和填充数组

```html
<script type="text/javascript">
    var myArray = [100, "Adam", true];
</script>
```

***读取数组内容***

索引从0开始

```html
<script type="text/javascript">
    var myArray = [100, "Adam", true];
    document.writeln("Index 0: " + myArray[0]);
</script>
```

***修改数组内容***

修改js数组中指定位置的数据，只需将新值赋给该索引位置的数组元素，改变数组元素的数据类型没有任何问题

```html
<script type="text/javascript">
    var myArray = [100, "Adam", true];
    myArray[0] = "Tuesday";
    document.writeln("Index 0: " +
        myArray[0]);
</script>
```

**枚举数组内容**

可以用for循环枚举数组内容

```html
<script type="text/javascript">
    var myArray = [100, "Adam", true];
    for (var i = 0; i < myArray.length; i++) {
        document.writeln("Index " + i + ":" + myArray[i]);
    }
</script>
```

确定数组中的元素个数可以使用其`length`属性

**常见的内置数组方法**

|         方法          |                             说明                             |
| :-------------------: | :----------------------------------------------------------: |
|   `concat(a1,...)`    | 将数组和参数所指数组的内容合并为一个新数组，不会改变原数组，是返回一个新数组可指定多个数组 |
|       `join()`        | 将数组或一个类数组的所有元素连接到一个字符串中，参数指定分隔符 |
|        `pop()`        |                  删除并返回数组最后一个元素                  |
|       `push()`        |         向数组末尾添加一个或多个元素，并返回新的长度         |
|       `shift()`       |                   删除并返回数组第一个元素                   |
|      `unshift()`      |         向数组开头添加一个或多个元素，并返回新的长度         |
|     `slice(i,j)`      |               返回一个i索引到j索引的新数组对象               |
| `splice(i1,i2,n1,n2)` | arr.splice(1, 2, 'a', 'b')，数组的1-2下标的元素会被替换为a、b，以新数组形式返回 |
|      `reverse()`      |                   原地反转数组中的元素顺序                   |
|       `sort()`        |                      原地对数组元素排序                      |
|      `indexOf()`      | 返回在数组中找到给定元素的第一个索引，若不存在返回-1，`lastIndexOf()`返回最后一个索引 |

## 处理错误

`ry-catch`语句在JavaScript中用于处理可能抛出错误的代码块。`try`块中的代码是你希望执行的代码，而`catch`块中的代码会在`try`块中的代码抛出错误时执行

```html
<script type="text/javascript">
    try {
        var myArray;
        for (var i = 0; i < myArray.length; i++) {
            document.writeln("Index " + i + ": " + myArray[i]);
        }
    } catch (e) {
        document.writeln("Error: " + e);
    }
</script>
```

`catch`子句提供一个从错误中恢复或在错误发生后进行一些清理工作的机会。如果不管是否发生错误都执行一些语句，那么可以加上一条`finally`子句并将它们置于其中

```html
<script type="text/javascript">
    try {
        var myArray;
        for (var i = 0; i <
            myArray.length; i++) {
            document.writeln("Index " + i + ": " + myArray[i]);
        }
    } catch (e) {
        document.writeln("Error: " + e);
    } finally {
        document.writeln("Statements here are always executed");
    }
</script>
```

## 比较undefined和null值

在读取未赋值的变量或试图读取对象没有的属性时得到的就是`undefined`值

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        weather: "sunny",
    };
    document.writeln("Prop: " + myData.doesntexist);
</script>
```

而`null`值是用于表示已经赋了一个值，但该值不是一个有效的对象和其他基本类型，表示无值

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
    };
    document.writeln("Var: " +
        myData.weather);
    document.writeln("Prop: " + ("weather"
        in myData));
    myData.weather = "sunny";
    document.writeln("Var: " +
        myData.weather);
    document.writeln("Prop: " + ("weather"
        in myData));
    myData.weather = null;
    document.writeln("Var: " +
        myData.weather);
    document.writeln("Prop: " + ("weather"
        in myData));
</script>
```

使用if语句和逻辑非运算符检查属性或变量是否为`undefined`或`null`

```html
<script type="text/javascript">
    var myData = {
        name: "Adam",
        city: null
    };
    if (!myData.name) {
        document.writeln("name IS null or undefined");
    } else {
        document.writeln("name is NOT null or undefined");
    }
    if (!myData.city) {
        document.writeln("city IS null or undefined");
    } else {
        document.writeln("city is NOT null or undefined");
    }
</script>
```

检查的值会被当做布尔值处理

区分使用等同运算符，不区分使用相等运算符，js会自动进行类型转换

# HTML元素

