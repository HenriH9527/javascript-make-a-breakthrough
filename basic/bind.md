# **bind**的模拟实现

### bind的介绍：

> bind() 方法会创建一个新函数，当这个新函数被调用时，bind()的第一个参数将作为它运行时的 this ，之后的一系列参数将会在传递的实参前传入作为它的参数。(来自MDN)

由此我们可以首先得出 bind 函数的两个特点：

1. 返回一个参数
2. 可以传入参数

### 返回函数的模拟实现

从第一个特点开始，我们举个例子：

```javascript
var foo = {
	value : 1
}

functuib bar() {
	console.log(this.value);
}

//返回一个函数
var bindFoo = bar.bind(foo);

bindFoo();  //1
```

关于指定 this 的指向，我们可以用 call 和 apply 实现，我们来看第一版的代码：

```javascript
//第一版
Function.prototype.bind2 = function(context) {
  var self = this;
  return function() {
    return self.apply(context);
  }
}
```

此外，之所以 `return self.apply(context)` ,是考虑到绑定函数可能是有返回值的，依然是这个例子：

```JavaScript
var foo = {
  value： 1
}

function bar() {
  return this.vaule;
}
var bindFoo = bar.bind(foo);

console.log(bindFoo());  //1
```

### 传参的模拟实现

接下来看第二点，可以传入参数，有两点需要考虑：

1. bind的时候，需不需要传参
2. 执行bind返回的函数的时候，可不可以传参

我们继续看下个例子：

```javascript
var foo = {
  value: 1
}

function bar(name, age) {
  console.log(this.value);
  console.log(name);
  console.log(age);
}

var bindFoo = bar.bind(foo, 'daisy');
bindFoo(18);
//1
//daisy
//18
```

函数需要传 name 和 age 两个参数，而这两个参数一个是在绑定的时候传入的，而另一个是在执行返回的函数的时候传入的，但是都有了正确的输出，我们接下来用 arguments 进行处理

```javascript
//第二版
Function.prototype.bind2 = function(context) {
  var self = this;
  //获取bind2函数从第二个参数到最后一个参数
  var args = Array.prototype.slice.call(arguments, 1);
  
  return function() {
    //这个时候的arguments是指bind返回的函数传入的参数
    var bindArgs = Array.prototype.slice.call(arguemnts);
    return self.apply(context, args.concat(bindArgs));
  }
}
```

### 构造函数效果的模拟实现

完成了这两点，最难的部分到啦！因为bind还有一个特点，就是

> 一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。

也就是说当 bind 返回的函数作为构造函数的时候，bind 是指定的值会失效，但传入的参数依然生效。举个例子：

```javascript
var value = 2;

var foo = {
  value: 1
}

function bar(name, age) {
 	this.habit = 'shopping';
  console.log(this.value)
  console.log(name);
  console.log(age);
}

bar.prototype.friend  = 'kiven';

var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18');
// undefined
// daisy
// 18
console.log(obj.habit);
console.log(obj.friedn);
//shopping
//kiven
```

注意：尽管在全局和 foo 都声明了 value 值，最后依然返回了 undefined, 说明绑定 this 失效了，如果大家了解 new 的模拟实现，就会知道这个时候的 this 已经指向了 obj。

所以我们可以通过修改返回的函数的原型来实现：

```javascript
//第三版
Function.prototype.bind2 = function(context) {
  var self = this;
  var args = Array.prototype.slice.call(arguments, 1);
  
  var fBound = function() {
    var bindArgs = Array.prototype.slice.call(arguments);
    //当作为构造函数时，此时结果为 true,将绑定函数的 this 指向该实例，可以让实例获得来自绑定函数的值
    //以上面的 demo 为例，如果改成 `this instanceof fBound ? null : context`，实例只是一个空对象，将 null 改成 this ，实例会具有 habit 属性
    //当做普通函数时，this 指向 window ，此时结果为 false ，将绑定函数的 this 指向 context
    return self.apply(this instanceof fBound ? this : context, args.concat(bindArgs));
  }
  //修改返回函数的 prototype 为绑定函数的 prototype, 实例就可以继承函数的原型中的值
  fBound.prototype = this.prototype;
  return fBound;
}
```

### 构造函数效果的优化实现

在上个例子中，直接将 `fBound.prototype = this.prototype`  ，直接修改 `fBound.prototype`的时候，也会直接修改绑定函数的 prototype 。这个时候，我们可以通过一个空函数来进行中转：

```javascript
//第四版
Function.prototype.bind2 = function(context) {
  var self = this;
  var args = Array.prototype.slice.call(arguments);
  
  var fNOP = function() {};
  
  var fBound = function() {
    var bindArgs = Array.prototype.slice.call(arguments);
    return self.apply(this instanceof fNOP ? this : context(bindArgs));
  }
  
  fNOP.prototype = this.prototype;
  fBound.prototype = new fNOP();
  return fBound;
}
```

### 最终代码

```javascript
Function.prototype.bind2 = function(context){
	if (typeof this !== "function") {
      	throw new Error('Function.prototype.bind- what is trying to bound is not callabled');
      }
  
  var self  = this;
  var args = Array.prototype.slice.call(arguments);
  
  var fNOP = function() {}
  
  var fBound = function() {
    var bindArgs = Array.prototype.slice.call(arguments);
    return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
  }
  
  fNOP.prototype = this.prototype;
  fBound.prototype = new fNOP();
  return fBound;
}
```

