---
layout: post
title: "Markdown 语法手册"
categories: Manual
tags: Markdown
---

* content
{:toc}

**NOTE:** This is Simplified Chinese Edition Document of Markdown Syntax. If you are seeking for English Edition Document. Please refer to [Markdown: Syntax][eng-doc].

  [eng-doc]:http://daringfireball.net/projects/markdown/syntax

## 概述 

### 哲学

Markdown 的目标是实现「易读易写」。

不过最需要强调的便是它的可读性。一份使用 Markdown 格式编辑的文件应该可以直接以纯文本的形式发布，并且看起来不会像是许多标签或者指令所构成的，Markdown 受到一些即有的 text-to-HTML 格式的影响，包括 [Setext][1]、[atx][2]、[Textile][3]、[reStructuredText][4]、[Grutatext][5] 和 [EtText][6]，然而最大的灵感来源其实是纯文本的电子邮件格式。

  [1]: http://docutils.sourceforge.net/mirror/setext.html
  [2]: http://www.aaronsw.com/2002/atx/
  [3]: http://textism.com/tools/textile/
  [4]: http://docutils.sourceforge.net/rst.html
  [5]: http://www.triptico.com/software/grutatxt.html
  [6]: http://ettext.taint.org/doc/






因此 Markdown 的语法全由标点符号组成，并且经过慎重的选择，是为了让它们看起来就像所要表达的意思。像是在文字两旁加上星号，看起来就像 `\*强调\*`。Markdown 的列表看起来，嗯，就是列表。假如你有使用过电子邮件，区块引言看起来就真的像是引用一段文字。

### 行内 HTML

Markdown 的语法有个主要的目的：用来作为一种网络内容的写作语言。

Markdown 不是要来取代 HTML，甚至也没有要和它相似，它的语法种类不多，只和 HTML 的一部分有关系，重点不是要创造一种易于写作的 HTML 文件的语法，我认为 HTML 已经很容易写了，Markdown 的重点在于，它能让文件更容易阅读、编写。HTML 是一种发布的格式，而 Markdown 是一种编写的格式，因此，Markdown 的语法格式只涵盖纯文本可以涵盖的范围。

不在 Markdown 涵盖范围之内的标签，都可以直接在文件里用 HTML 编写。不需要额外标注这是 HTML 或者是 Markdown，只要直接加标签就可以了。

只有区块元素，比如 `<div>`、`<table>`、`<pre>`、`<p>` 等标签，必须在前后加上空行，以便利于区分内容。而这些（元素）开始与结尾的标签，不可以使用 Tab 或者空格来进行缩排。Markdown 的解释器可以自动判断，避免在区块标签前加上没有必要的 `<p>` 标签。

举例来说，在 Markdown 文件里加上一段HTML表格：

~~~html
This is a regular paragraph.

<table>
    <tr>
        <td>Foo</td>
    </tr>
</table>

This is another regular paragraph.
~~~

请注意，Markdown 语法在 HTML 区块标签中将不会被进行处理。例如你无法在 HTML 区块内使用Markdown形式的 `*强调*`。

HTML 的行内标签如 `<span>`、`<cite>`、`<del>` 则不受限制，可以在 Markdown 的段落、列表或者标题里任意使用。依照个人习惯，甚至可以不用 Markdown 格式，而采用 HTML 标签来格式化。举例说明：如果比较喜欢 HTML 的 `<a>` 或 `<img>` 标签，可以直接使用这些标签，而不用 Markdown 提供的链接或是影像标示语法。

HTML 行内标签和区块标签不同，在行内标签的范围内，Markdown 的语法是有效的。

### 特殊元字符自动转换

在 HTML 文件中，有两个元字符需要特殊处理：`<` 和 `&`。`<` 符号用于起始标签，`&` 符号用于标记 HTML 实体，如果你只是想要使用这些符号，你必须使用实体的形式，如 `&lt;` 和 `&amp;`。

