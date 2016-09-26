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

* 1、append: 给列表添加元素

```js
	function append(element) {
		this.dataStore[this.listSize++] = element;
	}
```

* 2、find: 在列表中查找某一元素

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

* 3、remove: 从列表中删除元素

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

* 4、length: 列表中有多少个元素

```js
	function length() {
		return this.listSize;
	}
```

* 5、toString: 显示列表中的元素

```js
	function toString() {
		return this.dataStore;
	}
```

* 6、insert: 向列表中插入一个元素

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

* 7、clear: 清空列表中所有的元素

```js
	function clear() {
		delete this.dataStore;
		this.dataStore = [];
		this.listSize = this.pos = 0;
	}
```

* 8、contains: 判断给定值是否在列表中

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

* 9、遍历列表

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

* **应用1： 使用队列对数据进行排序（基数排序）**

	**原理：将数据集扫描两次，第一次按个位上的数字进行排序，第二次按十位上的数字进行排序。**
	
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

##实现链表

链表是由一组节点组成的集合。每个节点都使用一个对象的引用指向它的后继。指向另一个节点的引用叫做链。链表的优势主要在于可以**高效率地插入和删除节点**。

此处设计链表包含两个类，Node类用来表示节点，LinkedList类提供了插入节点、删除节点、显示列表元素的方法，以及其他一些辅助方法。

* 1、Node类

```js
	function Node(element) {
		this.element = element;
		this.next = null;
	}
```

* 2、LinkedList类

```js
	function LList() {
		this.head = new Node("head");
		this.find = find;
		this.insert = insert;
		this.remove = remove;
		this.display = display;
	}
	
	//查找节点
	
	function find(item) {
		var currNode = this.head;
		while (currNode.element != item) {
			currNode = currNode.next;
		}
		return currNode;
	}
	
	function insert(newElement, item) {
		var newNode = new Node(newElement);
		var current = this.find(item);
		newNode.next = current.next;
		current.next = newNode;
	}
	
	//删除节点时，需要找到待删除节点的前一个节点，修改其next属性，使其指向待删除节点的下一个节点。因此需要定义一个findPrevious()方法，检查每一个节点的下一个节点中是否存储着待删除数据。如果找到，返回该节点。
	
	function findPrevious(item) {
		var currNode = this.head;
		while (!(currNode.next == null) && (currNode.next.element != item)) {
			currNode = currNode.next;
		}
		return currNode;
	}
	
	// 删除节点
	
	function remove(item) {
		var prevNode = this.findPrevious(item);
		if (!(prevNode.next) == null) {
			prevNode.next = prevNode.next.next;
		}
	}
	
```

##双向链表

由于单向链表只能从头到尾遍历，因此有时需要引入双向链表，通过给Node对象增加一个属性，该属性存储指向前驱节点的链接。

```js
	function Node(element) {
		this.element = element;
		this.next = null;
		this.previous = null;
	}
	
	function LList() {
		this.head = new Node("head");]
		this.find = find;
		this.insert = insert;
		this.display = display;
		this.remove = remove;
		this.findLast = findLast;
		this.dispReverse = dispReverse;
	}
	
	function dispReverse() {
		var currNode = this.head;
		currNode = this.findLast();
		while (!(currNode.previous == null)) {
			console.log(currNode.element);
			currNode = currNode.previous;
		}
	}
	
	function findLast() {
		var currNode = this.head;
		while (!(currNode.next == null)) {
			currNode = currNode.next;
		}
		return currNode;
	}
	
	function remove(item) {
		var currNode = this.find(item);
		if (!(currNode.next == null)) {
			currNode.previous.next = currNode.next;
			currNode.next.previous = currNode.previous;
			currNode.next = null;
			currNode.previous = null;
		}
	}
	
	function display() {
		var currNode = this.head;
		while (!(currNode.next == null)) {
			console.log(currNode.next.element);
			currNode = currNode.next;
		}
	}
	
	function find(item) {
		var currNode = this.head;
		while (currNode.element != item) {
			currNode = currNode.next;
		}
		return currNode;
	}
	
	function insert(newElement, item) {
		var newNode = new Node(newElement);
		var current = this.find(item);
		newNode.next = current.next;
		newNode.previous = current;
		current.next = newNode;
	}
```

##循环链表

循环链表和单向链表唯一的区别是，在创建循环列表时，让其头节点的next属性指向它本身。

```js
	function LList() {
		this.head = new Node("head");
		this.head.next = this.head;
		this.find = find;
		this.insert = insert;
		this.display = display;
		this.findPrevious = findPrevious;
		this.remove = remove;
	}
```

循环链表中的一些方法也需要修改，避免出现死循环，例如display()方法：

```js
	function display() {
		var currNode = this.head;
		while (!(currNode.next == null) && !(currNode.next.element == “head”)) {
			console.log(currNode.next.element);
			currNode = currNode.next;
		}
	}
```



##字典

```js
	function Dictionary() {
	    this.add = add;
	    this.datastore = new Array();
	    this.find = find;
	    this.remove = remove;
	    this.showAll = showAll;
	}
	
	function add(key, value) {
	    this.datastore[key] = value;
	}
	
	function find(key) {
	    return this.datastore[key];
	}
	
	function remove(key) {
	    delete this.datastore[key];
	}
	
	function showAll() {
	    console.log(this.datastore)
	    console.log(Object.keys(this.datastore))
	    for (var key of Object.keys(this.datastore)) {
	        console.log(key + "->" + this.datastore[key])
	    }
	}
```

用数组而不是对象来储存数据是因为对象里的键值对无法排序，但是当数组中键的类型为字符串时，length属性就失效了。此时应该定义一个获取dictionary长度的方法：

```js
	function count() {
		var n = 0;
		for (var key of Object.keys(this.datastore)) {
			++n;
		}
		return n;
	}
```

清除数据中的内容：

```js
	function clear() {
		var each (var key of Object.keys(this.datastore)) {
			delete this.datastore[key];
		}
	}
```

* 为Dictionary类添加排序功能

	和length()方法一样，使用字符串作为键的数组的排序sort()方法是无效的。如果想使现实内容时结果是有序的，只能在showAll()方法中对键进行排序。

```js
	function showAll() {
		for (var key of Object.keys(this.datastore).sort()) {
			console.log(key + "->" + this.datastore[key]);
		}
	}
```

##散列（HashTable)

散列的特点：可以快速插入和取用数据，但是查找操作效率低下。

基于数组设计的散列：数组长度必须为质数，预先设定。所有元素根据和该元素对应的键，保存在数组的特定位置。使用散列表存储数据时，通过一个**散列函数**将键映射为一个数字，这个数字的范围是0到散列表的长度。

* 1、定义HashTable类

```js
	function HashTable() {
		this.table = new Array(137);
		this.simpleHash = simpleHash;
		this.showDistro = showDistro;
		this.put = put;
		//this.get = get;
	}
```

* 2、选择一个散列函数

	* 如果键是整型，最简单的散列函数就是以数组的长度对键取余。称为除留余数法。