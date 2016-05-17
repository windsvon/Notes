##响应式设计学习笔记

###1、为什么要进行响应式设计

* 设备尺寸不同
* 网速差异
* 浏览器支持的规范不同
* 输入方式不同（鼠标，键盘，触摸板，触屏etc)
* 使用场景不同
* 设备生产成本的降低使设备种类急剧增

----------
###2、流动布局
####2.1 四种布局选择
1. 固定宽度型（fixed-width):设定具体尺寸-**不推荐！**
2. 流动型（liquid)：设定尺寸百分比
3. 弹性型(elastic)：设定type size (em = 当前字体大小)
4. 混合型（hybrid)

####2.2 设定字体大小
1. 像素
   * 优点：易于控制
   * 缺点：1）无层级(cascade),不易维护。2）当页面缩放时，IE8之前版本中字体不会自动调整。3）不同设备显示不同 
2. ems
   * 优点：1）字体大小可变。 2）有层级，易于维护
   * 缺点：2）因为有层级会产生一些问题，需要注意CSS和HTML的结构
3. 百分比
   * 和Ems的性质基本一致，可以解决ems在IE中遇到的Bug
   
   **注意不同设备的默认字体大小是不同的！**
4. rems
   * 和ems相似，不过是相对于root element的字体大小
   * 可以避免ems因为层级产生的问题
   * 缺点是不兼容很多移动端浏览器（Opera Mobile）

####2.3 栅格布局
* 优点
   * 使信息的表现更加有序，新颖，和谐
   * 帮助读者搜寻信息
   * 使添加新信息更加简便，同时不影响原有页面的表现
   * Grids facilitate collaboration on the design of a single solution without compromising the overall vision of the solution.

####让内容来决定布局设计!

####2.4 Box-sizing
   * 正常的box在设置padding时会增大box的宽度，而使用box-sizing不会

####2.5 表格布局
   * 实现固定宽度和可变宽度的混合
   * 可以使用display: table-cell实现
   
-----------
###3、Media Query

####3.1 Viewport
* 浏览器的两种像素：device pixels（不变） 和 CSS pixels（变）
* 两种viewport: visual viewport（变） 和 layout viewport（不变）
* 手机为了实现全页面显示会使用很大的layout viewport

####3.2 Viewport的标签和性质
* 在`<head>`标签下输入如下格式的标签

  `<meta name="viewport" content="directive,directive" />`
* 设置宽度时，使用`content="width=device-width"`，不要设定具体宽度
* 高度设置不常用，除非不想让屏幕上下滚动
* 可以通过设置`content="user-scalable=no"`来禁止用户缩放页面
* 可以设置`initial-scale``maximum-scale`以及`minimum-scale`

####3.3 Media query的结构
1. Media query的基本结构格式如下：

		@media [not|only] type [and] )expr) {
			rules
		}
		
	其一般包含四个基本部分：
	* Media types:规定设备种类
	* Media expressions:判断一个特性是true还是false
	* Logical keywords:逻辑关键词，或且非
	* Rules:设定满足条件情况下需要调整的样式

2. Media types：有十种，用的时候再查。常用的只有all, screen 和 print。格式如下：

		@media print {
			rules
		}
		
	也可以在link element中设置

		<link rel="stylesheet" href="print.css" media="print" />
	
	两种方法各有利弊：前一种只需要一次http request, 第二种文件小

3. Media expression：格式如下：
		
		@media screen and (min-width: 320px) {
			rules
		}
		
	可以检测的特性有很多，常用的有width, height, orientation, resolution和 aspect-ratio。前面可以加前缀min-, max-
	
	会有一些新加的特性：如script, pointer 和 hover

4. Logical keywords: 
	* 注意not的用法：

			@media not screen and (color) {...} //equates to not (screen and (color))
		
	* 不支持or, 但是可以用逗号实现相同功能：
			
			@media screen and (color), projection and (color)
	
	* 关于only：一些老的浏览器不支持media query, 当遇到media query语句时会试图下载一些不需要的样式。使用only可以对老浏览器屏蔽media query语句。对于支持media query的浏览器，only不起任何作用。