`&amp;` 符号其实很容易让写作网络文件的人感到困扰，如果你要打『AT&amp;T』，你必须写成『`AT&amp;T`』，还得转换网址内的 `&amp;` 符号，如果你要链接到：

~~~
http://images.google.com/images?num=30&q=larry+bird
~~~

你必须将网址转成：

~~~
http://images.google.com/images?num=30&amp;q=larry+bird
~~~

这样才能放到链接标签的 href 属性里。不用说也知道这很容易忘记，这也可能是 HTML 标准检查所检查到的错误中，数量最多的。

Markdown 允许你直接使用这些符号，但是你要小心跳脱字元的使用，如果你是在 HTML 实体中使用 `&` 符号的话，它不会被转换，而在其它情形下，它则会被转换成 `&amp`。所以你如果要在文件中插入一个著作权的符号，你可以这让样写：

~~~
&copy
~~~

Markdown 将不会对这段文字做修改，但是如果你这让写：

~~~
AT&T
~~~

Markdown就会将它转换为：

~~~
AT&amp;T
~~~

类似的情况也会发生在 `<` 符号上，因为 Markdown 支持行内 HTML，如果你是使用 `<` 符号作为 HTML 标签使用，那么 Markdown 也不会对它做任何转换，但是如果你是写：

~~~
4 < 5
~~~

Markdown 将会把它转为：

~~~
4 &lt; 5
~~~

不过要注意的是，code 范围内，不论是行内还是区块，`<` 和 `&` 两个符号都一定会被转换成 HTML 实体，这样的特性让你可以很容易地用 Markdown 写 HTML code（和 HTML 相对而言。在 HTML 语法中，你要将所有的 `<` 和 `&` 都转换为 HTML 实体，才能在 HTML 文件里写出 HTML code）。

## 区块元素

### 段落和换行

一个段落是由一个以上相连的行句组成，而一个以上的空行则会切分出不同的段落（空行的定义是显示上看起来像是空行，便会被视为空行。比方说，若某一行只包含空白和 Tab，则该行也会被视为空行），一般的段落不需要空白或者断行缩排。

『一个以上相连的行句组成』这句话其实暗示了 Markdown 允许段落内的强迫断行，这个特性和其他大部分的 text-to-HTML 格式不一样（包括 MovableType 的『Convert Line』选项），其它的格式会把买个断行都转换成 `<br />` 标签。

如果你真的想要插入 `<br />` 标签的话，在行尾加上两个以上的空格，然后按Enter。

是的，这确实需要话比较多的功夫来插入 `<br />`，但是『每个换行都转换为 `<br />`』的方法在 Markdown 中并不合适，Markdown 中 Email 的区块引言和多段落列表在使用换行来排版的时候，不但更好用，还更好阅读。

### 标题

Markdown 支持两种标题的语法，Setext 和 atx 形式。

Setext 形式是用底线的形式，利用 `=`（最高级标题）和 `-`（第二级标题），例如：

~~~markdown
This is an H1
=============

This is an H2
-------------
~~~

任何数量的 `=` 和 `-` 都可以有效果。

Atx形式则是在行首插入 1 到 6 个`#`，各自对应到标题 1 到 6 级，例如：

~~~markdown
# This is an H1

## This is an H2

###### This is an H6
~~~

你可以选择性的『开闭』atx 样式的标题，这纯粹只是美观用的，若是觉得这样看起来比较舒服，你可以在行尾加上 `#`，而行尾的 `#` 的数量也不用和开头一样（行首的井字数量决定标题的级数）：

~~~markdown
# This is an H1 #

## This is an H2 ##

### This is an H3 ######
~~~

### 区块引言

Markdown 使用 Email 形式的区块引言，如果你很熟悉如何在 Email 信件中引言，你就知道怎么在 Markdown 文件中建立一个区块引言，那会看起来像是你强迫断行，然后再每行的最后面加上`>`：

