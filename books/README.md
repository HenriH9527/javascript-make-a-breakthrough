# 快速学习犀牛书：



### JavaScript概述：

> 高端的、动态的、弱类型的编程语言，非常适合面向对象和函数式的编程风格。

### JavaScript原始类型：

- 数字、字符串、布尔值
- null、undefined

### JavaScript对象类型：

> 除了数字、字符串、布尔值、Null、undefined之外就是对象了



<u>注：JavaScript可以自由的进行数据类型转换，因为JavaScript变量是无类型的，变量可以被赋予任何类型的值，同样一个变量也可以重新赋予不同类型的值。</u>



##### Math对象的属性定义的函数：

```javascript
Math.pow(2, 53)  //2的53次幂
Math.round(.6)   //四舍五入
Math.ceil(.6)    //向上求整
Math.floor(.6)   //向下求整
Math.abs(-6)     //求绝对值
Math.max(x, y, z)  //返回最大值
Math.min(x, y, z)  //返回最小值
Math.random()      //生成一个大于等于.小于1.0的伪随机数
Math.PI            //π  圆周率
Math.E             //e  自然对数的底数
Math.sqrt(3)       //3的平凡根
Math.pow(3, 1/3)   //3的立方根
Math.sin(0)        //三角函数：还有 Math.cos  Math.atan
Math.log(10)       //10的自然对数
Math.log(100)/Math.LN10      //以10为底100的对数
Math.log(512)/Math.LN2       //以2为底512的对数
Math.exp(3)                  //e的3次幂
```



<u>注：编程语言分为静态（类型）语言和动态（类型）语言，动态语言是指在运行期间才去做数据类型检查的语言。也就是说，在用动态语言编程时，永远也不用给任何变量指定数据类型。</u>



> JavaScript 是基于词法作用域的语言：通过阅读包含变量定义在内的数行源码，就能知道变量的作用域。全局变量在程序中始终都是有定义的，局部变量在声明它的函数体内以及所嵌套的函数内始终是有定义的。



### 作用域链

每一段 JavaScript 代码（全局代码或函数）都有一个与之关联的作用域链（scope chain）。这个作用域链是一个对象列表或者链表，这组对象定义了这段代码“作用域中”的变量。



#### 函数调用

如果函数使用return语句给出一个返回值，那么这个返回值就是真个调用表达式的值，否则，调用表达式的值就是 undefined。



#### 真假值

假值： false  null  undefined  0  -0  NaN  ""；

真值： 所有其他的值包括所有的对象都是真值；



#### 赋值操作符

赋值操作符的优先级很低，通常在一个较长的表达式中用到了一条赋值语句的值的时候，需要补充圆括号以保证正确的运算顺序。

赋值操作符的结合性是从右至左，也就是说，如果一个表达式中出现了多个赋值运算符，**运算顺序是从右至左**，因此，可以通过如下的方式对多个变量赋值：

```javascript
i = k = j = 0;
```

#### eval() 

当直接使用非限定的“eval”名称来调用eval()函数时，通常称为 “直接eval”(direct eval) 。直接调用eval()函数时，它总是在调用它的上下文作用域内执行。其他的简介调用则使用全局对象作为其上下文作用域，并且无法读、写、定义局部变量和函数。

```javascript
var geval = eval;                           //使用别名调用eval将是全局eval
var x = "global", y = "global";				//两个全局变量
function f() {								//函数内执行的是局部eval
    var x = "local";						//定义局部变量
    eval("x += 'changed';");				//直接eval更改了局部变量的值
    return x;								//返回更改后的局部变量
}

function g() {								//这个函数内执行了全局eval
    var y = "local";						//定义局部变量
    geval("y += 'changed';");				//简介调用改变了全局变量的值
    return y;								//返回未更改的局部变量
}

console.log(f(), x);						//输出：‘local changed global’
console.log(g(), y);						//输出：'local globalchannged'
```



#### 有副作用的操作：

赋值  递增  递减  删除等



#### 对象：

对象是 javascript 的基本数据类型。对象是一种复合值（原始值或者其他对象）聚合在一起，可通过名字访问这些值。对象也可看做是属性的无序集合，每个属性都是一个名/值对。属性名是字符串，因此我们可以把对象看成是从字符串到值的映射。



“原型式继承”是javascript的核心特征。



```javascript
var o2 = Object.create(null);    //o2不继承任何属性和方法

var o3 = Object.cteate(Object.prototype);   //o3个 new Object() 一样
```



#### 通过原型链创建一个对象

```javascript
function inherit(p){
    if(p === null) throw TypeError();
    if(Object.create) {
        return Object.create(p);
    }
    var t = typeof p;
    if (t !== "Objec" && t !== "function"){
        throw TypeError();
    }
    function f() {};
    f.prototype = f;
    return new f();
}
```



