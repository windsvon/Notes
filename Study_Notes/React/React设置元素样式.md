# React设置元素样式

##行内设置

```js
	var React = require('react');
	var ReactDOM = require('react-dom');
	
	var styleMe = <h1 style={{ background: 'lightblue', color: 'darkred' }}>Please style me!  I am so bland!</h1>;
	
	ReactDOM.render(
		styleMe, 
		document.getElementById('app')
	);
```

##通过对象变量设置

```js
	var React = require('react');
	var ReactDOM = require('react-dom');
	var styles = {
	  background: 'lightblue',
	  color: 'darkred',
	  marginTop: "100px",
	  fontSize: "50px"
	};
	
	var styleMe = <h1 style={styles}>Please style me!  I am so bland!</h1>;
	
	ReactDOM.render(
		styleMe, 
		document.getElementById('app')
	);
```

**注意css样式名称的写法与标准写法的差异**