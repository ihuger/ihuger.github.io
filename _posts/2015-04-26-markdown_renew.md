---
layout: post
title: " 重新学习markdown"
description: ""
category: tools
tags: [markdown]
---

{% include JB/setup %}

###标题
\#{n}  多少个#就代表第几层标题，如 

        #### H4 四级标题

###列表
使用 "-/./+" 来表示无序列表

使用 "num." 来表示有序列表

1. 如果单个列表项跨多个段，需要空行之后、递进4个空格符

        1. list1, 第一段 

            第二段
        2. list2
2. 如果需要在列表中添加块，同样需要递进4个空格符

        1. list1

            > b1
            >
            > b2
        2. list2

3. 如果需要在列表中添加代码块，需要递进8个字符

        * list1

                echo 'shell script'|wc -c
        * list2


###块
使用 ">" 来表示block，如下：(注意需要 '>'开始的空行过渡)

        > level 1 block
        >
        > > level 2 block
        >
        > - 支持block内的markdown
        > - list2

###程序块
1. 使用 \` , (这种markdown解析对\`\`\`支持不是很好, [github-flavored-markdown]()中支持\`\`\`的表示方式)

    `echo "test for code "`

2. 使用递进（对齐）来表示

    
###链接
1. 普通的表示方式，括号内可以使用相对当前网址的相对位置来表示

        This is [an example](http://www.baidu.com/ "Title") inline link.

        [This link](/about.html) has no title attribute.

    将会生成

    This is [an example](http://www.baidu.com/ "Title") inline link.

    [This link](/about.html) has no title attribute.

2. 通过id的方式来表示

        id1连接表示[id1][id_link_1] 
    
        id2（相对路径）表示[id2][id_link_2] 

        空标记 [GOOGLE][]
            
        [id_link_1]: /about.html "title: link by id"
        [id_link_2]: http://lake59.github.io 'title: link by id'
        [GOOGLE]: https://www.google.com 

    将会生成：

    id1连接表示[id1][id_link_1] 
    
    id2（相对路径）表示[id2][id_link_2] 

    空标记 [GOOGLE][]
        
    [id_link_1]: /about.html "title: link by id"
    [id_link_2]: http://lake59.github.io 'title: link by id'
    [GOOGLE]: https://www.google.com 

3. image的表示，与链接表示很像，需在方括号前加上 !，如

        ![image]({{ site.url }}/assets/images/github.png)

    将会产生：

    ![image]({{ site.url }}/assets/images/github.png)

###强调
使用 \* 或 \_ 来表示

- 一个 \* 或 \_ 表示\*斜体\*:  *斜体*
- 两个 \* 或 \_ 表示\*\*粗体\*\*:  **粗体**
- [github-flavored-markdown]()语法中，删除线用两个波浪线表示,如 ~~Mistaken text.~~

###表
table的表示在做一些展示的时候经常用到, markdown中使用 | - : 这三个符号来构成表，| 用于分割列，: 用于表示对齐方式, - 用于分割表头和数据， 如

    |col1|col2|col3
    |:-|-|-
    |val1|val2|val3

将会生成：

|col1|col2|col3
|:-|-|-
|val1|val2|val3


####refer：
* [github-flavored-markdown](https://help.github.com/articles/github-flavored-markdown/)
* [daringfireball markdwon](http://daringfireball.net/projects/markdown/syntax)


