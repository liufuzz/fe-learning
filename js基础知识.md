



## js基础知识点

#### 原始类型

> 原始类型有哪几种？ null是对象吗？

在js中存在着7种原始值，分别是：

* boolean
* null
* undefined
* number
* string
* symbol
* bigInt

原始值存储的都是值，是没有函数可以调用的， 比如**undefined.toString()**会报错。

* Object 对象类型

对于**null**来说，它不是对象类型。虽然**typeof null** 会输出**object**， 但这是js中的一个bug。

******



#### 对象类型

> 对象类型和原始类型有什么不同？

​    原始类型存储的是值，对象类型存储的是地址（指针）。

> 函数的参数是对象会发生什么问题？

```javascript
function test(person) {
  person.age = 26
  person = {
    name: 'yyy',
    age: 30
  }

  return person
}
const p1 = {
  name: 'yck',
  age: 25
}
const p2 = test(p1)
console.log(p1) // -> {name: "yck", age: 26}
console.log(p2) // -> {name: "yyy", age: 30}
```

*****



#### typeof vs instanceof

> **typeof** 是否能正确判断类型？ **instanceof**能正确判断对象类型的原理是什么？

**typeof** 对于原始类型来说，除了 **null** 都可以显示正确的类型 

```
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
```

**typeof** 对于对象来说，除了函数都会显示**object**， 所以说**typeof**并不能准确判断变量到底是什么类型

```
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```

要判断一个对象的正确类型，可以使用**instanceof**, 内部机制是通过原型链来判断的。

```
const Person = function()
const p1 = new Person()
p1 instanceof Person // true

var str = 'hello world'
str instanceof String // false
```

*****



#### this

> 如何正确判断this? 

```
function foo() {
	console.log(this.a)
}
var a = 1
foo()

const obj = {
	a: 2,
	foo: foo
}

obj.foo()

const c = new Foo()

// 1
// 2
// undefined
```

* 对于直接调用foo来说， 不管foo被放在什么地方，this一定是window
* 对于obj.foo()来说， 谁调用了，this就是谁。
* 对于new 的方式， this被永远绑定在了c 上面，不会被任何方式改变this。

> this图解

<img src="file:///F:/%E6%95%99%E7%A8%8B/%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95%E4%B9%8B%E9%81%93/2-JS%20%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E7%82%B9%E5%8F%8A%E5%B8%B8%E8%80%83%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%88%E4%B8%80%EF%BC%89_files/16717eaf3383aae8" />