#《ES6标准入门》读记之数组的扩展

#Array.from()
> 将两类对象转为数组
1、类似数组的对象
2、可遍历的对象（iterable）,包括数据结构的Set和Map

```
var obj = {"0":"aaa","1":"bbb","2":"ccc",length:3}

# ES5的写法
console.log([].slice.call(obj))

# ES6写法
console.log(Array.from(obj));

// 结果  [ 'aaa', 'bbb', 'ccc' ]
```
实际应用中，常见的类似的数组对象是`DOM`操作返回的`NodeList`集合，以及函数内部的`arguments`对象。
```
var pArr = document.querySelectorAll("p");
Array.from(pArr).forEach(function(p){
	console.log(p)
})
```

#Array.of()
> 用于将一组值转换为数组
Array方法没有参数和有一个参数或三个参数时，返回的结果不一样

```
# Array()
console.log(Array());// []
console.log(Array(3));// [,,]
console.log(Array(3,4,5,6));//[3,4,5,6]

#Array.of()
console.log(Array.of());//[]
console.log(Array.of(3));//[3]
console.log(Array.of(3,4,5,6));
// 结果 [ 3, 4, 5, 6 ]
```
```
Array.of()等同于
function ArrayOf(){
	return [].slice.call(arguments)
}
console.log(ArrayOf(1,2,3))
```
# copyWithin()
> 在当前数组内部将指定位置的成员复制到其他位置（会覆盖原有成员）
这个方法会修改当前数组

> Array.prototype.copyWithin(target,start=0,end = this.length)
target（必需）:从该位置开始替换数据
start（可选）:从该位置开始读取数据，默认0；如果为负数，则倒数
end (可选)：到该位置停止读取数据，默认等于数据的长度。如果是负数，表示倒数

```
console.log([1,2,3,4,5].copyWithin(0,2));
// [ 3, 4, 5, 4, 5 ]

console.log([1,2,3,4,5].copyWithin(0,2,4));
// [ 3, 4, 3, 4, 5 ]
```
# find()和findIndex()
> `find`找出第一个符合条件的数组成员
`findIndex`返回第一个符合条件的数组成员的位置，如果所有成员都不符合，则返回-1

> `find`方法可以接受三个参数
第一个参数是当前的值
第二个参数是当前的位置
第三个参数是原数组

```
console.log([1,2,3,4,5,6,7].find((n)=> n>5));
// 结果 6

[1,2,3,4,5,6,7].find(function(value,index,arr){
	return value>6
})

[1,2,3,4,5,6,7].findIndex(function(value,index,arr){
	return value>6
});// 7

console.log([1,2,3,4,5,6,7].findIndex(function(value,index,arr){
	return value>6
}));// 6
```
#fill()
> 给定值填充数据
用于空数组的初始化
还可以接受第二个和第三个参数，用于指定的起始位置和结束位置

```
console.log(new Array(10).fill(true));
// [ true, true, true, true, true, true, true, true, true, true ]
```
# entries()、keys()、values()
> `entries`对键值对的遍历
`keys`       对键名的遍历
`values`      对键值得遍历

```
# keys
for(let index of [1,2,3,4].keys()){
	console.log(index)
}
```
```
# values
for(let value of [1,2,3,4].values()){
	console.log(index)
}
```
```
# entries
for(let [index,value] of [1,2,3,4].entries()){
	console.log(index)
}
```
# includes()
> 表示某个数组是否包含给定的值

```
console.log([1,2,3,4,5].includes(2));// true
```