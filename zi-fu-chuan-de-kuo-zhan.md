#《ES6标准入门》读记之字符串的扩展

# 字符的`Unicode`表示
> js允许采用`\uxxxx`形式表示一个字符，`xxxx`表示字符的码点
`Unicode`源于一个很简单的想法：将全世界所有的字符包含在一个集合里，计算机只要支持这一个字符集，就能显示所有的字符，再也不会有乱码了。
ES6增强了对`Unicode`的支持

关于`Unicode`，详情点击:[Unicode](http://www.ruanyifeng.com/blog/2014/12/unicode.html)
`Unicode` 的表示局限于\u0000-\uFFFF之间的字符
```
console.log("\u0061");// a
"\u8717\u725b"则代表的是蜗牛
如果超过范围的字符，必须用2个双字节的形式表达
```
ES6对此做出了改进，只要将码点放入大括号，就能正确读取该字符
```
console.log("\u{41}");//  A

console.log("\u1F680");// Ὠ0
console.log("\u{1F680}" === "\uD83D\uDE80");//true
```
# codePointAt()
> 能正确处理4个字节存储的字符，返回一个字符的码点
返回的码点是十进制值，如果想返回十六进制的值，可以使用toString转换

```
var s = "吉利";
# 十进制
console.log(s.codePointAt());//21513

#十六进制
console.log(s.codePointAt().toString(16));// 5409

# 遍历
var s = "吉利";
for(let cn of s){
    console.log(cn.codePointAt(0).toString(16))
}
// 5409   5229
```
判断一个字符由2个字节还是4个字节组成
```
var s = "吉";
console.log(s.codePointAt(0) > 0XFFFF);//false
```
# String.fromCodePoint()
> 此方法用于从码点返回对于的字符
不能识别32位的UTF-16字符（Unicode编号大于0xFFFF）

```
console.log(String.fromCodePoint(0x20BB7));// 𠮷
```
# 字符串的遍历接口（for of）
> 可以识别大于`0XFFFF`的码点，传统的`for`循环无法识别

```
for(let a of "蜗牛"){
    console.log(a)
}
```
```
for(let a of String.fromCodePoint(0x20BB7)){
    console.log(a)
}
```
# at()和charAt()
> ES5的`charAt()`返回字符串给定位置的字符
`charAt()`该方法不能识别码点大于0xFFFF的字符

```
console.log("abc".charAt(0));// a
console.log("𠮷".charAt(0));
```
ES7提供的at()，存在兼容问题
```
console.log("abc".at(0))
console.log("𠮷".at(0))
```
# normalize()
> 用来将字符的不同表示方法统一为同样的形式，也就是Unicode正规化

# includes()
> 返回boolean值，表示是否找到了参数字符串
支持第二个参数，表示搜索的位置

```
console.log("abcd".includes("a"));// true
console.log("abcd".includes("a",1));// false
```

# startsWith()
> 返回boolean值，表示参数字符串是否在源字符串的头部
支持第二个参数，表示搜索的位置

```
console.log("abcd".startsWith("a"));// true
console.log("abcad".startsWith("a",3));// true
```
# endsWith()
> 返回boolean值,表示参数字符串是否在源字符串的尾部
支持第二个参数，表示搜索的位置，前n个字符

```
console.log("abcd".endsWith("a"));// false
console.log("abcdc".endsWith("c",3));// true
```
# repeat()
> 返回一个新的字符串，表示将源字符串重复n次

```
console.log("12".repeat(10));
// 12121212121212121212
```
如果生成一个10个相同值(12)的数组
```
var str = "12,".repeat(10).split(",");
str.pop();
console.log(str);
// [ '12', '12', '12', '12', '12', '12', '12', '12', '12', '12' ]
```
# padStart()和padEnd()
> 如果某个字符串未达指定长度，会在头部或尾部补全
padStart用于头部补全，padEnd用于尾部补全

```
console.log("a".padStart(6,"nihao"));// nihaoa
console.log("a".padStart(5,"nihao"));// nihaa
```
```
console.log("a".padEnd(5,"nihao"));// aniha
console.log("a".padEnd(10,"nihao"));// anihaoniha
```
# 模板字符串
js正常的字符串
```
var b = "bbb";
var a = "aaa"+"<div>"+b+"</div>";
```
这样的写法比较繁琐，特别是多行的字符串,es6提供了增强版，用反引号(`)标识，可以作为普通字符串使用，也可以定义多行字符串，或者字符串中嵌入变量
```
var name = "蜗牛";
var teh = `demo
nihao
hello ${name}
`;
console.log(teh);
```
# String.raw()
> 此方法用来充当模板字符串的处理函数，返回一个被转义的字符串，对应于替换变量后的模板字符串