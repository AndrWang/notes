# 《JavaScript 语言精粹》

## 对象
+ JavaScript 中的对象是可变的键控集合（keyed collections）
+ JavaScript 中的对象是无类型（type-free）的
+ JavaScript 包含一种原型链的特性，允许对象继承另一个对象的属性。正确使用它可以减少对象初始化的时间和内存。
+ 取属性值，可以用 `||` 填充默认值，可以用 `&&` 避免对 undefined 取值产生异常
+ 对象通过引用来传递，直接赋值不会被复制
+ ！原型，原型链
+ 反射：`typeof`, `hasOwnProperty`
+ 枚举属性：`for in` （会包含原型链中的所有属性）
+ `delete` 可以删除对象的属性（是否可以删除来自原型链的属性？）
+ 减少全局变量污染：可以把所有全局变量用一个对象包起来（极大程度减少冲突）

## 函数
+ 函数也是对象。对象是可变的键控集合，并拥有一个连接到原型对象的隐藏连接。对象字面量产生的对象连接到 `Object.prototype`，函数对象连接到 `Function.prototype`（而它又连接到 `Object.prototype`）
+ 每个函数在创建时会附加两个隐藏属性：函数的上下文和实现函数行为的代码
+ ！函数的 `prototype` 与 `Function.prototype`，闭包
+ 调用
  + 除了形参，每个函数还接收两个附加参数，`this` 和 `arguments`
  + 函数有 4 种调用模式
  + 传参过少，缺失的值为 `undefined`，传参过多，多余的将被忽略
+ 4 函数调用模式，差异主要在如何初始化 `this` 参数
  + 方法调用模式：被保存为一个对象的属性时，这个函数被称为该对象的一个方法；调用表达式中包含提取该属性的动作（`obj.inc(1)` 或 `obj['inc'](1)`）都是方法调用；在方法中可以使用 `this` 引用包含它的对象，而 `this` 的绑定发生在调用的时候（和 Java 似乎没有区别）
  + 函数调用模式：如果函数没有被保存为一个对象的属性，就称之为函数调用；此时 `this` 被绑定到全局变量上（而不是外部函数的 `this`），这一点很不好，例如方法的内部函数无法共享方法对 对象的访问权，但可以通过以下方式实现访问：

    ~~~ javascript
    obj.double = function () {
    var that = this

    var helper = function () {
        that.val *= 2
    }

    helper()
    }
    ~~~

  + 构造器调用模式：如果在函数前面带上 `new` 来调用，那这个调用会创建一个连接到该函数的 `prototype` 成员的新对象，同时，`this` 会被绑定到新对象上；如果一个函数的目的就是为了结合 `new` 前缀来调用，就称之为 _构造器函数_，通常将这种函数保存在大写开头命名的变量中；不建议使用；
  + `apply` 调用模式：`apply` 是函数对象的方法，调用它将会执行该函数，而且 `apply` 可以传两个参数，第一个绑定到函数的 `this` 上，第二个绑定到函数的 `arguments` 上
+ `arguments` 可以访问传递给函数的所有实际参数，包括未在形式参数中声明的参数；可以用它实现变长参数，但不建议；`arguments` 不是数组，它只是有一个 `length` 属性；
+ 一个函数总会返回一个值，没有指定时，返回 `undefined`
+ 如果函数调用时在前面加上了 `new` 前缀，且返回值不是一个对象，则返回 `this`（该新对象）
+ 异常，一个 `try` 只会有一个 `catch`，多种可能的异常需要根据异常对象的属性来进行区分处理
+ 为基本类型扩充功能
+ JavaScript 没有块级作用域，函数内定义的变量在函数内任何位置都可见，所以最好在函数开头声明所有变量
+ 通过函数字面量创建的函数对象包含一个连接到外部上下文的连接，这被称作闭包
+ 内部函数可以访问在外部函数
+ 模块模式：一个定义了私有变量和函数的函数；利用闭包，创建可以访问私有变量和函数的特权函数；最后返回这个特权函数，或者把它们保存到一个可以访问到的地方

## 继承
+ 函数化
  + 以任何方式创建一个新对象
  + 有选择地定义私有实例变量和方法
  + 给新创建的对象扩充方法，这些方法有特权访问参数、第 2 步中定义的私有变量和方法
  + 返回新创建的对象