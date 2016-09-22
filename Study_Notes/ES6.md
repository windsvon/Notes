#ES6 新特性总结

##1、Template String(模板字符串）

ES6引入了一种新的字符串表达形式--模板字符串，使用反引号`` (` `) ``来表示。模板字符串带来了强大的字符创建能力，例如：

```js
	var person = "Yu"
    `Hello, ${person}! 1 + 1 = ${1 + 1}`
    
    output:
    "Hello, Yu! 1 + 1 = 2"
```
同时这种字符串支持多行输入，例如：

```js
	`Hello,
	world!`
```
在模板字符串之前添加函数名，可以使模板字符串实现类似于函数的功能。例如：

```js
	fn`Hello ${you}! You're looking ${adjective} today!`
```
可以通过声明一个名为`fn`的函数对上述字符串进行控制，这个函数如下：

```js
	fn(["Hello", "! You're looking ", " today!"], you, adjective) { //so something };
```
可以看出以上函数传入了n+1个参数，其中n为模板字符串中变量的数目，函数的第一个参数为一个数组，可以看做是字符串被变量“打断”后的内容组成的数组。

##2、Arrow Function(箭头函数)

这个新特性很容易让人联想到coffescript, 其形式如下:

```js
	x => x + 1
```
转换为ES5代码后为:

```js
	function (x) { return x + 1; }
```

箭头函数还可以解决使用this时遇到的一些麻烦，这个其实也是coffeecript中的一个特性，例如：

```js
	var foot = {
		 kick: function () {
		 	this.yelp = "Ouch!";
		 	setImmediate(function () {
		 		console.log(this.yelp);
		 	});
		 }
	};
	foot.kick();
```
以上代码无法输出的是`undefined`，因为`console.log(this.yelp)`中的`this`指代是包含它的函数，即`setImmediate`中传入的函数，而不是kick();要解决这个问题有多种方法，例如：

```js
	setImmediate(function () {
		console.log(this.yelp);
	}.bind(this));
```
或者

```js
	var that = this;
	setImmediate(function () {
	 	console.log(that.yelp);
 	})	
``` 
以上写法都比较麻烦，如果使用箭头函数，只需要：

```js
	setImmediate(
		() => console.log(this.yelp)
	)
```	

##3、Spread & Rest

对于可变数量参数的函数，例如`Math.max`,在ES5之前如果想传入数组中作为参数，需要用到`.apply`方法。而在ES6中可以直接传入`...args`作为参数，其中`args`为一个任意大小的数组。这个特性称为**Spread**，例如：

```
	var numbers = [1, 1, 2, 4, 5, 6]
	var max = Math.max(...numbers);
```

当想要定义一个可变数量参数的函数时，需要用到ES6的另一个新特性，**Rest**，例如：

```
	function sum(...args) {
		var result = 0;
		args.forEach(function (value) {
			result += value;
		});
		
		return result;
	}
	
	sum(1, 2, 3);
```

我们可以注意到在上方的例子中，`args`是一个真正的Array（可以调用forEach方法）。可见**rest**可以帮助我们将arguments转化为一个真正的数组，从而避免了使用`Array.prototype.slice.call(arguments)`,例如一个求平均值的函数可以采用如下写法：

```
	function average(...args) {
		return args.reduce(function(prev, curr) {
			return prev + curr
		})/args.length;
	}
```

##4、默认参数

在ES6中允许在定义函数时规定默认参数值，例如

```
	function sayHello(greeting = "Hello", name = "CampJS") {
		console.log(`${greeting} ${name}!`);
	}
```

当函数调用时，如果某个参数没有传入值，那么将使用默认值。需要注意的是即使传入的参数值为`null`,`false`, `""`或者`0`，也不会使用默认值。

另外，默认参数还可以传入表达式，例如

```
	function log(arg, transform = x => x) {
		console.log(transform(arg));
	}
	
	log("Hello"); // => "Hello"
	log("Hello", y => y.toUpperCase());	// => "HELLO"
```

	



	
	

	