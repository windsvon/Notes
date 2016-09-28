# React组件通信

## 父子组件通信

父组件

```js
	var React = require('react');
	var ReactDOM = require('react-dom');
	var Child = require('./Child');
	
	var Parent = React.createClass({
	   getInitialState: function () {
		   	 return { name: 'Frarthur' };
	   },
		changeName: function (newName) {
		    this.setState({
		      name: newName
		    })
	   },
	   render: function () {
		    return (
		    	<Child name={this.state.name} onChange={this.changeName} />
		    );
	   }
	});
	
	ReactDOM.render(
		<Parent />, 
		document.getElementById('app')
	);
```

子组件

```js
	var React = require('react');
	
	var Child = React.createClass({
	  handleChange: function (e) {
	    var name = e.target.value;
	    this.props.onChange(name);
	  },
	  render: function () {
	    return (
	      <div>
	        <h1>
	          Hey my name is {this.props.name}!
	        </h1>
	        <select id="great-names" onChange={this.handleChange}>
	          <option value="Frarthur">
	            Frarthur
	          </option>
	
	          <option value="Gromulus">
	            Gromulus
	          </option>
	
	          <option value="Thinkpiece">
	            Thinkpiece
	          </option>
	        </select>
	      </div>
	    );
	  }
	});
	
	module.exports = Child;
```

## 子组件间通信

父组件

```js
	var React = require('react');
	var ReactDOM = require('react-dom');
	var Child = require('./Child');
	var Sibling = require('./Sibling');
	
	var Parent = React.createClass({
	  getInitialState: function () {
	    return { name: 'Frarthur' };
	  },
	  
	  changeName: function (newName) {
	    this.setState({
	      name: newName
	    });
	  },
	
	  render: function () {
	    return (
	      <div>
	        <Child onChange={this.changeName} />
	        <Sibling name={this.state.name}/>
	      </div>
	    );
	  }
	});
	
	ReactDOM.render(
	  <Parent />,
	  document.getElementById('app')
	);
```

子组件Child.js

```js
	var React = require('react');
	
	var Child = React.createClass({
	  handleChange: function (e) {
	    var name = e.target.value;
	    this.props.onChange(name);
	  },
	  
	  render: function () {
	    return (
	      <div>
	        <select 
	          id="great-names" 
	          onChange={this.handleChange}>
	          
	          <option value="Frarthur">Frarthur</option>
	          <option value="Gromulus">Gromulus</option>
	          <option value="Thinkpiece">Thinkpiece</option>
	        </select>
	      </div>
	    );
	  }
	});
	
	module.exports = Child;
```

子组件Sibling.js

```js
	var React = require('react');
	
	var Sibling = React.createClass({
	  render: function () {
			var name = this.props.name;
	    return (
	      <div>
	        <h1>Hey, my name is {name}!</h1>
	        <h2>Don't you think {name} is the prettiest name ever?</h2>
	        <h2>Sure am glad that my parents picked {name}!</h2>
	      </div>
	    );
	  }
	});
	
	module.exports = Sibling;
```
