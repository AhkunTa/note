#JavaScript 相关

### 1 字符串转数组

#### （1） 数组转字符串

需要将数组元素用某个字符连接成字符串，示例代码如下：
var a, b,c;
a = new Array(a,b,c,d,e);
b = a.join('-'); //a-b-c-d-e 使用-拼接数组元素
c = a.join(''); //abcde

#### （2）字符串转数组

实现方法为将字符串按某个字符切割成若干个字符串，并以数组形式返回，示例代码如下：
var str = 'ab+c+de';
var a = str.split('+'); // [ab, c, de]
var b = str.split(''); //[a, b, +, c, +, d, e]

---

### 2 JavaScript 中~~符号

    ~~true == 1
    ~~false == 0
    ~~"" == 0
    ~~[] == 0

    ~~undefined ==0
    ~~!undefined == 1
    ~~null == 0
    ~~!null == 1

    操作符~， 是按位取反的意思，表面上~~（取反再取反）没有意义，实际上在JS中可以将浮点数变成整数。
    在js中可以取代Math.floor() 并且性能更好

---

### 3 字符串转数字 ####（1）字符串转数字

    1
    parseInt('123')            // 123
    parseInt('0xA')           // 10
    parseInt('22.5')           // 22
    parseInt("AF",  16);  //returns  175
    parseInt("10",  2);  //returns  2
    parseInt("10",  8);  //returns  8
    parseInt("10",  10);  //returns  10

    parseFloat  同理

    2 强制类型转换
    Boolean(value)——把给定的值转换成Boolean型；
    Number(value)——把给定的值转换成数字（可以是整数或浮点数）；
    String(value)——把给定的值转换成字符串

    3 利用弱类型转换
    var  str= '012.345 ';
    var  x  =  str-0;
    x  =  x*1;

    1.value.toString()
    2."" + value
    3.String(value)
    var n = 17
    n.toString(2)           //  "10001"
    "0" + n.toString(8)     //  "021"
    "0x" + n.toString(16)   //  "0x11"

#### （2）数字转字符串

1 tostring（）

    n = 100
    x = n.toString() => "100"或是（100）toString()  //tostring（2/16/8）还可以实现进制的转化

    2 数字+任意字符串“”

    var n = 1234;
    var nn = 1234+""

---

### 4 js 每四位添加空格 ####首先只能输入数字 将现有的输入内容去掉所有空格，然后再每四个数字分组加入空格，最后将末尾的空白替换为空字符串

cardNum = cardNum.replace(/[^\d\s]/g,"").replace(/(\s)/g,'').replace(/(\d{4})/g,'$1 ').replace(/\s*$/,'');
$1 匹配括号里的内容
/(\d{4})(abc)/ 匹配 1234abc
$1 为 1234  
 $2 为 abc

---

### 5 js 链接跳转

    js跳转页面与打开新窗口的方法
    1.超链接<a href="http://www" title="">Welcome</a>
    等效于js代码
    window.location.href="http://www.";     //在同当前窗口中打开窗口
    2.超链接<a href="http://www." title="" target="_blank">Welcome</a>
    等效于js代码
    window.open("http://www.");                 //在另外新建窗口中打开窗口

---

### 6 数组去重

1 遍历数组法
var arr=[2,8,5,0,5,2,6,7,2];
function unique1(arr){
var hash=[];
for (var i = 0; i < arr.length; i++) {
if(hash.indexOf(arr[i])==-1){
hash.push(arr[i]);
}
}
return hash;
}

    2 数组下标判断法
    function unique2(arr){
      var hash=[];
      for (var i = 0; i < arr.length; i++) {
         if(arr.indexOf(arr[i])==i){
          hash.push(arr[i]);
         }
      }
      return hash;
    }

    3 排序后相邻去除法
    function unique3(arr){
      arr.sort();
      var hash=[arr[0]];
      for (var i = 1; i < arr.length; i++) {
         if(arr[i]!=hash[hash.length-1]){
          hash.push(arr[i]);
         }
      }
      return hash;
    }

    4 ES6实现
    function unique4(arr){
      var x = new Set(arr);
     return [...x];
    }

    5 优化遍历数组法（推荐）
    function unique5(arr){
      var hash=[];
      for (var i = 0; i < arr.length; i++) {
        for (var j = i+1; j < arr.length; j++) {
          if(arr[i]===arr[j]){
            ++i;
          }
        }
          hash.push(arr[i]);
      }
      return hash;
    }


    6 js hash 去重
    // 去重只需遍历要去重的数组次数 优化性能
    function unique6(arr) {

    	var hash={},newArr=[];
    	for(let key in arr){
    		if(!hash[arr[key]]){
    			hash[arr[key]] = true;
    			newArr.push(arr[key])
    		}
    	}
    	return newArr;
    }

