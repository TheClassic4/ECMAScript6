# ECMAScript6


### import {}
在JavaScript ES6中，export与export default均可用于导出常量、函数、文件、模块等，你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用，但在一个文件或模块中，export、import可以有多个，export default仅有一个  
通过export方式导出，在导入时要加{ },并且export能导出变量表达式  
  

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

### 浏览器同源策略  
浏览器限制了从同一个源加载的文档或脚本如何与另一个源的资源进行交互，用于隔离潜在恶意文件的重要安全机制。  
如果两个页面的协议，主机，端口都相同，则称两个页面具有相同的源
1
