# ECMAScript6


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
### 字符串的扩展

### Interator(遍历器)和for...of循环
每次调用`next`方法，都会返回`数据结构`当前成员的信息(返回一个包含`value`和`done`两个属性对象，done是一个布尔值，表示遍历是否结束)
```
var it = makeIterator(['a','b']);

it.next() // { value: "a", done: false }
it.next() // { value: "b", done: false }
it.next() // { value: undefined, done: true }

function makeIterator(array) {
  var nextIndex = 0;
  return {
    next: function() {
      return nextIndex < array.length ?
        {value: array[nextIndex++]} :
        {done:true};
    }
  };
}
```
Iteraror接口的目的，是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环，使用此循环时该循环会自动去寻找Iterator接口
ES6 规定，默认的`Iterator`接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）。`Symbol.iterator`属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。  
Symbol.iterator，它是一个表达式，返回Symbol对象的iterator属性，这是一个预定义好的、类型为 Symbol 的特殊值  
```
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```
原生具备Iterator接口的数据结构，除原生以外都需要自己在Symbol.iterator属性上面部署
```
-Array
-Map
-Set
-String
-TypedArray
-函数的 arguments 对象
-NodeList 对象
```
对象(Object)没有默认部署Iterator接口，因为哪个属性先后遍历是不确定都，遍历器是一种线性处理，严格地说对对象部署遍历器接口不是很必要，因为ES6提供了原生对Map结构

```
function Obj(value){
  this.value = value;
  this.next = null;
}
Obj.prototype[Symbol.iterator] = function() {
  var iterator = { next:next };
  
  var that = this;
  
  function next() {
    if (that) {
      var value = that.value;
      current = that.next;
      return { done: false, value: value };
    } else {
      return { done: true };
    }
  }
  return iterator;
}

var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);

one.next = two;
two.next = three;

for (var i of one){
  console.log(i); // 1, 2, 3
}
```
上面代码首先在构造函数的原型链上部署`Symbol.iterator`方法，调用该方法会返回遍历器对象`iterator`，调用该对象`next`
方法，在返回一个值的同时，自动将内部指针移到下一个实例
### Symbol 
是一种独一无二的类型undefine null string boolean number object symbol  
对象的属性名可以有两种类型，一种是原来有的string，另一种就是symbol类型
作为属性名的symbol,由于每一个Symbol值都是不相等的，这意味着Symbol值可以作为标识符，用于***对象***的属性名，就能保证不会出现同名的属性。这对于一个由多个模块构成的情况非常有利，能防止某一个键被不小心改写或覆盖。
```
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 2
let a = {
  [mySymbol]:'Hello!'
};

// 3
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

a[mySymbol] // "Hello!"
```
在对象内部，使用Symbol值定义属性时，Symbol值必须放在方括号之中。
```
let s = Symbol();

let obj = {
  [s]: function (arg) { ... }
};

obj[s](123);
```
### 浏览器同源策略  
浏览器限制了从同一个源加载的文档或脚本如何与另一个源的资源进行交互，用于隔离潜在恶意文件的重要安全机制。  
如果两个页面的协议，主机，端口都相同，则称两个页面具有相同的源
1
