## map filter reduce

> map, filter, reduce 各自有什么用？

`map` 的作用是生成一个新数组，遍历原数组，将每个元素拿出来做一些变换后放入到新数组中。

```
[1, 2, 3].map(v => v + 1) // [2, 3, 4]
```

另外 `map` 的回调函数接受三个参数，分别是当前索引元素，索引，原数组

```
['1','2','3'].map(parseInt) // [1, NaN, NaN]
```



`filter` 的作用也是生成一个新数组，在遍历数组时，将返回值为 `true` 的元素放到新数组中，可以用来删除一些不需要的元素。

```
let arr = [1, 2, 4, 6]
let newArr = arr.filter(x => x !== 6)
console.log(newArr) // [1, 2, 4]
```

和 `map` 一样，`filter` 的回调函数也接受三个参数，用处也相同。



`reduce`  可以将数组中的元素通过回调函数最终转换为一个值。

```
const arr = [1, 2, 3]
const sun = arr.reduce((acc, current) => acc + current, 0)
console.log(sum) // 6
```

对于 `reduce` 来说， 它接受两个参数，分别是回调函数和初始值。

* 首先初始值为 `0`，该值会在执行第一次回调函数时作为第一个参数传入
* 回调函数接受四个参数，分别为累计值、当前元素、当前索引、原数组
* 第一次执行回调函数时，当前值和初始值相加得出结果 `1`，该结果会在第二次执行回调函数时当做第一个参数传入
* 所以在第二次执行回调函数时，相加的值就分别是 `1` 和 `2`，以此类推，循环结束后得到结果 `6`



> 使用 `reduce` 来实现  `map` 函数

```
const arr = [1, 2, 3]

const mapArr = arr.map(x => x * 2)

const reduceArr = arr.reduce((acc, current) => {
	acc.push(current * 2)
	return acc
}, [])

console.log(reduceArr) // [2, 4, 6]
```

