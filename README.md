# ECMAScript6
### 2.let和const
`let`只在自己所在的代码块(块级作用域)内生效，`const`声明一个只读常量，一旦声明常量的值就不能改变  

### 3.变量的解构
`let [a, b, c] = [1, 2, 3]`  
  
```
let [x, ,y] = [1, 2, 3]
x // 1
y // 3
```

解构赋值允许指定默认值

`let [foo = true] = []`

### this的详解
this指的是函数运行时所在的环境，object和class的区别，比如class是人，而我是一个object
```
var obj = {
            x: 0,
            f1: function () {
                console.log(this.x);
            }
        }
        var f1 = obj.f1;
        var x = 1;

        obj.f1(); //0
        f1(); //1
```
对于obj.f1()来说f1的运行的环境时obj，内部使用了关键字this，对f1来说，运行的环境是全局
![1.png](https://upload-images.jianshu.io/upload_images/15578663-12534be407d5d8f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
因为函数是一个单独的值，所以函数运行时有不同的环境（上下文）执行，我们可以在函数内部引用当前环境的其他变量，this可以使函数获取当前运行环境  
var f1 = obj.f1后f1 ()直接指向函数本身，obj.f1()是通过obj内存的地址找到的f1

### import {}
在JavaScript ES6中，export与export default均可用于导出常量、函数、文件、模块等，你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用，但在一个文件或模块中，export、import可以有多个，export default仅有一个  
通过export方式导出，在导入时要加{ },并且export能导出变量表达式  
  
### 箭头函数
引入箭头函数有两个方面的作用：更简短的函数且不绑定this  
箭头函数不会创建自己的this,它只会从自己的作用域链的上一层继承this
```
// 更短的函数
var elements = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

elements.map(function(element) { 
  return element.length; 
}); // 返回数组：[8, 6, 7, 9]

// 上面的普通函数可以改写成如下的箭头函数
elements.map((element) => {
  return element.length;
}); // [8, 6, 7, 9]

// 当箭头函数只有一个参数时，可以省略参数的圆括号
elements.map(element => {
 return element.length;
}); // [8, 6, 7, 9]

// 当箭头函数的函数体只有一个 `return` 语句时，可以省略 `return` 关键字和方法体的花括号
elements.map(element => element.length); // [8, 6, 7, 9]

// 在这个例子中，因为我们只需要 `length` 属性，所以可以使用参数解构
// 需要注意的是字符串 `"length"` 是我们想要获得的属性的名称，而 `lengthFooBArX` 则只是个变量名，
// 可以替换成任意合法的变量名
elements.map(({ "length": lengthFooBArX }) => lengthFooBArX); // [8, 6, 7, 9]

```
在箭头函数出现之前，每个新定义的函数都有它自己的 this值（在构造函数的情况下是一个新对象，在严格模式的函数调用中为 undefined，如果该函数被作为“对象方法”调用则为基础对象等）。This被证明是令人厌烦的面向对象风格的编程。
```
// 不绑定this
在ECMAScript 3/5中，通过将this值分配给封闭的变量，可以解决this问题。

function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    //  回调引用的是`that`变量, 其值是预期的对象. 
    that.age++;
  }, 1000);
}
或者，可以创建绑定函数，以便将预先分配的this值传递到绑定的目标函数（上述示例中的growUp()函数）。

箭头函数不会创建自己的this,它只会从自己的作用域链的上一层继承this。因此，在下面的代码中，传递给setInterval的函数内的this与封闭函数中的this值相同：

function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| 正确地指向 p 实例
  }, 1000);
}

var p = new Person();
```
箭头函数可以有一个“简写体”或常见的“块体”。  

在一个简写体中，只需要一个表达式，并附加一个隐式的返回值。在块体中，必须使用明确的return语句。  
```
var func = x => x * x;                  
// 简写函数 省略return

var func = (x, y) => { return x + y; }; 
//常规编写 明确的返回值
```
记住用params => {object:literal}这种简单的语法返回对象字面量是行不通的。
```
var func = () => { foo: 1 };               
// Calling func() returns undefined!

var func = () => { foo: function() {} };   
// SyntaxError: function statement requires a name
```
这是因为花括号（{} ）里面的代码被解析为一系列语句（即 foo 被认为是一个标签，而非对象字面量的组成部分）。

所以，记得用圆括号把对象字面量包起来：
```
var func = () => ({foo: 1});
```
示例
```
var simple = a => a > 15 ? 15 : a; 
simple(16); // 15
simple(10); // 10
```
