# core factory

定义了jQuery构造函数，返回值 实例化jQuery对象



#### 无new化

```js
// Define a local copy of jQuery
jQuery = function( selector, context ) {

	// The jQuery object is actually just the init constructor 'enhanced'
	// Need init if jQuery is called (just allow error to be thrown if not included)
	return new jQuery.fn.init( selector, context );
}
```





