---
title: "理解margin负值在CSS圣杯布局中的应用"
date: 2022-04-26T01:34:01+08:00
categories: ["学习笔记"]
tags: ["CSS"]
draft: false
---

假设一个页面是圣杯布局（三栏，左右两栏固定宽度 150px，中间栏自适应），很多人会选择用 三个浮动盒子 + margin 负值来实现，例如：

```html
<div class="clearfix">   
    <div id="main">      
        <div id="body">我是中间</div>   
    </div>   
    <div id="left">我是左边</div>   
    <div id="right">我是右边</div> 
</div> 
<p>嘿，我是normal flow</p>
```

注意：使用这种方法时中间栏的标签必须放在左右两栏的前面。

```
html,
body {
  margin: 0;
  height: 100%;
}

#main {
  width: 100%;
  height: 300px;
  float: left;
  background-color: pink;
}

#main #body {
  margin: 0 150px;
  height: 100%;
  background-color: skyblue;
}

#left,
#right {
  width: 150px;
  height: 300px;
  float: left;
  background-color: #a2d6a0;
}

#left {
  margin-left: -100%;
}

#right {
  margin-left: -150px;
}

.clearfix:after {
  content: "";
  display: table;
  clear: both;
}
```



但我一直不明白，为什么上面这个例子中，给左栏添加 margin-left: -100% 后左栏会跑到上一行中？于是进行了如下探索。

我们知道，在一般文档流布局中 margin-left 值为负数时，元素与正常移动方向相反，向左移动，负数的绝对值越大，元素向左移动得越远，甚至溢出屏幕。

但如果是给浮动元素添加 margin-left 负值呢？例如下图：三个宽高为 100px 的靠左浮动的盒子。

 ![](/img/posts/margin-1.png)

给它们都添加 margin-left: -50px 之后：

![](/img/posts/margin-2.png)

发生了什么？依次来看：

粉盒子会向左移动 50px，由于靠左浮动的元素要靠左贴边，绿&紫也会紧贴上来。

在此基础上，绿盒子继续向左移动 50px，遮住了粉盒子剩下的一半。紫盒子也依旧紧贴上来。

最后，紫盒子也要向左移动 50px，因此遮住绿盒子的一半。

总结一下，三个浮动元素靠左贴边的同时各自相对地向左移动了 50px。

如果只给绿&紫盒子添加 margin-left: -50px：

{{< figure  src="/img/posts/margin-3.png"  width="20%" >}}

那么只有绿&紫盒子相对向左移动 50px 。

------

接下来考虑到一种情况，依旧是三个浮动盒子，但粉盒子非常宽，极端一点， 直接width: 100%，那么绿&紫盒子无疑会被挤到下一行。

{{< figure  src="/img/posts/margin-4.png"  width="50%" >}}

如果此时给绿盒子添加margin-left: -50px，会怎么样？

{{< figure  src="/img/posts/margin-5.png"  width="50%" >}}

绿盒子向左移动 50px，如果它们都是在同一行，此时粉盒子最右边的 50px将被绿盒子给遮住，但现在不行，因为不可能让绿盒子拆分为两半，它只能保留在下一行。

那么，如果给绿盒子添加 margin-left: -100px 呢？绿盒子被允许向左移动 100px，依旧想象一下它们在同一行，那么绿盒子将遮盖粉盒子最右侧的 100px，因为100px 正好是绿盒子的宽度，所以它将能完整地出现在第一行。

{{< figure  src="/img/posts/margin-6.png"  width="50%" >}}

以此类推，粉盒子就是圣杯布局的中间栏，绿盒子就是左侧栏。如果给绿盒子添加 margin-left: -100%，就是让绿盒子向左移动一个页面宽度的距离，它将出现在自己原位置的正上方。

{{< figure  src="/img/posts/margin-7.png"  width="50%" >}}

总结一下，和一般文档流时使用 margin 负值不同，给浮动元素添加 margin 负值，关键在于理解“浮动”的吸附（贴边）特性（即使被挤到换行这个特性也依然存在）
