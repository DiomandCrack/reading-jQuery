# wrapper

```js
( function( global, factory ) {

	"use strict";

	if ( typeof module === "object" && typeof module.exports === "object" ) {

		// For CommonJS and CommonJS-like environments where a proper `window`
		// is present, execute the factory and get jQuery.
		// For environments that do not have a `window` with a `document`
		// (such as Node.js), expose a factory as module.exports.
		// This accentuates the need for the creation of a real `window`.
		// e.g. var jQuery = require("jquery")(window);
		// See ticket #14549 for more info.
		module.exports = global.document ?
			factory( global, true ) :
			function( w ) {
				if ( !w.document ) {
					throw new Error( "jQuery requires a window with a document" );
				}
				return factory( w );
			};
	} else {
		factory( global );
	}

// Pass this if window is not defined yet
} )( typeof window !== "undefined" ? window : this, function( window, noGlobal ) {

// Edge <= 12 - 13+, Firefox <=18 - 45+, IE 10 - 11, Safari 5.1 - 9+, iOS 6 - 9.1
// throw exceptions when non-strict code (e.g., ASP.NET 4.5) accesses strict mode
// arguments.callee.caller (trac-13335). But as of jQuery 3.0 (2016), strict mode should be common
// enough that all such attempts are guarded in a try block.
"use strict";

// @CODE
// build.js inserts compiled jQuery here

return jQuery;
} );
```

## 1 匿名函数立即执行

将全局变量变成函数内的局部变量,保证在这个函数内定义的变量不会污染到全局命名空间

### 1.1 表达式

> 表达式 JavaScript中的一个短语，JavaScript解释器会将其计算出一个结果

常量和变量名也都是表达式

#### 1.1.1 函数定义表达式

函数定义表达式就是定义了一个JavaScript函数,也被称为"函数字面量"或“函数直接量”。`function`关键字后面是`()`和`{}`，函数名称标识符可有可无

#### 1.1.2 调用表达式

> 调用表达式是一种调用（或者执行）函数或方法的语法表示

一个函数表达式后面跟随`()`

### 1.2 函数声明语句

函数声明语句是用来定义函数的一种形式

关键字`function`必须加函数名称标识符加`()`紧跟`{//多条语句}`

一旦调用函数，`{}`里的语句就会被执行

### 1.3 函数立即执行

```js
(function(window, undefined) {
    var jQuery = function() {}
    // ...
    window.jQuery = window.$ = jQuery;
})(window);
```

`function`必须用`()`包裹，JavaScript中`()`里不能包含语句，JavaSctipt解释器就将`()`里的`function`解析成了函数定义表达式，**函数表达式**后面再**加`()`**就成了**调用表达式**，函数就会自动调用

如果`function`外面没有`()`，JavaScript解析器就将关键字`function`解析成了**函数声明语句**，但是函数声明语句必须带有**函数名称标识符**，所以会报错

```js
function(){//...}();
//Syntax error
```

只要能将`function`解析成表达式就能使函数自动调用
比如:

```js
!function () {
  console.log('我是匿名函数');
}();
```

~~我觉得叫匿名函数自执行不是很恰当，自执行有种自己执行自己的感觉，像递归，所以我喜欢用立即执行或立即调用~~

### 2 分析wrapper

对不同的平台加载方式不同，支持AMD和CommonJS规范

### 2.1 `window`作为实参传入

将`window`作为实参:

- 1 主要提高性能，如果`window`在函数作用域内,由于`window`是全局对象在作用域链的最顶层,查找`window`会通过作用域链一层一层向上找，影响性能

- 2 代码压缩 能节省一点空间

### 2.2 第二个参数能传`undifined`

```js
typeof window !== "undefined" ? window : this, function( window, noGlobal ){//...}
```
`undifined`不是JavaScript关键字,而是JavaScript预定义的全局变量，ES3中是可读可写的变量，ES5规定是只读的
防止某些浏览器在外部修改`undefined`












