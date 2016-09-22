#ES6中新的方法函数总结

##1、Object.assign()

**作用：**将一系列源对象的内容添加到目标对象中。

**例子：**

```js
var o1 = { a: 1 };
var o2 = { b: 2 };
var 03 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj);
console.log(o1); // { a: 1, b: 2, c: 3 }
```

##2、Array.prototype.find()

**作用：**遍历数组，对数组中的每一个值调用callback函数，返回第一个使callback值为true的数组元素。

**例子：**

```js
[1, 3, 4, 5, 6, 3].find(x => x > 2)

//3
```
如果在ES5中，只能通过filter来实现：

```js
[ 1, 3, 4, 5, 6, 3].filter(function (x) { return x > 2; })[0];
```

##3、String.prototype.repeat()

**作用：**使一个字符串重复若干次，返回新数组，原数组不会被改变；

**例子：**

```js
var str = "haha";

var newStr = str.repeat(3);

console.log(newStr); //hahahahaha
console.log(str); //haha
```
##4、String的查询方法

在ES5中，字符串查询只能通过indexOf来实现，ES6中引入了一系列新方法。

```js
"hello".startsWith("ello", 1) // true
"hello".endsWith("hell", 4)   // true
"hello".includes("ell")       // true
"hello".includes("ell", 1)    // true
"hello".includes("ell", 2)    // false
```
对应的ES5写法为：

```js
"hello".indexOf("ello") === 1;    // true
"hello".indexOf("hell") === (4 - "hell".length); // true
"hello".indexOf("ell") !== -1;    // true
"hello".indexOf("ell", 1) !== -1; // true
"hello".indexOf("ell", 2) !== -1; // false
```
##5、数值类型的判断

ES6提供了数值类型判断的便捷方法：

```js
Number.isNaN(42) === false
Number.isNaN(NaN) === true

Number.isFinite(Infinity) === false
Number.isFinite(-Infinity) === false
Number.isFinite(NaN) === false
Number.isFinite(123) === true
```

##6、数值安全范围的判断

Javascript中规定了数值的安全大小范围，即2的53次方，ES6中提供了判断数值大小是否安全的便捷方法：

```js
Number.isSafeInteger(42) === true
Number.isSafeInteger(9007199254740992) === false
```
在ES5中，相同的判断需要定义一个辅助函数：

```js
function isSafeInteger (n) {
    return (
           typeof n === 'number'
        && Math.round(n) === n
        && -(Math.pow(2, 53) - 1) <= n
        && n <= (Math.pow(2, 53) - 1)
    );
}
isSafeInteger(42) === true;
isSafeInteger(9007199254740992) === false;
```

##7、Number.EPSILON

可以返回一个标准Epsilon值，其代表了两个不同的数字的最小的差。换句话说，如果两个数字的差小于Epsilon, 即可以认为这两个数字相等。这个属性可被用来比较两个float的大小:

```js
console.log(0.1 + 0.2 === 0.3) // false
console.log(Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON) // true
```

##8、Number.trunc()

可以用来获取小数的整数部分，避免了使用`Math.ceil`和`Math.floor`:

```js 
console.log(Math.trunc(42.7)) // 42
console.log(Math.trunc( 0.1)) // 0
console.log(Math.trunc(-0.1)) // -0
```

##9、Number.sign()
可以用来判断一个数字的正负号，同时可以提示非数字以及“正负0”的情况

```js
console.log(Math.sign(7))   // 1
console.log(Math.sign(0))   // 0
console.log(Math.sign(-0))  // -0
console.log(Math.sign(-7))  // -1
console.log(Math.sign(NaN)) // NaN
```

##10、Object.is()
