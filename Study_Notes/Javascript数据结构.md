#Javascript实现数据结构

##实现列表类

```js
	function List() {
		this.listSize = 0;
		this.pos = 0;
		this.dataStore = [];
		this.clear = clear;
		this.find = find;
		this.toString = toString;
		this.insert = insert;
		this.append = append;
		this.remove = remove;
		this.front = front;
		this.end = end;
		this.prev = prev;
		this.next = next;
		this.length = length;
		this.currPos = currPos;
		this.moveTo = moveTo;
		this.getElement = getElement;
		this.contains = contains;
	}
```

###1、append: 给列表添加元素

```js
	function append(element) {
		this.dataStore[this.listSize++] = element;
	}
```

###2、find: 在列表中查找某一元素

```js
	function find(element) {
		for(var i = 0; i < this.dataStore.length; ++i) {
			if (this.dataStore[i] == element) {
				return i
			}	
		}
		return -1
	}
```

###3、remove: 从列表中删除元素

```js
	function remove(element) {
		var foundAt = this.find(element);
		if (foundAt > -1) {
			rhis.dataStore.splice(foundAt, 1);
			--this.listSize;
			return true;
		}
		return false;
	}
```

###4、length: 列表中有多少个元素

```js
	function length() {
		return this.listSize;
	}
```

###5、toString: 显示列表中的元素

```js
	function toString() {
		return this.dataStore;
	}
```

###6、insert: 向列表中插入一个元素

```js
	function insert(element, after) {
		var insertPos = this.find(after);
		if (insertPos > -1) {
			this.dataStore.splice(insertPos+1, 0, element);
			++this.listSize;
			return true;
		}
		return false;
	}
```

###7、clear: 清空列表中所有的元素

```js
	function clear() {
		delete this.dataStore;
		this.dataStore = [];
		this.listSize = this.pos = 0;
	}
```

###8、contains: 判断给定值是否在列表中

```js
	function contains(element) {
		for (var i = 0; i < this.dataStore.length; ++i) {
			if (this.dataStore[i] == element {
				return true;
			}
		}
		return false;
	}
```

###9、遍历列表

```js
	function front() {
		this.pos = 0;
	}
	
	function end() {
		this.pos = this.listSize-1;
	}
	
	function prev() {
		if (this.pos > 0) {
			--this.pos;
		}
	}
	
	function next() {
		if (this.pos < this.listSize-1) {
			++this.pos;
		}
	}
	
	function currPos () {
		return this.pos
	}
	
	function moveTo(position) {
		this.pos = position;
	}
	
	function getElement() {
		return this.dataSotre[this.pos]
	}
```

* 使用迭代器访问列表

```js
	for(names.front(); names.currPos() < names.length(); names.next()) {
		print(names.getElement());
	}
```

##实现栈
栈 = "First-In-Last-Out"

```js
	function Stack() {
		this.dataStorage = [];
		this.pop = 0;
		this.push = push;
		this.pop = pop;
		this.peek = peek;
	}
	
	function push(element) {
		this.dataStore[this.top++] = element;
	}
	
	function pop() {
		return this.dataStore[--this.top];
	}
	
	function peek() {
		return this.dataStore[this.top-1];
	}
	
	function length() {
		return this.top;
	}
	
	function clear() {
		this.top == 0;
	}
```

* 应用1：将数字转化为二至九进制的数字：

```js
	function mulBase(num, base) {
		var s = new Stack();
		do {
			s.push(num % base);
			num = Math.floor(num /= base);
		} while (num > 0);
		var converted = " ";
		while (s.length() > 0) {
			converted += s.pop();
		}
		return converted;
	}
```

* 应用2： 递归演示

实现阶乘的传统方法：

```js
	function factorial(n) {
		if (n === 0) {
			return 1;
		}
		else {
			return n * factorial(n-1);
		}
	}
```

使用栈模拟递归过程：

```js
	function fact(n) {
		var s  = new Stack();
		while (n > 1) {
			s.push(n--);
		}
		var product = 1;
		while (s.length() > 0) {
			product *= s.pop()
		}
		return product;
	}
```

##实现队列
队列 = “First-In-First-Out”

```js
	function Queue() {
		this.dataStore = [];
		this.enqueue = enqueue;
		this.dequeue = dequeue;
		this.front = front;
		this.back = back;
		this.toString = toString;
		this.empty = empty;
	}
	
	function enqueue(element) {
		this.dataStore.push(element);
	}
	
	function dequeue() {
		return this.dataStore.shift();
	}
	
	function front() {
		return this.dataStore[0];
	}
	
	function back() {
		return this.dataStore[this.dataStore.length - 1];
	}
	
	function toString() {
		var retStr = "";
		for (var i = 0; i < this.dataStore.length; ++i) {
			retStr += this.dataStore[i] + "\n";
		}
		return retStr;
	}
	
	function empty() {
		if (this.dataStore.length == 0) {
			return true;
		}
		else {
			return false;
		}
	}
```

* 应用1： 使用队列对数据进行排序（基数排序）

	原理：将数据集扫描两次，第一次按个位上的数字进行排序，第二次按十位上的数字进行排序。
	
	第一步： 定义一个函数，根据相应位（个位或十位）上的数值，将数字分配到相应队列的函数：
	
	```js
		function distribute (nums, queues, n, digit) {
			for (var i = 0; i < n; ++i) {
				if (digit ==1) {
					queues[nums[i]%10].enqueue(nums[i]);
				}
				else {
					queues[Math.floor(nums[i] / 10)].enqueue(nums[i]);
				}
			}
		}
	```
	
	第二步： 定义一个从队列中收集数字的函数：
	
	```js
		function collect (queues, nums) {
			var i = 0;
			for (var digit = 0; digit < 10; ++digit) {
				while (!queues[digit].empty()) {
					nums[i++] = queues[digit].dequeue();
				}
			}
		}
	```
	
	主程序
	
	```js
		var queues = [];
		for (var i = 0; i < 10; ++i) {
			queues[i] = new Queue();
		}
		var nums = [];
		for (var i = 0; i < 10; ++i) {
			nums[i] = Math.floor(Math.floor(Math.random()*101));
		}
		console.log(nums);
		distribute(nums, queues, 10, 1);
		collect(queues, nums);
		console.log(nums);
		distribute(nums, queues, 10, 10);
		collect(queues, nums);
		console.log(nums)
	```
