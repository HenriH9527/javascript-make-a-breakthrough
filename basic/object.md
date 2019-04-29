# 创建对象的多种方式及优缺点

### 1、工厂模式

```javascript
function creartePerson(name) {
    var o = new Object();
    o.name = name;
    o.getName = function() {
        console.log(this.name);
    }
    
    return o;
}

var person1 = createPerson('kiven');

```



缺点： 对象无法识别，因为所有的实例都指向一个原型



## 2.构造函数模式

```javascript
function Person(name) {
	this.name = name;
    this.getName = function() {
        console.log(this.name);
    }
}

var person1 = new Person('kiven');

```



优点：实例可以识别为一个特定的类型

缺点：每次创建实例时，每个方法都要被创建一次



### 2.1 构造函数模式优化

```javascript
function Person(name) {
    this,name = name;
    this.getName = getName;
}

function getName() {
    console.log(this.name);
}

var person1 = new Person('kiven'); 
```



优点： 解决了每个方法都要被创建的问题

缺点： 没有封装



### 3.原型模式

```javascript
function Person(name) {
    
}

Person.prototype.name = 'kiven';
Person.prototype.getName = function() {
    console.log(this.name);
}

var person1 = new Person();

```



优点：方法不会重新创建

缺点：

1. 所有的属性和方法都共享
2. 不能初始化参数



### 3.1 原型模式优化

```javascript
function Person(name) {

}

Person.prototype = {
    name: 'kiven',
    getName: function() {
        console.log(this.name);
    }
}

var person1 = new Person();

```



优点： 封装性好了一点

缺点：重写了原型,丢失了 constructor 属性



### 3.2 原型模式优化

```javascript
function Person() {
    
}
Person.prototype = {
    constructor: Person,
    name: 'kiven',
    getName: function() {
        console.log(this.name);
    }
}

var person1 = new Person();

```



优点：实例可以通过 constructor 属性找到所属的构造函数

缺点：原型模式该有的缺点还是有



### 4. 组合模式

构造函数模式与原型模式双剑合璧

```javascript
function Person(name) {
	this.name = name;	
}

Perons.prototype = {
    constructor: Person,
    getName: function() {
        console.log(this.name);
    }
}

var person1 = new Person('kiven');

```



优点：该共享的共享，该私有的私有，使用最广泛的模式

缺点：需要更好的封装性



### 4.1 动态原型模式

```javascript
function Person(name) {
    this.name = name;
    if (typeof this.getName !== "function") {
        	Person.prototype.getName = function() {
                console.log(this,name);
            }
        }
}

var person1 = new Perons('kiven');
```



> 注意：使用动态原型模式时，不能用对象字面量重写原型



如果非要用字面量方式写代码，可以尝试如下代码

```javascript
function Perons(name) {
    this.name = name;
    if (typeof this.getName !== "function") {
        	Person.prototype = {
                constructor: Person,
                getName: function() {
                    console.log(this.name);
                }
            }
        	return new Person(name);
        }
}
```



### 5.1 寄生构造函数

```javascript
function Person(name) {
	var o = new Object();
    o.name = name;
    0.getName = function() {
        console.log(this.name);
    }
    
    return o;
}

var person1 = new Person('kiven');
console.log(person1 instanceof Person); //false
console.log(person1 instanceof Object); //true

```

### 5.2 稳妥构造函数模式



```javascript
function person(name){
    var o = new Object();
    o.sayName = function(){
        console.log(name);
    };
    return o;
}
var person1 = person('kevin');

person1.sayName(); // kevin

person1.name = "daisy";

person1.sayName(); // kevin

console.log(person1.name); // daisy
```

所谓稳妥对象，指的是没有公共属性，而且其方法也不引用 this 的对象。

与寄生构造函数模式有两点不同：

1. 新创建的实例方法不引用 this
2. 不使用 new 操作符调用构造函数

稳妥对象最适合在一些安全的环境中。

稳妥构造函数模式也跟工厂模式一样，无法识别对象所属类型。