---

### 7 js date 函数

    var date = new date()
    date.getTime()               // 获取当前时间的总时间（毫秒数） 1555928482366
    date.toLocaleDateString()   // 2019/4/22
    date.toLocaleString()       // 2019/4/22 下午6:20:09
    其他的一些获取时间的日期，分钟 详情见api文档


    var date1=new Date();        //开始时间
    var date2=new Date();        //结束时间
    var date3=date2.getTime()-date1.getTime()  //时间差的毫秒数
    date3/(1000*24*3600)         //获取剩下的天数

---

### 8 js reduce() 函数

#### arr.reduce(callback[, initialValue])

####callback 传递四个参数：

1.  Accumulator (acc) (累计器) 累计器累计回调的返回值; 它是上一次调用回调时返回的累积值
2.  Current Value (cur) (当前值) 数组中正在处理的元素。
3.  Current Index (idx) (当前索引) 数组中正在处理的当前元素的索引
4.  Source Array (src) (源数组)

#### initialValue 可选

> 作为第一次调用 callback 函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

    例子：
    [0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
      return accumulator + currentValue;
    });

    callback	accumulator	currentValue	currentIndex	array			return value
    first call		0			1				1		[0, 1, 2, 3, 4]			1
    second call		1			2				2		[0, 1, 2, 3, 4]			3
    third call		3			3				3		[0, 1, 2, 3, 4]			6
    fourth call		6			4				4		[0, 1, 2, 3, 4]			10

---

### 9 幂运算

1.                       **num

        2**2  //4
        2**3  //8
        3**3  //27

2.  Math.pow(2,2)
    Math.pow(2,2) //4
    Math.pow(10,2) //100

---

### 10 Json 处理