~~~markdown
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.
~~~

Markdown 也允许你只在整个段落的第一行最前面加上 `>`：

~~~markdown
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aliquam hendrerit miposuere lectus. Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse id sem consectetuer libero luctus adipiscing.
~~~

区块引言也可以有层级（例如：引言内的引言），只要根据层数加上不同数量的 `>`：

~~~markdown
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
~~~

引言的区块内也可以使用其它的 Markdown 语法，包括标题、列表、代码区块等：

~~~markdown
> ## This is a header.
> 
> 1.   This is the first list item.
> 2.   This is the second list item.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");
~~~

任何标准的文字编辑器都能简单地建立 Email 样式的引言，例如 BBEdit，你可以选取文字然后从菜单中选择增加引言的层级。

### 列表

Markdown支持有序列表和无序列表。

无序列表使用星号、加号或是减号作为列表标记：

~~~markdown
*   Red
*   Green
*   Blue
~~~

等同于：

~~~markdown
+   Red
+   Green
+   Blue
~~~

也等同于：

~~~markdown
-   Red
-   Green
-   Blue
~~~

有序列表则使用数字接着一个英文句号：

~~~markdown
1.  Bird
2.  McHale
3.  Parish
~~~

很重要的一点是，你在列表标记上使用的数字并不会影响输出的HTML结果，上面的列表所产生的HTML标记为：

~~~html
<ol>
    <li>Bird</li>
    <li>McHale</li>
    <li>Parish</li>
</ol>
~~~

如果你的列表标记写成：

~~~markdown
1.  Bird
1.  McHale
1.  Parish
~~~

或甚至是：

~~~markdown
5.  Bird
2.  McHale
8.  Parish
~~~

你都会得到完全相同的HTML输出。重点在于，你可以让 Markdown 文件的列表数字和输出结果相同，或是你懒一点，你可以完全不用在意数字的正确性。

如果你使用懒惰的方法，建议第一个项目最好是从「1.」开始，因为 Markdown 未来可能会支持有序列表的 start 属性。

列表项目标记通常是放在最左边，但是其实也可以缩排，最多三个空格，项目标记后面则一定要接至少一个空白或 Tab。

要让列表看起来更漂亮，你可以把内容固定的缩排整理好：

~~~markdown
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.
~~~

但是如果你很懒，那也不一定需要：

~~~markdown
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
Suspendisse id sem consectetuer libero luctus adipiscing.
~~~

如果列表项目间用空行分开，Markdown会把项目内的内容在输出时用 `<p>` 标签包起来，举例来说：

~~~markdown
*   Bird
*   Magic
~~~

但是这样：

~~~markdown
*   Bird

*   Magic
~~~

会被转换为：

~~~html
<ul>
    <li><p>Bird</p></li>
    <li><p>Magic</p></li>
</ul>
~~~

列表项目可以包含多个段落，每个项目下的段落都必须缩排 4 个空格或者是一个 Tab：

~~~markdown
1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.
~~~

如果你每行都有缩排，看起来会好看很多，当然，再次地，如果你很懒惰，Markdown 也允许：

~~~markdown
*   This is a list item with two paragraphs.

    This is the second paragraph in the list item. You're
only required to indent the first line. Lorem ipsum dolor
sit amet, consectetuer adipiscing elit.

*   Another item in the same list.
~~~

如果要在列表项目内放进引言，那 `>` 就需要缩排：

~~~markdown
*   A list item with a blockquote:

    > This is a blockquote
    > inside a list item.
~~~

如果要放程序代码区块的话，该区块就需要缩排两次，也就是 8 个空格或者两个 Tab：

~~~markdown
*   A list item with a code block:

        <code goes here>
~~~

当然，项目列表很可能会不小心产生，像是下面这样的写法：

~~~
1986. What a great season.
~~~

换句话说，也就是在行首出现数字一个英文句号一点空白，要避免这样的情况，你可以在句号前面加上反斜杠：

