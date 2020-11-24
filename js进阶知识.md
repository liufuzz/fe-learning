## js进阶知识



### == vs === 

> == 和 === 有什么区别？

对于 `==` 来说，如果双方类型不一致，会先进行类型转换。

对于 `===` 来说就简单多了，就是判断两者类型和值是否相同。



### 闭包

> 什么是闭包？

闭包定义：函数A内部有一个函数B， 函数B可以访问到函数A中的变量，那么函数B就是闭包。

```
function A() {
	let a = 1
	window.B = function() {
		console.log(a)
	}
}
A()
B()  // 1
```

闭包存在的意义就是让我们可以间接访问函数内部的变量。

> 经典面试题，循环中使用闭包解决 `var` 定义函数的问题

```
for (var i = 1; i <= 5; i++) {
	setTimeout(function timer() {
		console.log(i)
	}, i * 1000)
}
```

首先因为 `setTimeout` 是个异步函数，所以会先把循环全部执行完毕，这时候 `i` 就是 6 了，所以会输出一堆 6.

解决方法有三种，第一种是使用闭包方式

```
for (var i = 1; i <= 5; i++) {
	;(function(j) {
		setTimeout(function timer() {
			console.log(j)
		})
	})(i)
}
```

第二种就是使用 `setTimeout` 的第三个参数，这个参数会被当成 `timer` 函数的参数传入。

```
for (var i = 1; i <= 5; i++) {
  	setTimeout(function timer(j) {
      	console.log(j)
    },
    i * 1000,
    i
  )
}
```

第三种就是使用 `let` 定义 `i` 了来解决问题了，这个也是最为推荐的方式

```
for (let i = 1; i <= 5; i++) {
    setTimeout(function timer() {
      console.log(i)
    }, i * 1000)
}
```



### 深浅拷贝

#### 浅拷贝

首先可以使用 `Object.assign` 来解决。`Object.assign` 只会拷贝所有的属性值到新的对象中，如果属性值是对象的话，拷贝的是地址。并不是深拷贝。

```
let a = {
	age: 1
}
let b = Object.assign({}, a)
a.age = 2
console.log(b.age)

---------------------------------------------------

const obj1 = {a: {b: 1}}
const obj2 = Object.assign({}, obj1)

obj1.a.b = 2
obj2.a.b // 2
```

另外我们还可以通过展开运算符 `...` 来实现浅拷贝

```
let a = {
  age: 1
}
let b = { ...a }
a.age = 2
console.log(b.age) // 1
```

#### 深拷贝

这个问题通常可以通过 `JSON.parse(JSON.stringify(object))` 来解决。

```
let a = {
	age: 1,
	jobs: {
		first: 'FE'
	}
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.frist = 'native'
console.log(b.jobs.frist) // FE
```

但是该方法也是有局限性的：

- 会忽略 `undefined`
- 会忽略 `symbol`
- 不能序列化函数
- 不能解决循环引用的对象

```
let a = {
  age: undefined,
  sex: Symbol('male'),
  jobs: function() {},
  name: 'yck'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: "yck"} 该方法会忽略掉函数和 undefined 。
```

> 实现简易版深拷贝

```
function deepClone(obj) {
  function isObject(o) {
    return (typeof o === 'object' || typeof o === 'function') && o !== null
  }

  if (!isObject(obj)) {
    throw new Error('非对象')
  }

  let isArray = Array.isArray(obj)
  let newObj = isArray ? [...obj] : { ...obj }
  Reflect.ownKeys(newObj).forEach(key => {
    newObj[key] = isObject(obj[key]) ? deepClone(obj[key]) : obj[key]
  })

  return newObj
}
```

