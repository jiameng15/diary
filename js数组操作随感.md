关于数组的复制

```js
let zoo = ['cat','dog','pig']
let copies = zoo.slice(0) // ['cat','dog','pig']
zoo.push('bird') 
console.log(zoo) // ['cat','dog','pig','bird']
console.log(copies) // ['cat','dog','pig']

// 复制数组copies不会因为zoo的改变而改变
// Array.slice 方法会返回一个全新的数组
```

关于数组的操作

```js
//Array.splice 方法会改变原始数组
// 使用splice的前提是知道对应下标
// 当我知道某个条件时,何获取数组下标后,才能使用splice删除该项
// indexOf 配合 find
let zoo = [{name:'cat',age:1},{name:'dog',age:2},{name:'pig',age:3}]
let findItem = zoo.find(item=>item.name=='dog') // {name: "dog", age: 2}
let findIndex = zoo.indexOf(findItem) // 1 此时我已获取需要的下标
// 合并写法
// 想从zoo中删除name为dog的动物
zoo.splice(zoo.indexOf(zoo.find(item=>item.name=='dog')),1) // 返回新数组,数组中为删除项[{name: "dog", age: 2}]

console.log(zoo) // 此时zoo变为[{name:'cat',age:1},{name:'pig',age:3}]
```



关于数组的清空

```js
let zoo = ['cat','dog','pig']
zoo.length = 0
console.log(zoo) // []

// 为什么不是用zoo = []?
let park = ['Benz','BMW','Audi']
let newZoo = zoo

```



```js
let zoo = ['cat','dog','pig']
let park = ['Benz','BMW','Audi']
let newZoo = zoo
let newPark = park
zoo.length = 0
park = []
console.log(newZoo) // []
console.log(newPark) // ['Benz','BMW','Audi']

//使用 Array.lengt = 0 清空数组,则其他变量引用没有改变,所以其他变量也清空了
//使用 Array = [] 等价于 Array = new Array(),创建了一个全新的数组,用此方法清空数组,则改变了当前数组的引用,其他变量的引用地址没有改变,所以其他变量没有清空
```

