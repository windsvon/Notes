##块级格式上下文（BFC）学习心得

BFC是CSS中一个相对冷门的概念，以至于很多从事前端开发多年的程序员对它也不是很熟悉。网上有很多关于BFC的文章，大都是对一些基本概念的解释，辅以一些简单的例子，读完常常感觉自己已经明白了，但是在实际项目中遇到还是会很疑惑。看来想彻底理解BFC这个概念还是需要多看、多总结。

这篇文章主要总结了一些我认为重要的BFC基本概念

###BFC 定义
Block formatting context(BFC)是一个独立的渲染区域，只有Block-Level box参与，它规定了内部的Box如何布局，并且与这个区域外部毫不相干。

###BFC布局规则
* 内部的BOX会在垂直方向一个接一个地放置。
* Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
* 每个元素的margin box的左边，与包含块border box的左边相接触（对于从左往右的格式化，否则相反）。即使存在浮动也是如此。
* BFC的区域不会与float box重叠。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
* 计算BFC的高度时，浮动元素也参与计算。

###如何生成BFC
* 设置浮动
* 绝对定位元素
* inline-block
* table-cell
* table-caption
* 设置overflow且值不为visible
* display:flex 或者 display:inline-flex