####3.4 Media query的顺序

1. 从桌面到移动端：设定max-width
2. 从移动端到桌面：设定min-width。好处：CSS代码更简单？
	* 先写出核心功能
	* 确定临界点 -- **最好根据内容自行确定**，而不要用流行的设备尺寸，因为流行设备尺寸变化很快
	* 为大屏幕追加功能
	* 使用ems设置临界点可令media query更加灵活

####3.5 对IE浏览器的支持

IE8以及之前版本不支持media query，可使用条件注释解决：
		
		<!--[if (lt IE9) & (!IEMobile)]>
		<link rel="stylesheet" href=""/css/ie.css" media="all">
		<![endif]-->
		
------

###4、响应式的媒介

####4.1 目的
在不同使用场景下有选择地加载或者加载不同质量的图片/视频等媒介，提高页面性能

####4.2 有选择性地加载图片

* 先制作移动端页面，html中不包含不必要的图片
* 使用javascript加载图片
* 利用`machMedia()`命令来控制javascript加载图片

####4.3 实现响应式图片的策略

1. Fighting the browser：赶在浏览器加载图片前转换加载的图片。**缺点**：这种方法越来越难，因为浏览器会尽量快速地加载图片，因此很难赶在图片加载前完成转换。
2. Resignation：先加载小图片，当必要的时候再加载大的。**缺点**：对于大屏幕需要两次网络请求，影响性能。
3. Going to the server：在服务器端确定应该加载哪个图片。**缺点**：设备类型越来越多，服务器很难储存所有设备的信息。

**每种方法都有局限性！**

####4.4 响应式图片的实现方法

* Sencha.io Src

  在图片地址之前添加Sencha.io Src的地址，例如：
  		
  		http://src.sencha.io/http://mysite.com/images/football.jpg
  
  可以根据设备尺寸调整图片尺寸，不过有局限性。因为默认是把图片尺寸调整为100%设备尺寸，而且需要第三方服务。

* Adaptive Images

 它可以根据设备屏幕尺寸创建一个相应尺寸的图片，可通过以下几个步骤设置：
 
 1. 将`.htaccess`和`adaptive-images.php`文件放置在根目录下
 2. 创建一个`ai-cache`文件并保证其具有写入权限
 3. 将如下的javascript代码放入文件的`<head>`标签中
 		
 			<script>document.cookie='resolution='+Math.max(screen.width, screen.height)+'; path=/';<script>
 	这行代码的目的是获取当前屏幕的分辨率并保存在cookie中
 4. 设置`adaptive-images.php`的方法有很多，常用的是设置通过`$resolutions`设置临界点。例如：
 	
		```
		$resolution = array(860, 600, 320);
	
		```
 			
 5. 缺点是 1）没有art direciton? 2）有时在大屏幕状态下图片反而尺寸小（本来一列变两列），也会产生问题。3）由于这种方法不改变URL，CDN会缓存图片，当屏幕尺寸变化时会使用缓存中的图片。
 
 **每种方法都有局限，要根据项目选择合适的方法，不过Adaptive Images是一种相对更好的方法**
 
####4.5 背景图片、字体

   * **使用mobile up**

####4.6 高分辨率显示

   * 大多数浏览器使用`min-resolution`，webkit使用`-webkit-min-device-pixel-ratio`

####4.7 SVG
	
   * 可以结局高分辨率显示的问题，图片尺寸改变不影响文件大小

####4.8 视频和广告

###一些暂时不太明白的点

* style guide

* 使用API管理内容

###服务器端的响应式设计

####客户端响应式设计的局限

* 不易适应内容：客户端的响应式解决方案只能考虑已发送到客户端的内容。
* 不易考虑性能：不能阻止不需要的markup, javascript和CSS的下载。
* 不能定位低端设备
* 不能定位电视

WURFL

Modernizr - 用来检测浏览器是否支持某特性

Moernizr-server - 在服务器端检测

###根据网速进行响应式设计

* 实现方法：1）通过测试一个图片的加载速度来判断网速。2）使用navigator.connection



  
  
		
		
		



