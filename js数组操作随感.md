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