~~~
1986\. What a great season.
~~~

### 代码区块

和代码相关的编写或是标记语言的源代码通常会有已经排版好的代码区块，通常这些区块我们并不希望它以一般段落文件的方法去排版，而是照着原来的样子显示，Markdown 会用 `<pre>` 和 `<code>` 标签来把代码区块包起来。

要在 Markdown 中建立代码区块很简单，只要简单地缩排 4 个空格或是 1 个 Tab 就可以，例如，下面的输入：

~~~markdown
This is a normal paragraph:

    This is a code block.
~~~

Markdown会转换成：

~~~html
<p>This is a normal paragraph:</p>

<pre><code>This is a code block.
</code></pre>
~~~

这个每行一级的缩排（4 个空格或者是 1 个 Tab），都会被移除，例如：

~~~markdown
Here is an example of AppleScript:

    tell application "Foo"
        beep
    end tell
~~~

会被转换为：

~~~html
<p>Here is an example of AppleScript:</p>

<pre><code>tell application "Foo"
    beep
    end tell
</code></pre>
~~~

一个代码区块会一直持续到没有缩排的那一行（或是文件尾）。

在代码区块里面，`&`、`<` 和 `>` 会自动转换成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入示例用的 HTML 源代码，只需要被复制上，再加上缩排就可以了，剩下的 Markdown 会帮你处理，例如：

~~~html
<div class="footer">
    &copy; 2004 Foo Corporation
</div>
~~~

会被转换为：

~~~html
<pre><code>&lt;div class="footer"&gt;
&amp;copy; 2004 Foo Corporation
&lt;/div&gt;
</code></pre>
~~~

代码区块中，一般的 Markdown 语法不会被转换，像是星号便只是星号，这表示你可以很容易地以 Markdown 语法编写 Markdown 语法相关的文件。

### 分隔线

你可以在一行中使用三个或以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号中间插入空格。下面每种写法都可以建立分隔线：

~~~markdown
* * *

***

*****

- - -

---------------------------------------
~~~

## 区段元素

### 链接

Markdown 支持两种方法的链接语法：行内和参考两种形式。

不管是哪一种，连接的文字都是用方括号 `[]` 来标记。

要建立一个行内形式的链接，只要在方括号后面马上接着括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面用双引号把 title 文字包起来即可，例如：

~~~markdown
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
~~~

会产生：

~~~html
<p>This is <a href="http://example.com/" title="Title"> an example</a> inline link.</p>

<p><a href="http://example.net/">This link</a> has no title attribute.</p>
~~~

如果你是要链接到本地资源，你可以使用相对路径：

~~~markdown
See my [About](/about/) page for details.
~~~

参考形式的链接使用另一个方括号接在链接文字的括号后面，而在第二个方括号里要填入用以辨识链接的标签：

~~~markdown
This is a [an example][id] reference-style link.
~~~

你也可以选择性地在两个方括号中间加上空格：

~~~markdown
This is [an example] [id] reference-style link.
~~~

接着，在文件的任意位置，你可以把这几个标签的链接内容定义出来：

~~~markdown
[id]: http://example.com/  "Optional Title Here"
~~~

链接定义的形式为：

+   方括号，里面输入链接的辨识用标签
+   接着一个冒号
+   接着一个以上的空白或者 Tab
+   接着链接的网址
+   选择性的接着 title 内容，可以用单引号、双引号或者是括号包着

下面这三种链接的定义都是相同的：

~~~markdown
[foo]: http://example.com/  "Optional Title Here"
[foo]: http://example.com/  'Optional Title Here'
[foo]: http://example.com/  (Optional Title Here)
~~~

**请注意：**有一个已知的问题是 Markdown.pl 1.0.1 会忽略单引号包起来的链接 title。

链接网址也可以用方括号抱起来：

~~~markdown
[id]: <http://example.com/>  "Optional Title Here"
~~~

