在src/core/instance/index.js中

```js
if (process.env.NODE_ENV !== 'production' &&
!(this instanceof Vue)
) {
    warn('Vue is a constructor and should be called with the `new` keyword')
 }
```

这里通过this instanceof Vue来判断有没有用new关键词调用，为什么可以这么判断？我们分别了解一下this和instanceof的用法

## this

在 JavaScript 中，this 是动态绑定，或称为运行期绑定的，它可以是全局对象、当前对象或者任意对象，这取决于函数的调用方式。函数的调用有以下几种方式：作为对象方法调用，作为函数调用，作为构造函数调用，和使用 apply 或 call 调用。

**1、作为对象方法调用**

```js
var point = { 
    x : 0, 
    y : 0, 
    moveTo : function(x, y) { 
        this.x = this.x + x; 
        this.y = this.y + y; 
    } 
}; 
```

point.moveTo(1, 1)//this 绑定到当前对象，即 point 对象

**2、作为函数调用**

```js
function makeNoSense(y) { 
    this.x = y; 
} 
 
makeNoSense(5); 
x;// 调用函数的对象是window,所以x 已经成为一个值为 5 的全局变量
```

下面，我们看另一种情况

```js
var point = { 
    x : 0, 
    y : 0, 
    moveTo : function(x, y) { 
        // 内部函数
        var moveX = function(x) { 
        this.x = x;//this 绑定到了哪里？
       }; 
       // 内部函数
       var moveY = function(y) { 
       this.y = y;//this 绑定到了哪里？
       }; 
     
       moveX(x); 
       moveY(y); 
       } 
}; 
point.moveTo(1, 1); 
point.x; //==>0 
point.y; //==>0 
x; //==>1 
y; //==>1
```

this除了指向它的直接调用者外，还有一种情况就是如果没有明确的调用对象的时候，将对函数的this使用默认绑定：绑定到全局的window对象。

**3、作为构造函数调用**

```js
function Point(x, y){ 
   this.x = x; 
   this.y = y; 
}
var test = new Point(1, 2)
```

我们需要理解的是，new运算符做了什么：

```js
第一步: 创建一个空的对象，{}。
第二步: 链接该对象（即设置该对象的构造函数）到另一个对象，即o.\__proto__ == Point.prototype。
第三步: 将步骤1新创建的对象作为this的上下文
第四步: 如果该函数没有返回对象，则返回this
```

**4、使用 apply 或 call 调用**

apply和call可以切换函数执行的上下文环境（context）

```js
function add(x, y) {
    console.log(x + y)
}

function del(x, y) {
    console.log(x - y)
}

add.call(del, 3, 1) // 4
```

## instanceof

1、通常来讲，使用 instanceof 就是判断一个实例是否属于某种类型，比如：

```js
// 判断 foo 是否是 Foo 类的实例
function Foo(){} 
var foo = new Foo(); 
console.log(foo instanceof Foo)//true
```

2、另外，更重要的一点是 instanceof 可以在继承关系中用来判断一个实例是否属于它的父类型。例如：

```js
// 判断 foo 是否是 Foo 类的实例 , 并且是否是其父类型的实例
function Aoo(){} 
function Foo(){} 
Foo.prototype = new Aoo();//JavaScript 原型继承
 
var foo = new Foo(); 
console.log(foo instanceof Foo)//true 
console.log(foo instanceof Aoo)//true
```

上面的代码中是判断了一层继承关系中的父类，在多层继承关系中，instanceof 运算符同样适用。

3、ECMAScript中instanceof的定义

> 11.8.6 The instanceof operator
> The production RelationalExpression:

```js
 RelationalExpression instanceof ShiftExpression is evaluated as follows: 
 1. Evaluate RelationalExpression. 
 2. Call GetValue(Result(1)).// 调用 GetValue 方法得到 Result(1) 的值，设为 Result(2) 
 3. Evaluate ShiftExpression. 
 4. Call GetValue(Result(3)).// 同理，这里设为 Result(4) 
 5. If Result(4) is not an object, throw a TypeError exception.// 如果 Result(4) 不是 object，
                                                                //抛出异常
 /* 如果 Result(4) 没有 [[HasInstance]] 方法，抛出异常。规范中的所有 [[...]] 方法或者属性都是内部的，
在 JavaScript 中不能直接使用。并且规范中说明，只有 Function 对象实现了 [[HasInstance]] 方法。
所以这里可以简单的理解为：如果 Result(4) 不是 Function 对象，抛出异常 */ 
 6. If Result(4) does not have a [[HasInstance]] method, 
   throw a TypeError exception. 
 // 相当于这样调用：Result(4).[[HasInstance]](Result(2)) 
 7. Call the [[HasInstance]] method of Result(4) with parameter Result(2). 
 8. Return Result(7). 
 // 相关的 HasInstance 方法定义
 15.3.5.3 [[HasInstance]] (V) 
 Assume F is a Function object.// 这里 F 就是上面的 Result(4)，V 是 Result(2) 
 When the [[HasInstance]] method of F is called with value V, 
     the following steps are taken: 
 1. If V is not an object, return false.// 如果 V 不是 object，直接返回 false 
 2. Call the [[Get]] method of F with property name "prototype".// 用 [[Get]] 方法取 
                                                                // F 的 prototype 属性
 3. Let O be Result(2).//O = F.[[Get]]("prototype") 
 4. If O is not an object, throw a TypeError exception. 
 5. Let V be the value of the [[Prototype]] property of V.//V = V.[[Prototype]] 
 6. If V is null, return false. 
 // 这里是关键，如果 O 和 V 引用的是同一个对象，则返回 true；否则，到 Step 8 返回 Step 5 继续循环
 7. If O and V refer to the same object or if they refer to objects 
   joined to each other (section 13.1.2), return true. 
 8. Go to step 5.
 
```

翻译成 JavaScript 代码如下所示：

```js
function instance_of(L, R) {//L 表示左表达式，R 表示右表达式
 var O = R.prototype;// 取 R 的显示原型
 L = L.__proto__;// 取 L 的隐式原型
 while (true) { 
   if (L === null) 
     return false; 
   if (O === L)// 这里重点：当 O 严格等于 L 时，返回 true 
     return true; 
   L = L.__proto__; 
 } 
}
```

从代码中我们可以看到，instanceof是比较左侧的__proto__(隐式原型)和右侧的prototype(显示原型)是否相等，如果不相等，取左侧__proto__的__proto__，依次循环比较，直到取到Object.prototype.__proto__即null为止。有关__proto__和prototype请查看我这篇博客

**回到主题，this instanceof Vue我们可以这么分解：this.__proto__和Vue.prototype**

- 没有使用new

this指向window，结果为false。

- 使用了new

回到上面作为构造函数调用：

```js
    第一步: 创建一个空的对象，vat o = {}。
    第二步: 链接该对象（即设置该对象的构造函数）到另一个对象，即o.\__proto__ == Vue.prototype。
    第三步: 将步骤1新创建的对象作为this的上下文
    第四步: 如果该函数没有返回对象，则返回this
    
```

所以，结果可以看做这样：

```js
o.\__proto__ == this.\__proto__  == Vue.prototype
```

**所以如果用new操作符的话，this instanceof Vue结果为true。**







[原文链接]: https://segmentfault.com/a/1190000019017266	"从Vue源码学习JavaScript之this instanceof Vue"