- JSON.stringify() 方法是将一个 JavaScript 值(对象或者数组)转换为一个 JSON 字符串，如果指定了 replacer 是一个函数，则可以替换值，或者如果指定了 replacer 是一个数组，可选的仅包括指定的属性。

      	JSON.stringify(value[, replacer [, space]])

      	//举个栗子
      	JSON.stringify({ a: 2 }, null, " ");   // '{\n "a": 2\n}'

  [具体请见 MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

- JSON.parse() 方法用来解析 JSON 字符串，构造由字符串描述的 JavaScript 值或对象。提供可选的 reviver 函数用以在返回之前对所得到的对象执行变换(操作)。
  JSON.parse(text[, reviver])

      text
      要被解析成JavaScript值的字符串，关于JSON的语法格式,请参考: JSON。
      reviver 可选
      转换器, 如果传入该参数(函数)，可以用来修改解析生成的原始值，调用时机在parse函数返回之前。

---

### 11 vue 原生监听 scroll 事件

- 获取 scrollTop

      网页被卷起来的高度/宽度（即浏览器滚动条滚动后隐藏的页面内容高度）

      (javascript)        document.documentElement.scrollTop //firefox

      (javascript)        document.documentElement.scrollLeft //firefox

      (javascript)        document.body.scrollTop //IE

      (javascript)        document.body.scrollLeft //IE

      (jqurey)             $(window).scrollTop()

      (jqurey)             $(window).scrollLeft()

      网页工作区域的高度和宽度

      (javascript)       document.documentElement.clientHeight// IE firefox

      (jqurey)             $(window).height()

      元素距离文档顶端和左边的偏移值(包含元素本身)

      (javascript)        DOM元素对象.offsetTop //IE firefox

      (javascript)        DOM元素对象.offsetLeft //IE firefox

      (jqurey)             jq对象.offset().top

      (jqurey)             jq对象.offset().left


      通用方式 let scrollTop =  document.body.scrollTop|| document.documentElement.scrollTop||window.pageYOffset

- 确保元素内部为 overflow:visible 或 unset
- 在 mounted 中挂载 监听 scroll 事件

      mounted(){
      	this.$nextTick(()=>{
      		// handleScroll为处理事件方法
      		// 如果监听外层为overflow auto 监听失效
      		window.addEventListener('scroll',this.handleScroll)
      	})
      }

---

### 12 移动端获可视区域代码

    	document.documentElement.clientHeight,
    	document.body.clientHeight
    	window.innerHeight

---

### 13 js 防抖和节流

- #### debounce 防抖

      function debounce(func, wait) {
          let timeout = null
          return function() {
              let _this = this, args = arguments;
              clearTimeout(timeout);
              timeout = setTimeout(function() {
                  func.apply(_this, args);
              }, wait);
          };
      };

      function debounce(func, wait) {
          let time = false
          return function() {
      		if(!time) {

      		}
              let _this = this, args = arguments;
              clearTimeout(timeout);
              timeout = setTimeout(function() {
                  func.apply(_this, args);
              }, wait);
          };
      };

- #### throttle 节流

      // func 方法函数
      // wait 延时
      // mustRun 必须的间隔

      function throttle(func,wait,mustRun){
      	if(!mustRun){
      		mustRun = wait
      	}

      	let timeout,
      		startTime = Date.now();
      	return function(){
      		let _this = this;
      		    args = arguments;
      			curTime = Date.now();
      		clearTimeout(timeout)
      		if(curTime - startTime >= mustRun){
      			func.apply(_this,args);
      		}else {
      			timeout = setTimeout(func,wait)
      		}
      	}
      }

        // 节流函数 延迟实现
        function throttle(callback,wait){
      	let timer = null;
      	return function(...args){
      		let _this = this;
      		if(!timer){
      			timer = setTimeout(()=>{
      				callback.apply(_this,args)
      				timer = null;
      			})
      		}
      	}
        }
        // 节流函数 date实现
        function throttle(callback,wait){
      	let pre = 0;
      	return function(...args){
      		let _this = this;
      		let now = new Date();
      		if(now - pre >= wait){
      			callback.apply(_this,args)
      			pre = now;
      		}
      	}
        }

---

### 14 js `Math.round` `toFixed()` 保留数据四舍五入问题

- ##### 银行家算法 ： "四舍六入五成双" 后面数字<=4 时舍去 >=6 时进位 当=5 时 判断
  - 当 5 后面存在位数时 进位 1.551 保留一位时进位
  - 当 5 后不存在位数时 需要分两种情况来讲 1.55 保留一位判断：
    - 5 前为奇数，舍 5 入 1 进位 1.55 进位
    - 5 前为偶数，舍 5 不进 舍去（0 是偶数） 1.45 舍去

> **但经测试 在 chrome 下`toFixed()`方法不满足银行家算法**

    chrome版本 75.0.3770.100（正式版本） （64 位）
    以下为toFixed()保留一位后的数字
    满足四舍六入在五后存在位数时也满足
    但在五后不存在位数时不满足银行家算法

    在火狐65.0.1 (32 位) 上与chrome一致

    保留前 保留后 银行家算法应该求得的值
    1.05   1.1   1.0
    1.15   1.1 	 1.2
    1.25   1.3   1.2
    1.35   1.4   1.4
    1.45   1.4   1.4
    1.55   1.6   1.6
    1.65   1.6   1.6
    1.75   1.8   1.8
    1.85   1.9   1.8
    1.95   1.9   2.0
    1.54   1.5   1.5
    1.56   1.6   1.6

    ie 下toFixed()满足四舍五入
    1.05  1.1
    1.15  1.2
    1.25  1.3
    1.35  1.4
    1.45  1.5
    1.55  1.6
    1.65  1.7
    1.75  1.8
    1.85  1.9
    1.95  2.0
    1.54  1.5
    1.56  1.6

##### 解决方法

- 用`Math.round`方法

      Math.round(number * 10) / 10
      保留会有精度问题 如保留一位 正整数 1.0时不会有小数点后一位

- 加上一个很小的数 因为在 chrome 下满足 1.5500001 进位效果
  (5.175 + 0.00001).toFixed(2)
- 最推荐的方法 两者混用 **经试验错误移除**

      // 错误代码
      // 试验8765.335 在chrome下保留两位为 8765.33 错误
      (Math.round(1.35 * 10) /10).toFixed(1)

- 最推荐的方法 [stackoverflow 解决方法](https://stackoverflow.com/questions/11832914/round-to-at-most-2-decimal-places-only-if-necessary)
  function roundNumber(num, scale) {
  if(!("" + num).includes("e")) {
  return +(Math.round(num + "e+" + scale) + "e-" + scale);
  } else {
  var arr = ("" + num).split("e");
  var sig = ""
  if(+arr[1] + scale > 0) {
  sig = "+";
  }
  return +(Math.round(+arr[0] + "e" + sig + (+arr[1] + scale)) + "e-" + scale);
  }
  }
- 还有网上一些巨长的代码 ~

---

### 15 部分浏览器对于相同请求，请求缓存不更新数据 （没错 ie 说的就是你，部分 ios Webkit 内核）

- **给请求的内容加一个随机字符**

      _noCache: `${Date.now()}${parseInt(Math.random() * 1000)}`
      或者给url加上类似随机字段

---

###16 js 中`for in`和`for of`

- 简单来说 `for in` 遍历 key 而 `for of` 遍历 value

##### 官方文档这么解释

> for...in 语句以任意顺序迭代对象的可枚举属性。
>
> for...of 语句遍历可迭代对象定义要迭代的数据。

> for in 在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

- **for...of 循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。这一点跟 for...in 循环也不一样。 如下**

      let arr = [3, 5, 7];
      arr.foo = 'hello';

      for (let i in arr) {
        console.log(i); // "0", "1", "2", "foo"
      }

      for (let i of arr) {
        console.log(i); //  "3", "5", "7"
      }

**总结**

1.  推荐在循环对象属性的时候，使用 for...in,在遍历数组的时候的时候使用 for...of
2.  for...of 不能循环普通的对象，需要通过和 Object.keys()搭配使用
3.  for...of 的兼容性不是很好，推荐还是使用 for...in [兼容性点这里](https://caniuse.com/#search=for%20...%20of)

### 17 js 中判断数据类型方法 toString.call(obj)

        jquery 中判断类型方法

    	jQuery.each( "Boolean Number String Function Array Date RegExp Object Error Symbol".split( " " ),
    	function( i, name ) {
    	  class2type[ "[object " + name + "]" ] = name.toLowerCase();
    	} );
    	function toType( obj ) {
    	  if ( obj == null ) {
    	    return obj + "";
    	  }

    	  // Support: Android <=2.3 only (functionish RegExp)
    	  return typeof obj === "object" || typeof obj === "function" ?
    	    class2type[ toString.call( obj ) ] || "object" :
    	    typeof obj;
    	}


    		{
    		"[object Array]": "array",
    		"[object Boolean]": "boolean",
    		"[object Date]": "date",
    		"[object Error]": "error",
    		"[object Function]": "function",
    		"[object Number]": "number",
    		"[object Object]": "object",
    		"[object RegExp]": "regexp",
    		"[object String]": "string",
    		"[object Symbol]": "symbol"
    		}

### 18 Array.prototype.map() && Array.prototype.forEach()

#### 1.map

    map() 方法创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。

    callback
    生成新数组元素的函数，使用三个参数：
    	currentValue
    		callback 数组中正在处理的当前元素。
    	index可选
    		callback 数组中正在处理的当前元素的索引。
    	array可选
    		map 方法调用的数组。
    thisArg可选
    	执行 callback 函数时值被用作this。

arr..map(function(currentValue,index,array){
console.log(this) // thisArg
},thisArg)

> 注 1 因为 map 生成一个新数组，当你不打算使用返回的新数组却使用 map 是违背设计初衷的，请用 forEach 或者 for-of 替代。你不该使用 map: A)你不打算使用返回的新数组，或/且 B) 你没有从回调函数中返回值

> 注 2 如果使用箭头函数表达式来传入函数参数， thisArg 参数会被忽略，因为箭头函数在词法上绑定了 this 值。

> map 方法处理数组元素的范围是在 callback 方法第一次调用之前就已经确定了。**调用 map 方法之后追加的数组元素不会被 callback 访问**。如果存在的数组元素改变了，那么传给 callback 的值是 map 访问该元素时的值。**在 map 函数调用后但在访问该元素前**，该元素被删除的话，则无法被访问到。

#### 2.forEach 参数传递和 map 一样

### 19 js 数据类型判断

1.  null `===`

        obj === null

2.  undefined `typeof`

> 和 === 的区别 当 obj 都没申明时 === 会报 is not defined 错误
> typeof 则不会

    	typeof obj === undefined

3 基本数据类型判断`toString.call(obj)` jquery 内源码

    function toType( obj ) {
      if ( obj == null ) {
    	// 通过 '' 空字符串，免于调用toString方法
    	// null undefined
        return obj + "";
      }
      return typeof obj === "object" || typeof obj === "function" ?
        class2type[ toString.call( obj ) ] || "object" :
        typeof obj;
    }
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

### 20 js '==' 运算符

| #   | 参数 1                                | 参数 2                                    | 转换                                                                                                |
| --- | ------------------------------------- | ----------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| 1   | null                                  | undefined                                 | 不转换返回 true                                                                                     |
| 2   | null 或 undefined                     | 其他任何非 null 或 undefined 的类型       | 不转换返回 false                                                                                    |
| 3   | 基础数据类型 String Boolean Number 等 | 基础数据类型 String Boolean Number 等     | 转换为 Number 类型                                                                                  |
| 4   | 基础数据类型 String Boolean Number 等 | 引用数据类型 Date 对象                    | 将基础数据类型转换为数字；将 Date 对象转换为原始类型（优先尝试 toString 方法，再尝试 valueOf 方法） |
| 5   | 基础数据类型 String Boolean Number 等 | 引用数据类型 非 Date 对象 Array Object 等 | 将基础数据类型转换为数字；将引用数据类型转换（优先尝试 valueOf 方法，再尝试 toString 方法）         |
| 6   | 引用数据类型                          | 引用数据类型                              | 判断对象的引用的指针                                                                                |

### 21 Number 超过最大安全数

> Number 类型大于 1e21 时， Number 类型会自动转换为科学计数法

> IEEE 754 规定了四种表示浮点数值的方式：单精确度（32 位）、双精确度（64 位）、延伸单精确度（43 比特以上，很少使用）与延伸双精确度（79 比特以上，通常以 80 位实现）。js 使用双精确值即 64 位

1. Number.MAX_SAFE_INTEGER 为 9007199254740991 即 2\*\*53 -1
2. 首先 js 中 Number 实际存储都是通过 8 字节 double 浮点型 表示的
3. double 浮点数结构如下
4. 1 位符号位
5. 11 位指数位
6. 52 位尾数位
7. 浮点型在保存数字的时候做了规格化处理，小数点前保留一位， 规格化后始终是 1.\*\*\* x 2 \*\* 53 , 节省了 1 bit，这个 1 并不需要保存
   2\*\*53===2\*\*53+1 // true
   尾数都有 53 位，但只要有 52 个空！ 它的处理办法是 忽略第 53 位

### 22 全局修改扩大字体

```javascript
function fontSizeBigger(mult) {
  let size = [];
  document
    .querySelector("body")
    .querySelectorAll("*")
    .forEach(item => {
      let fontSize =
        getComputedStyle(item).getPropertyValue("font-size") || "16px";
      size.push({
        ele: item,
        fontSize: fontSize,
      });
    });
  size.forEach(item => {
    let fontSizeArr = item.fontSize.replace(/^(\d+)(.+)/g, "$1|$2").split("|");
    item.ele.style.fontSize = fontSizeArr[0] * mult + fontSizeArr[1];
  });
  size.length = 0;
}
```

### 23 复制文本到剪贴板

```javascript
function copy(node) {
  // node dom 节点 或纯文本
  var range = document.createRange();
  var selection = document.getSelection();
  if (
    typeof HTMLElement == "object"
      ? node instanceof HTMLElement
      : typeof node == "object" &&
        node.nodeType == 1 &&
        typeof node.nodeName === "string"
  ) {
    selection.removeAllRanges();
    range.selectNodeContents(node);
  } else if (typeof node == "string") {
    var spanCopy = document.createElement("span");
    spanCopy.innerText = node;
    spanCopy.style.opacity = "0";
    spanCopy.style.diaplay = "absolute";
    spanCopy.style.top = "0";
    document.body.appendChild(spanCopy);
    selection.removeAllRanges();
    range.selectNodeContents(spanCopy);
  } else {
    console.log("传入未定义参数失效");
  }
  selection.addRange(range);
  document.execCommand("copy");
  if (spanCopy) {
    spanCopy.remove();
  }
}
copy(document.querySelector("#id"));
// 或
copy("复制文本");
```