你也可以把 title 属性放到下一行，也可以加一些缩排，网址太长的话，这样会比较好看：

~~~markdown
[id]: http://example.com/longish/path/to/resource/here
    "Optional Title Here"
~~~

网址定义只有在产生链接的时候用到，并不会直接出现在文件之中。

链接辨识标记可以有字母、数字、空格和标点符号，但是并不区分大小写，因此下面两个链接是一样的 ：

~~~markdown
[link text][a]
[link text][A]
~~~

预设的链接标签功能让你可以省略指定链接标签，这种情形下，链接标签和链接文字会视为相同，要用预设链接标签只要在链接文字后面加个一个空的方括号，如果你要让 "Google" 链接到 google.com，你可以简化成：

~~~markdown
[Google][]
~~~

然后定义链接内容：

~~~markdown
[Google]: http://google.com/
~~~

由于链接文字可能包含空白，多疑这种简化的标签内也可以包含多个文字：

~~~markdown
Visit [Daring Fireball][] for more information.
~~~

然后接着定义链接：

~~~markdown
[Daring Fireball]: http://daringfireball.net/
~~~

链接的定义可以放在文件中的任何一个地方，我比较偏好直接放在链接出现段落的后面，你也可以把它放在文件的最后面，就像是注解一样。

下面是一个参考链接的范例：

~~~markdown
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

[1]: http://google.com/        "Google"
[2]: http://search.yahoo.com/  "Yahoo Search"
[3]: http://search.msn.com/    "MSN Search"
~~~


如果改成用链接名称的方式写：

~~~markdown
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

[google]: http://google.com/        "Google"
[yahoo]:  http://search.yahoo.com/  "Yahoo Search"
[msn]:    http://search.msn.com/    "MSN Search"
~~~

上面两种写法都会产生下面的HTML：

~~~html
<p>I get 10 times more traffic from <a href="http://google.com/" title="Google">Google</a> than from
<a href="http://search.yahoo.com/" title="Yahoo Search">Yahoo</a>
or <a href="http://search.msn.com/" title="MSN Search">MSN</a>.</p>
~~~

下面是用行内形式写的同样一段内容的 Markdown 文件，提供作为比较只用：

