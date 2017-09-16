# 《ES6标准入门》读记之变量的解构赋值

## 数组的解构赋值
> ES6允许按照一定的语法，从数组或者对象中取值，然后对变量进行赋值

- ### 模式匹配赋值
> 等号两边的模式相同，左边的变量就会被赋予对应的值

解构赋值适用于`var`   `let`    `const` 命令

```
# 正常的赋值
var a = 1,b=2,c=3;
# ES6的赋值
let [a,b,c] = [1,2,3];
console.log(a,b,c);// 打印结果1 2 3
这种就是模式匹配
```
```
# 嵌套的数组的赋值
let [a,b,[c]] = [1,2,[3]];
console.log(a,b,c);// 打印结果 1 2 3

# 留空的数组赋值
let [a,,[c]] = [1,2,[3]];
console.log(a,c);// 打印结果 1 3

# 不定参数(可变参数)赋值
let [a,...c] = [1,2,3,"aaa"];
console.log(a,c);// 1 [ 2, 3, 'aaa' ]
```
如果解构不成功，变量的值就是`undefined`,这种被称为`不完全解构`
不成功的情况有很多种，比如右侧的数组或对象和左侧的变量未成功匹配
```
let [a,...c] = [1];
console.log(a,c);// 结果a=1 c=undefined
```
```
let [a,...c] = [1];
console.log(a,c);// 结果a=1 c=[]
```
- 解构赋值默认值
```
let [a,b="bbb"] = ["aaa"]
console.log(a,b);// 结果 aaa bbb

# ES6内部使用严格相等运算符`===`判断一个位置是否有值，如果不严格等于undefined,默认值是不会生效的
let [a,b="bbb"] = ["aaa",null]
console.log(a,b);// 结果 aaa null
```

如果是一个set的结构呢？那么我们也可以用数组的解构赋值
```
let [a,b,c] = new Set(["aaa","bbb","vvv"]);
console.log(a,b,c);// 结果：aaa bbb vvv

let [a,c] = new Set(["aaa","bbb","vvv"]);
console.log(a,c);// 结果：aaa bbb

let [a,,c] = new Set(["aaa","bbb","vvv"]);
console.log(a,c);// 结果：aaa vvv
```
只要某种数据结果具有[`Iterator(迭代器)`](https://baike.baidu.com/item/iterator/226189?fr=aladdin)接口,或者原生具有这个接口，都可以进行数组的解构赋值

那么什么是`Iterator(迭代器)`?
> 一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）
在ES6中原生具备这个接口的：`数组`  `类似数组的对象`  `Set`  `Map`

# 对象的解构赋值
> 对象的解构和数组的解构是有区别的，对象的解构赋值是没有次序的，而数组是有次序的
对象的变量必须与属性同名，才能取得正确的值

- 变量名和属性名一致
```
let {a,c} = {a:"aaa",b:"bbb",c:"ccc"}
console.log(a,c);// 结果就是 a="aaa",c="ccc"
```
- 那么如果变量名和属性名不一致怎么赋值？
```
let {a,c} = {a1:"aaa",b1:"bbb",c1:"ccc"}
console.log(a,c);// 结果 undefined undefined
# 无法取到值，so都是undefined
```
```
# 正确写法
let {a1:a,c1:c} = {a1:"aaa",b1:"bbb",c1:"ccc"}
console.log(a,c);//结果 aaa ccc
```
> 通过上述的方法，其实是对象的解构赋值的内部机制是先找到同名属性，然后再赋值给对应的变量，真正的赋值是后者还不是前者
var {foo,baz}={foo:"111",baz:"222"}
等同于
var {foo:foo,baz:baz}={foo:"111",baz:"222"}

- 嵌套对象的解构
```
var obj = {
    p:[
        "nihao",
        {y:"beijing"}
    ]
}
var {p:[x,{y}]} = obj;
console.log(x,y)//结果 nihao beijing
# p 是模式
```
```
var obj = {
    p:{
        start:{
            line:5
        }
    }
}
var {p:{start:{line}}} = obj;
console.log(line)
# p和start都是模式
```
# 字符串的解构赋值
> 字符串可以进行解构赋值，字符串会被转换成一个类似数组的对象

```
# 简单的例子
let [a,b,c] = 'hello';
console.log(a,b,c);// h  e  l
```
```
# 数组的对象都有length属性
let {length} = 'hello';
console.log(length);//5
```
# 数值和布尔值的解构赋值
> 在解构的时候，如果等号右边的是数值或布尔值，则会先转为对象

```
# undefined
let {length} = 123;
console.log(length);

数值和布尔值的包装对象都有toString属性
let {toString:s} = 123;
console.log(s === Number.prototype.toString);//true
```
# 函数参数的解构赋值
```
function add([a,b]){
    return a+b;
}
add([1,2]);//3
```
```
[[1,2],[5,6]].map(([a,b])=> a+b)//结果是 [3,11]
```
# 解构赋值的用处
- 交换变量的值
```
let a = 3;
let b = 5;
[a,b] = [b,a];
console.log(a,b);
# 当然交换变量的值还有很多种方法
```
- 从函数返回多个值
```
function ex(){
    return [1,2]
}
let [a,b] = ex();
console.log(a,b);//1  2
```
- 函数参数的定义
- 提取JSON数据
- 函数参数的默认值
- 遍历Map结构
因为Map结构原生支持`Iterator`,因此可以使用for ... of ... 对map进行遍历，这样比较方便
```
var map = new Map();
map.set("1","aaa");
map.set("2","bbb");
for(let [key,value] of map){
    console.log(key+":"+value);
}
// 结果 1:aaa    2:bbb
```
- 输入模块的指定方法