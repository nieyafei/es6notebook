#读记之常用变量

>ES5只有`var`和`function`这两种声明变量的方法
ES6添加了`let`、`const`、`import`、`class`这四种方法，所以ES6有6种声明变量的方法

# let和const使用
- ### let：用于声明变量
> 虽然和var用法类似,但是有区别
1、声明的变量只在其代码块有效，块级作用域
2、不存在变量的提升（一定要先声明再使用）
3、暂时性死区：如果let在块级作用域使用，那么声明的变量绑定此区域，不受外部影响，形成了封闭作用域
4、不允许重复声明

`let`和`var`使用例子
```
var a = "aaa";
function test(){
  let b = "bbbb";
}
console.log(a,b);
//报错：ReferenceError: b is not defined
```
```
console.log(a,b); // 报错：ReferenceError: b is not defined
var a = "a";
let b = "bbb";
```
下面用经典的面试例子来做一个例子
```
# 正常的for循环打印数据
for(var i=0;i<5;i++){
	setTimeout(function(){
		console.log(i)
	},100)
}
# 结果:5 5 5 5 5
```
```
# 使用let来循环一下看看
for(let i=0;i<5;i++){
	setTimeout(function(){
		console.log(i)
	},100)
}
# 结果:0 1 2 3 4
```
```
# 当然使用js闭包也可以
for(var i=0;i<5;i++){
	(function(m){
		setTimeout(function(){
			console.log(m)
		},100)
	})(i)
}
# 结果:0 1 2 3 4
```
关于块级作用域
```
# 声明变量
function ff(){
    var tem = new Date();
    if(true){
        var tem = "aaa";
    }
    console.log(tem)
}
ff()// 结果是aaa
```
- ### const：用于声明常量，一旦声明，其值不可改变
> `const`的使用和`let`有些类似
1、声明常量，不可更改
2、作用域：只在声明所在的作用域有效
3、和let一样声明变量不提升，存在短暂性死区，只能在声明之后使用
4、不可重复声明变量

```
# 声明一个变量，然后修改这变量会有什么结果
const AD = "25639895";
AD = "aaaa";
// TypeError: Assignment to constant variable.
```
```
# 如果提前使用变量或者不在作用域使用，结果会报未定义
console.log(AD)
const tem = "25639895";
或
function ff(){
    const tem = 'aaa';
    console.log("第一次："+tem)
}
console.log("第二次："+tem)
ff()
// 结果：ReferenceError: tem is not defined
```
`const`声明一个复合类型对象，变量名不指向数据，而是指向数据所在的地址，这样只能保证地址不变，不能保证数据不变；
```
const arr = [];
arr[0] = "aaa";
console.log(arr)// ['aaa']
如果我修改一下
arr = ["aaa"];//相当于重新赋值
// 结果就会报 TypeError: Assignment to constant variable
```
如果我现在有一个对象{a:1,b:2,c:3},我不想改变这里面的值，应该怎么办？那么我们就需要将这个对象冻结`Object.freeze()`
```
# 不冻结的情况
const arr = {a:1,b:2,c:3}
arr.a = "aaa"
console.log(arr.a)// 结果是aaa

#冻结的情况
const arr = Object.freeze({a:1,b:2,c:3})
arr.a = "aaa"
console.log(arr.a)// 结果是1
```