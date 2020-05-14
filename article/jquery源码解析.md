# jQuery 源码解析  3.3.1 基础


	




#### 生成jquery对象 
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
		// 防止window 还没有定义 比如在nodejs下 
		} )( typeof window !== "undefined" ? window : this, function( window, noGlobal ) {
		
		// Edge <= 12 - 13+, Firefox <=18 - 45+, IE 10 - 11, Safari 5.1 - 9+, iOS 6 - 9.1
		// throw exceptions when non-strict code (e.g., ASP.NET 4.5) accesses strict mode
		// arguments.callee.caller (trac-13335). But as of jQuery 3.0 (2016), strict mode should be common
		// enough that all such attempts are guarded in a try block.


#### 定义公共的属性和方法
		
		// 直接跳过
		var arr = [];
		
		var document = window.document;
		
		var getProto = Object.getPrototypeOf;
		
		var slice = arr.slice;
		
		var concat = arr.concat;
		
		var push = arr.push;
		
		var indexOf = arr.indexOf;
		
		var class2type = {};
		
		var toString = class2type.toString;
		
		var hasOwn = class2type.hasOwnProperty;
		
		var fnToString = hasOwn.toString;
		
		var ObjectFunctionString = fnToString.call( Object );
		
		var support = {};


#### isFunction

		var isFunction = function isFunction( obj ) {

	      // Support: Chrome <=57, Firefox <=52
	      // In some browsers, typeof returns "function" for HTML <object> elements
	      // (i.e., `typeof document.createElement( "object" ) === "function"`).
	      // We don't want to classify *any* DOM node as a function.
		 
		  // 在一些浏览器中，*特别是低版本ie下* typeof document.createElement( "object" ) === "function"
		  // 所以需要加入 额外判断  typeof obj.nodeType !== "number"
	      return typeof obj === "function" && typeof obj.nodeType !== "number";
  		};



#### isWindow
	
	// 判断是否是window对象
	var isWindow = function isWindow( obj ) {
    	return obj != null && obj === obj.window;
  	};




#### `DOMEval(暂不清楚) `


	// 全局作用域内的求值操作
	  var preservedScriptAttributes = {
	    type: true,
	    src: true,
	    noModule: true
	  };
	
	  function DOMEval( code, doc, node ) {
	    doc = doc || document;
	
	    var i,
	      script = doc.createElement( "script" );
	
	    script.text = code;
	    if ( node ) {
	      for ( i in preservedScriptAttributes ) {
	        if ( node[ i ] ) {
	          script[ i ] = node[ i ];
	        }
	      }
	    }
	    doc.head.appendChild( script ).parentNode.removeChild( script );
	  }


#### toType

		// 类型判断
		
		// line 62
		var class2type = {};
		var toString = class2type.toString;

		
		
		function toType( obj ) {
		  if ( obj == null ) {
			// 通过 '' 空字符串，免于调用toString方法
			// null 返回 'null'
		    return obj + "";
		  }
		
		  // Support: Android <=2.3 only (functionish RegExp)
 		  // 判断数据类型 使用了toString.call(obj)方法
		  return typeof obj === "object" || typeof obj === "function" ?
		    class2type[ toString.call( obj ) ] || "object" :
		    typeof obj;
		}

		// line 478
		jQuery.each( "Boolean Number String Function Array Date RegExp Object Error Symbol".split( " " ),
		function( i, name ) {
		  class2type[ "[object " + name + "]" ] = name.toLowerCase();
		} );
		
		// class2type 赋值后
		class2type = {
			"[object Array]": "array",
			"[object Boolean]": "boolean",
			"[object Date]": "date",
			"[object Error]": "error",
			"[object Function]": "function",
			"[object Number]": "number",
			"[object Object]": "object",
			"[object RegExp]": "regexp",
			"[object String]": "string",
			"[object Symbol]": "symbol",
		}
	 
#### [链式调用](https://www.cnblogs.com/aaronjs/p/3278578.html#!comments)
		
		
		 jQuery = function( selector, context ) {

		    // The jQuery object is actually just the init constructor 'enhanced'
		    // Need init if jQuery is called (just allow error to be thrown if not included)

			// 本质上链式调用通过返回一个实例化的init对象，init对象中通过return this 来返回本身使其可以链式调用自己的方法，
			// 再将jQuery的原型传递给jQuery.prototype.init.prototype 也就是覆盖到jQuery.fn.init.prototype上将方法和属性传递回去
		    return new jQuery.fn.init( selector, context );
		  }

		 // jquery.fn 只是一个对jquery.prototype的引用
		 
		jQuery.fn = jQuery.prototype = {

			  // The current version of jQuery being used
			  jquery: version,
			
			  constructor: jQuery,
			
			  // The default length of a jQuery object is 0
			  length: 0,
			
			  toArray: function() {
			    return slice.call( this );
		 }

		// line 2911
		

		init = jQuery.fn.init = function( selector, context, root ) {
		    var match, elem;
		
		    // HANDLE: $(""), $(null), $(undefined), $(false)
		    if ( !selector ) {
			  // 这里很重要 通过 return this 来传递
		      return this;
		    }
		
		    // Method init() accepts an alternate rootjQuery
		    // so migrate can support jQuery.sub (gh-2101)
		    root = root || rootjQuery;
			.........
		
		}
		// 下条等式转换一下就是 jQuery.prototype.init.prototype = jQuery.prototype
		// 将jQuery的原型传递给jQuery.prototype.init.prototype
		init.prototype = jQuery.fn;

> **jquery的链式调用**
> 
> 本质上链式调用通过返回一个实例化的init对象，init对象中通过return this 来返回本身使其可以链式调用自己的方法，
> 
> 再将jQuery的原型传递给jQuery.prototype.init.prototype 也就是覆盖到jQuery.fn.init.prototype上将方法和属性传递回去
> 详情 请见[链式调用](https://www.cnblogs.com/aaronjs/p/3278578.html#!comments)


#### extend
	jQuery.extend = jQuery.fn.extend = function() {
	  var options, name, src, copy, copyIsArray, clone,
	    // target 即传递的第一个参数
		target = arguments[ 0 ] || {},
	    i = 1,
	    length = arguments.length,
		// 深拷贝判断条件
	    deep = false;
	
	  // Handle a deep copy situation
	  if ( typeof target === "boolean" ) {
	    deep = target;
	
	    // Skip the boolean and the target
	    target = arguments[ i ] || {};
	    i++;
	  }
	
	  // Handle case when target is a string or something (possible in deep copy)
	  if ( typeof target !== "object" && !isFunction( target ) ) {
	    target = {};
	  }
	  // 判断是否为扩展方法 若是 将this赋值给 target
	  // Extend jQuery itself if only one argument is passed
	  if ( i === length ) {
	    target = this;
	    i--;
	  }
	
	  for ( ; i < length; i++ ) {
	
	    // Only deal with non-null/undefined values
	    if ( ( options = arguments[ i ] ) != null ) {
	
	      // Extend the base object
	      for ( name in options ) {
	        src = target[ name ];
	        copy = options[ name ];
	
	        // Prevent never-ending loop
	        if ( target === copy ) {
	          continue;
	        }


			// 递归判断条件 即 深拷贝判断条件
			// 1 deep 不为false，也就是前面的 target === "boolean" 或者  target !== "object" && !isFunction( target )
			// 2 copy 存在
			// 3 继承的对象中的属性 copy 必须为对象或数组
	        // Recurse if we're merging plain objects or arrays
	        if ( deep && copy && ( jQuery.isPlainObject( copy ) ||
	          ( copyIsArray = Array.isArray( copy ) ) ) ) {
	
	          if ( copyIsArray ) {
	            copyIsArray = false;
	            clone = src && Array.isArray( src ) ? src : [];
	
	          } else {
	            clone = src && jQuery.isPlainObject( src ) ? src : {};
	          }
	
	          // Never move original objects, clone them
	          target[ name ] = jQuery.extend( deep, clone, copy );
	
	        // Don't bring in undefined values
	        } else if ( copy !== undefined ) {
	          target[ name ] = copy;
	        }
	      }
	    }
	  }
	
	  // Return the modified object
	  return target;
	};


	// 调用方法
	$.extend(a: function(){
		//jquery 扩展方法
		...
	})
	// 浅拷贝
	$.extend(object1, object2);

	// 深拷贝 会在函数内部判断第一个参数是否为true，若是则进行深拷贝递归
	$.extend(true，object1, object2);
	

#### sizzle jQuery选择器引擎 line 510 ~ 2752 


#### isArrayLike


	判断是否为类数组结构
	
	function isArrayLike( obj ) {

	  // Support: real iOS 8.2 only (not reproducible in simulator)
	  // `in` check used to prevent JIT error (gh-2145)
	  // hasOwn isn't used here due to false negatives
	  // regarding Nodelist length in IE
	  var length = !!obj && "length" in obj && obj.length,
	    type = toType( obj );
	
	  if ( isFunction( obj ) || isWindow( obj ) ) {
	    return false;
	  }
	
	  return type === "array" || length === 0 ||
	    typeof length === "number" && length > 0 && ( length - 1 ) in obj;
	}


 **类数组结构几大条件**
 
1. 不为null和undefined
2. 对象包含length属性
3. 对象不是一个函数也不是window对象
4. 对象为array或者length为0  不满足时 一定满足length属性为number并且length>0以及 length-1 为对象属性

**length-1 为对象属性** 有点难理解
	 
		其实就是
	 ('string'.length -1) in Object('string') 
	 
	 // return true
	 	 
	

#### `isPlainObject`
-----------------

	  isPlainObject: function( obj ) {
	    var proto, Ctor;
	
		// 使用 toString.call(obj) 返回 '[object object]'
        // jquery 中type判断 中也是使用的toString.call()
	    // Detect obvious negatives
	    // Use toString instead of jQuery.type to catch host objects
	    if ( !obj || toString.call( obj ) !== "[object Object]" ) {
	      return false;
	    }
	
		// getProto =  Object.getPrototypeOf
		// 当使用 Object.create( null ) 对象为空 
		// getProto返回null 即 Object.create( null )没有继承的原型属性

	    proto = getProto( obj );
		
	    // Objects with no prototype (e.g., `Object.create( null )`) are plain
	    if ( !proto ) {
	      return true;
	    }
		
		//  var toString = class2type.toString;
		//	var hasOwn = class2type.hasOwnProperty;
		//	var fnToString = hasOwn.toString;
		//  var ObjectFunctionString = fnToString.call( Object );

		// class2type.hasOwnProperty.call( Object.getPrototypeOf(obj), "constructor" ) && Object.getPrototypeOf(obj).constructor
		
	    // Objects with prototype are plain iff they were constructed by a global Object function
	    Ctor = hasOwn.call( proto, "constructor" ) && proto.constructor;
	    return typeof Ctor === "function" && fnToString.call( Ctor ) === ObjectFunctionString;
	  }
		
> 一个对象只有通过{}直接创建、 new Object() 或者通过 Object.create(null) 方式创建那么它才是一个纯粹的对象

#### isEmptyObject
		
		// 直接循环是否具有属性 
	  isEmptyObject: function( obj ) {

	    /* eslint-disable no-unused-vars */
	    // See https://github.com/eslint/eslint/issues/6125
	    var name;
	
	    for ( name in obj ) {
	      return false;
	    }
	    return true;
	  },


#### each
	// line 348
	 each: function( obj, callback ) {
	    var length, i = 0;
	
	    if ( isArrayLike( obj ) ) {
	      length = obj.length;
	      for ( ; i < length; i++ ) {
			// 每一次循环将 obj[i] 绑定到了 callback的this上，返回false时跳出循环
	        if ( callback.call( obj[ i ], i, obj[ i ] ) === false ) {
	          break;
	        }
	      }
	    } else {
	      for ( i in obj ) {
	        if ( callback.call( obj[ i ], i, obj[ i ] ) === false ) {
	          break;
	        }
	      }
	    }
	    return obj;
	  }

	// 调用 
	$.each(obj,function(){
		...			
	})

### find filter is not

##### jquery.merge 方法将两个参数合并到一个对象中 `line 400` 
	// line 175
	  pushStack: function( elems ) {

    	// 调用方法 jQuery.merge 构建新的jquery对象
	    // Build a new jQuery matched element set
	    var ret = jQuery.merge( this.constructor(), elems );

		//在新Query对象ret上设置属性prevObject，指向当前jQuery对象，从而形成一个链式栈
	    // Add the old object onto the stack (as a reference)
	    ret.prevObject = this;
	
	    // Return the newly-formed element set
	    return ret;
	  },	



	// line 2813

	function winnow( elements, qualifier, not ) {
	  if ( isFunction( qualifier ) ) {
	    return jQuery.grep( elements, function( elem, i ) {
	      return !!qualifier.call( elem, i, elem ) !== not;
	    } );
	  }
	
	  // Single element
	  if ( qualifier.nodeType ) {
	    return jQuery.grep( elements, function( elem ) {
	      return ( elem === qualifier ) !== not;
	    } );
	  }
	
	  // Arraylike of elements (jQuery, arguments, Array)
	  if ( typeof qualifier !== "string" ) {
	    return jQuery.grep( elements, function( elem ) {
	      return ( indexOf.call( qualifier, elem ) > -1 ) !== not;
	    } );
	  }
	
	  // Filtered directly for both simple and complex selectors
	  return jQuery.filter( qualifier, elements, not );
	}
	
	jQuery.filter = function( expr, elems, not ) {
	  var elem = elems[ 0 ];
	
	  if ( not ) {
	    expr = ":not(" + expr + ")";
	  }
	
	  if ( elems.length === 1 && elem.nodeType === 1 ) {
	    return jQuery.find.matchesSelector( elem, expr ) ? [ elem ] : [];
	  }
	
	  return jQuery.find.matches( expr, jQuery.grep( elems, function( elem ) {
	    return elem.nodeType === 1;
	  } ) );
	};
	
> 方法继承 find filter not is


	// 将方法传递给 jquery.prototype
	jQuery.fn.extend( {
	  find: function( selector ) {
	    var i, ret,
	      len = this.length,
	      self = this;
	
	    if ( typeof selector !== "string" ) {
	      return this.pushStack( jQuery( selector ).filter( function() {
	        for ( i = 0; i < len; i++ ) {
	          if ( jQuery.contains( self[ i ], this ) ) {
	            return true;
	          }
	        }
	      } ) );
	    }
	
	    ret = this.pushStack( [] );
	
	    for ( i = 0; i < len; i++ ) {
	      jQuery.find( selector, self[ i ], ret );
	    }
	
	    return len > 1 ? jQuery.uniqueSort( ret ) : ret;
	  },
	  filter: function( selector ) {
	    return this.pushStack( winnow( this, selector || [], false ) );
	  },
	  not: function( selector ) {
	    return this.pushStack( winnow( this, selector || [], true ) );
	  },
	  is: function( selector ) {
	    return !!winnow(
	      this,
	
	      // If this is a positional/relative selector, check membership in the returned set
	      // so $("p:first").is("p:last") won't return true for a doc with two "p".
	      typeof selector === "string" && rneedsContext.test( selector ) ?
	        jQuery( selector ) :
	        selector || [],
	      false
	    ).length;
	  }
	} );

	