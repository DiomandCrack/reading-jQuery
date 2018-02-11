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

## 1 匿名函数自动调用

将全局变量变成函数内的局部变量,保证在这个函数内定义的变量不会污染到全局命名空间

### 1.1 表达式

> 表达式 JavaScript中的一个短语，JavaScript解释器会将其计算出一个结果

常量和变量名也都是表达式

#### 1.1.1 函数定义表达式

函数定义表达式就是定义了一个JavaScript函数,也被称为"函数字面量"或“函数直接量”。`function`关键字后面是`()`和`{}`，函数名称标识符可有可无

#### 1.1.2 调用表达式

> 调用表达式是一种调用（或者执行）函数或方法的语法表示

一个函数表达式后面跟随`()`

### 1.2 函数自动调用

```js
(function(window, undefined) {
    var jQuery = function() {}
    // ...
    window.jQuery = window.$ = jQuery;
})(window);
```

`function`必须用`()`包裹，JavaScript中`()`里不能包含语句，JavaSctipt解释器就将`()`里的`function`解析成了函数定义表达式，**函数表达式**后面再**加`()`**就成了**调用表达式**









