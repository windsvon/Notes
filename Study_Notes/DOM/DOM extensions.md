[toc]

# DOM Extensions

（ *Professional Javascript for Web Developer， 3rd Edition* -- Chapter 11 )

为了方便使用，DOM包含了很多扩展功能，起初几乎所有的DOM扩展都是浏览器专有的，08年之后W3C将许多专有扩展转化成了标准规范，其中最主要的两个标准化的DOM扩展是**Selector API**和**HTML5**



## Selector API

**Selector API**提供了类似Jquery的更加方便的DOM选择器，可以通过CSS query来选择DOM元素。

### querySelector()

这个方法可以根据css query选取子元素中**第一个**匹配的对象

### querySelectorAll()

这个方法可以根据css query选取子元素中**所有**匹配的对象

### matchesSelector()

判断一个`element`是否匹配某个CSS选择器



## Element Traversal API

由于IE9之前的IE浏览器中空格不被识别为文本节点，因此会导致`childNodes`和`firstChild`方法的浏览器差异性。以下几个属性可以解决这种差异性：

* `childElementCount` -- 返回子节点中`element`的数量（不包括`text`节点以及`comment`节点）
* `firstElementChild` -- 返回子节点中的第一个`element`
* `lastElementChild` -- 返回子节点中的最后一个`element`
* `previousElementSibling` -- 返回兄弟节点中的前一个`element`
* `nextElementSibling` -- 返回兄弟节点中的后一个`element`


## HTML5

### getElementsByClassName()

### classList

HTML5中提供了一个`classList`属性来处理一个元素有多个`class`的情况下对`class`的处理。`classList`属性是`DOMTokenList`的一个实例，其包含了如下几个方法：

* `add(value)` -- 向list中添加字符串
* `contains(value)` -- 判断list中是否包含某字符串
* `remove(value)` -- 从list中删除某些字符串
* `toggle(value)` -- 如果list中包含这个字符串，删之，否自添加之

### Focus Management

* `activeElement` -- 指向当前获取focus的DOM元素
* `focus()` -- 使元素获取focus

```js
	var button = document.getElementById("myButton");
	button.focus();
	alert(document.activeElemenet === button); //true
```

* `hasFocus()` -- 判断`document`是否获取focus，可用来判断用户是否在操作页面。

```js
	var button = document.getElementById("myButton");
	button.focus();
	alert(document.hasFocus()); //true
```

### HTMLDocument的新变化

* **`readyState`属性** -- 判断`document`状态
	* `loading` -- `document`正在加载
	* `complete` -- `document`加载完成
	
	```js
		if (document.readyState == "complete") {
			//do something
		}
	```
* 	**Compatibility Mode** -- 判断当前浏览器的渲染模式是**standards mode**还是**quirks mode**.

	```js
		if (document.compatMode == "CSS1Compat") {
			alert("Standards mode");
		} else {
			alert("Quirks mode");
		}
	```
	
* **`head`属性** -- 获取HTML文件的头部

	```js
		var head = document.head || document.getElementsByTagName("head")[0];
	```	
	
* **Character Set属性** -- 获取和设置文件的字符集

	```js
		alert(document.charset); //"UTF-16"
		document.charset = "UTF-8"
	```
	
	`defaultCharset`属性指定浏览器和系统默认的字符集
	
### 自定义Data Attributes

HTML允许元素定义以**`data-`**开头的attribute.

```html
	<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div>
```

以**`data-`**开头的attribute可以通过`dataset`属性来获取或修改

```js
	var div = document.getElementById("myDiv");
	
	//get the values
	var appId = div.dataset.appId;
	var myName = div.dataset.myname;
	
	//set the value
	div.dataset.appId = 23456;
	div.dataset.myname = "Michael";
	
	//is there a "myname" value?
	if (div.dataset.myname) {
		alert("Hello, " + div.dataset.myname);
	}
```

### 插入标签

* `innerHTML`属性
	* 用来读取时，返回元素所有的子节点，包括`element`, `comments`和`text`节点。
	* 用来写入时，用新的DOM树完全覆盖作用元素的子节点。

* `outerHTML`属性
	
	* 用来读取时，返回作用的HTML元素本身，例如`div.outerHTML`返回`div`及其所有子节点	
	* 用来写入时，用新的DOM树覆盖作用元素本身：
	
	```js
		div.outerHTML = "<p>This is a paragraph.</p>"
		
		//等价于
		
		var p = document.createElement("p");
		p.appendChild(document.createTextNode("This is a paragraph."));
		div.parentNode.replaceChild(p, div);
	```

* `insertAdjacentHTML()`方法 

```js
	//insert as previous sibling
	element.insertAdjacentHTML("beforebegin", "<p>Hello world!</p>");
	
	//insert as first child
	element.insertAdjacentHTML("afterbegin", "<p>Hello world!</p>");
	
	//insert as last child
	element.insertAdjacentHTML("beforeend", "<p>Hello world!</p>");
	
	//insert as next sibling
	element.insertAdjacentHTML("afterend", "<p>Hello world!</p>")
```	

### scrollIntoView()方法

在HTML元素上调用`scrollIntoView()`方法可以使该元素滚动至可视区域内

## 专有的扩展

还没有被引入规范中的扩展

### Document Mode (IE)

IE8引入了一个新概念称为**Document Mode**，一个页面的**Document Mode**决定了其所具有的特性，包括CSS支持性，Javascript特性以及对待`doctype`的方法。

可以强制定义页面使用某种**Document Mode**:

```html
	<meta http-equiv="X-UA-Compatible" content="IE=IEVersion">
```	

其中`IEVersion`的值有如下选择：

* `Edge` --使用最新的**Document Mode**
* 略

### children属性 (Chrome)

仅仅返回元素子节点中类型为`element`的节点

```js
	var childCount = element.children.length;
	var firstChild = element.children[0];
```

### contains()方法(IE, Chrome)

判断一个DOM元素是否包含另一个DOM元素

### innerText()

### outerText()




 