~~~markdown
I get 10 times more traffic from [Google](http://google.com/ "Google")
than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or
[MSN](http://search.msn.com/ "MSN Search").
~~~

参考式的链接其实重点不在于它比较好写，而是它比较好读，比较一下上面的范例，使用参考式的文章本身只有 81 个字符，但是用行内形式的链接却会增加到 176 个字符，如果是用纯 HTML 格式来写，会有 234 个字符，在 HTML 格式中，标签比文字还多。

使用 Markdown 的参考式链接，可以让文件更像是浏览器最后产生的效果，让你可以把一些标记相关的内容移到段落文字之外，你就可以增加链接而不让文章的阅读感觉被打断。

### 强调

Markdown使用星号（`*`）和下划线（`_`）作为强调字词的符号，被 `*` 或 `_` 包围的字词会被转成用 `<em>` 标签包围，用两个 `*` 或 `_` 包起来的话，则会被转成 `<strong>`，例如：

~~~markdown
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__
~~~

会转成：

~~~html
<em>single asterisks</em>

<em>single underscores</em>

<strong>double asterisks</strong>

<strong>double underscores</strong>
~~~

你可以随便用你喜欢的样式，唯一的限制是，你用什么符号开启标签，就要用什么符号结束。

强调也可以直接插在文字中间：

~~~
un*frigging*believable
~~~

但是如果你的`*`和`_`两边都有空白的话，它们就只会被当成普通的符号。

如果要在文字前后直接插入普通的星号或下划线，你可以用反斜杠：

~~~
\*this text is surrounded by literal asterisks\*
~~~

### 程序代码

如果要标记一小段行内程序代码，你可以用反引号将它包起来（`），例如：

~~~markdown
Use the `printf()` function.
~~~

会产生：

~~~html
<p>Use the <code>printf()</code> function.</p>
~~~

如果要在程序代码区段内插入反引号，你可以用多个反引号来开启和接触程序代码区段：

~~~markdown
``There is a literal backtick (`) here.``
~~~

这算语法会产生：

~~~html
<p><code>There is a literal backtick (`) here.</code></p>
~~~

程序代码区段的起始和结束端都可以放入一个空格，起始端后面一个，结束端前面一个，这样你就可以在区段的一开始就插入反引号：

~~~markdown
A single backtick in a code span: `` ` ``

A backtick-delimited string in a code span: `` `foo` ``
~~~

会产生：

~~~html
<p>A single backtick in a code span: <code>`</code></p>

<p>A backtick-delimited string in a code span: <code>`foo`</code></p>
~~~

在程序代码区段内，`&` 和方括号都会被转换成 HTML 实体，这样会比较容易插入 HTML 源代码，Markdown 会把下面这段：

~~~markdown
Please don't use any `<blink>` tags.
~~~

转为：

~~~html
<p>Please don't use any <code>&lt;blink&gt;</code> tags.</p>
~~~

你也可以这样写：

~~~markdown
`&#8212;` is the decimal-encoded equivalent of `&mdash;`.
~~~

以产生：

~~~html
<p><code>&amp;#8212;</code> is the decimal-encoded
equivalent of <code>&amp;mdash;</code>.</p>
~~~

### 图片

很明显地，要在纯文本的应用中设计一个「自然」的语法来插入图片是有一定难度的。

Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式：行内和参考。

行内图片的语法看起来像是：

~~~markdown
![Alt text](/path/to/img.jpg)

![Alt text](/path/to/img.jpg "Optional title")
~~~

详细描述如下：

+   一个感叹号 `!`
+   接着一个方括号，里面放上图片的代替文字
+   接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上选择性的 title 文字。

参考式的图片语法则长得像这样：

~~~markdown
![Alt text][id]
~~~

「id」是图片参考的名称，图片参考的定义方式则合链接参考一样：

~~~markdown
[id]: url/to/image  "Optional title attribute"
~~~

到目前为止，Markdown 还没有办法指定图片的宽高，如果你需要的话可以使用普通的 `<img>` 标签。

## 其他

### 自动链接

Markdown支持比较短的自动链接形式来处理网址和电子邮电地址，只要是用方括号包起来，Markdown 就会自动把它们转成链接，链接的文字就和链接位置一样，例如：

~~~markdown
<http://example.com/>
~~~

就会自动转为：

~~~html
<a href="http://example.com/">http://example.com/</a>
~~~

自动的邮件链接也很类似，只是Markdown会先做一个编码转换的过程，把文字字符转化为16进位码的HTML实体，这样的格式可以混淆一些不好的信箱地址收集机器人，例如：

~~~markdown
<address@example.com>
~~~

Markdown会转为：

~~~html
<a href="&#x6D;&#x61;i&#x6C;&#x74;&#x6F;:&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;">&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;</a>
~~~

在浏览器里面，这段字符串会变成一个可以点击的「address@example.com」链接。

（这种做法虽然可以混淆不少机器人，但并无法完全挡下来，不过这样也比什么都不做好些。无论如何，公开你的邮箱地址终究会引来广告邮件的。）

### 跳脱字元

Markdown 可以利用反斜杠来插入一些在语法中有其他意义的符号，例如：如果你想要用星号加载文字旁边的方式来作出强调效果（但不用 `<em>` 标签），你可以在星号的前面加上反斜杠：

~~~
\*literal asterisks\*
~~~

Markdown支持在下面这些符号前加上反斜杠来帮助插入普通的符号：

~~~
\   反斜杠
`   反引号
*   星号
_   下划线
{}  大括号
[]  方括号
()  括号
#   井字号
+   加号
-   减号
.   英文句号
!   感叹号
~~~