<u>**javascript 对象都是关联数组**</u>

#### getter & setter

```javascript
var p = {
    x: 1.0,
    y: 1.0,
    
    get r() {
        return Math.sqrt(this.x * this.x + this.y *this.y);
    }
    
    set r(newvalye) {
        var oldValue = Math.sqrt(this.x * this.x + this.y * this.y);
        var ratio = newvalue / oldvalue;
        this.x *= ratio;
        this.y *= ratio;
    }
	//只读存储器属性，只有getter方法
	get theta() {
        return Math.atan2(this.y, this.x);
    }
}
```



**存储器属性和数据属性一样，都是可以继承的**



#### 设置属性的特性（Object.defineProperty()）

```javascript
var o = {};

Object.defineProperty(o, "X", {
    value: 1,
    writable: true,
    enumerable: false,
    configurable: true
});

//将x从数据属性修改为存取器属性
Object.defineProperty(o, "x", {
    get: function(){
        return 0;
    }
})

//同时修改或创建多个属性，需要用到Object.defineProperties()
var p = Object.defineProperties({}, {
    x: { value: 1, writable: true, enumerable: true, configurable },
    y: { value: 2, writable: false, enumerable: true, configurable },
    r: {
        get: function() { return 0; },
        enumerable: false,
        configurable: true
    }
})
```



#### 对象的三个属性

原型： prototype

类： class

可扩展性： extensible attribute



**判断一个类**

```javascript
function classof(o){
   	if(o === null) return "NUll";
    if(0 === undefined) return "undefined";
    retrun object.prototype.toString.call(o).slice(8, -1);
}
//classof函数可以返回传递给它的任意对象的类+
```



```javascript
//在数组查找所有出现的x，并返回一个包含匹配的数组

function findall(a, x){
    var results = [];
    len = a.length;
    pos = 0;
    while(pos < len){
        pos = a.indexof(x, pos);
        if (pos === -1) break;
        results.push(pos);
        pos = pos + 1;
    }
    return results;
}
```



#### 保证类数组对象能拥有数组的方法

```javascript
Array.join = Array.join || function(a, sep) {
	return Array.prototype.join.call(a, sep);
}

Array.slice = Array.slice || function(a, from, to) {
	return Array.prototype.slice.call(a, from, to);
}

Array.map = Array.map || function(a, f, thisArg) {
	return Array.prototype.map.call(a, f, thisArg);
}
```

+ 

#### 定义一个函数来确定当前脚本运行是否为严格模式。

```javascript
var strict =  (function() { return !this; })
```



凡是没有形参的构造函数调用都可以省略圆括号。

```javascript
var o = new Object();
var o = new Object;
```



<u>*注：不定实参数的个数不能为零，arguments[]对象最适合的应用场景是在这样一类函数中，这类函数包含固定个数的命名和必需参数，以及随后个数不定的可选实参。*</u>



#### 闭包

> 函数体内部的变量都可以保存在函数作用域内，这种特性在计算机科学文献中称为闭包。
>
> 从技术角度讲，所有的javascript函数都是闭包，他们都是对象，它们都关联到作用域链。

#### 模拟bind函数

```javascript
if(!Function.prototype.bind){
	Function.prototype.bind = function(o){
        var self = this;
        boundArgs = arguments;
        
        return function() {
            var args = [];
            for(var i = 0; i < boundArgs.length; i++) args.push(boundArgs[i]);
            for(var i = 0; len = arguments.length; i < len; i++) args.push(arguments[i]);
            return self.apply(o, args);
        }
    }
}
```



### 定义类的步骤

1. 先定义一个构造函数，并设置初始化新对象的实例属性。
2. 给构造函数的prototype对象定义实例的方法。
3. 给构造函数定义类字段和类属性。



可以判断值类型的type函数

```javascript
function type(o) {
	var t, c, n;  //type class name
    
    //处理null值的特殊情形
    if(o === null) return "null";
    //另一种特殊情形，NaN和它自身不相等
    if(o !== o) return "NaN";
    
    //如果tyoeof的值不”object“,则使用这个值
    //这可以识别出原始值的类型和函数
    if((t = typeof o) !== "Object") return t;
    
    //这种方式可以识别大多的内置对象
    if(c = classof(o) !== "Object") return c;
    
    //如果对象构造函数的名字存在的话，则返回它
    if(o.constructor && typeof o.constructor === "function" && (n = o.constructor.getName()))  return n;
    
    return "Object";
}

//返回对象的类
function classof(o) {
    return Object.prototype.toString.call(o).slice(8, -1);
}

//返回函数的名字（可能是空字符串） 不是函数的话返回 null
Function.prototype.getName = function() {
    if ("name" in this) return this.name;
    return this.name = this.this.toString.match(/function\s*([^(]*)])\(/)/)[1];
}
```



