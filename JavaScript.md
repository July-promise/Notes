#### ES6

##### var、let、const区别

- 变量提升
- 暂时性死区
- 块级作用域
- 重复声明
- 修改声明的变量

在ES5中，顶层对象的属性和全局变量是等价的，用`var`声明的变量既是全局变量，也是顶层变量

注意：顶层对象，在浏览器环境指的是`window`对象，在 `Node` 指的是`global`对象

`let`是`ES6`新增的命令，声明的变量，只在`let`命令所在的代码块内有效

##### 变量提升

`var`声明的变量存在变量提升，即变量可以在声明之前调用，值为`undefined`

`let`和`const`不存在变量提升，即它们所声明的变量一定要在声明后使用，否则报错

```js
// var
console.log(a)  // undefined
var a = 10

// let 
console.log(b)  // Cannot access 'b' before initialization
let b = 10

// const
console.log(c)  // Cannot access 'c' before initialization
const c = 10
```

##### 暂时性死区

`var`不存在暂时性死区

`let`和`const`存在暂时性死区，只有等到声明变量的那一行代码出现，才可以获取和使用该变量

```js
// var
console.log(a)  // undefined
var a = 10

// let
console.log(b)  // Cannot access 'b' before initialization
let b = 10

// const
console.log(c)  // Cannot access 'c' before initialization
const c = 10
```

##### 块级作用域

`var`不存在块级作用域

`let`和`const`存在块级作用域

```js
// var
{
    var a = 20
}
console.log(a)  // 20

// let
{
    let b = 20
}
console.log(b)  // Uncaught ReferenceError: b is not defined

// const
{
    const c = 20
}
console.log(c)  // Uncaught ReferenceError: c is not defined
```

##### 重复声明

`var`允许重复声明变量

`let`和`const`在同一作用域不允许重复声明变量

```js
// var
var a = 10
var a = 20 // 20

// let
let b = 10
let b = 20 // Identifier 'b' has already been declared

// const
const c = 10
const c = 20 // Identifier 'c' has already been declared
```

##### 修改声明的变量

`var`和`let`可以

`const`声明一个只读的常量。一旦声明，常量的值就不能改变

```
/ var
var a = 10
a = 20
console.log(a)  // 20

//let
let b = 10
b = 20
console.log(b)  // 20

// const
const c = 10
c = 20
console.log(c) // Uncaught TypeError: Assignment to constant variable
```

#####  entries()，keys()，values()

`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历

```js
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
```

#### 字符串方法

##### charAt()

概述：charAt() 方法从一个字符串中返回指定的字符

```
语法：str.charAt(index)
```

```javascript
//示例：
var anyString = "Brave new world";
console.log("The character at index 0   is '" + anyString.charAt(0)   + "'");
console.log("The character at index 1   is '" + anyString.charAt(1)   + "'");
console.log("The character at index 2   is '" + anyString.charAt(2)   + "'");
console.log("The character at index 3   is '" + anyString.charAt(3)   + "'");
console.log("The character at index 4   is '" + anyString.charAt(4)   + "'");
console.log("The character at index 999 is '" + anyString.charAt(999) + "'");

//结果
The character at index 0 is 'B'
The character at index 1 is 'r'
The character at index 2 is 'a'
The character at index 3 is 'v'
The character at index 4 is 'e'
The character at index 999 is ''
```

##### charCodeAt(index)

概述：返回字符串指定位置的字符编码

```
语法：str.charCodeAt(index)
参数:一个大于等于 0，小于字符串长度的整数。如果不是一个数值，则默认为 0。
返回值:指定 index 处字符的 UTF-16 代码单元值的一个数字；如果 index 超出范围，charCodeAt() 返回 NaN。
```

```javascript
//示例
"ABC".charCodeAt(0) // returns 65:"A"

"ABC".charCodeAt(1) // returns 66:"B"

"ABC".charCodeAt(2) // returns 67:"C"

"ABC".charCodeAt(3) // returns NaN
```

##### concat()

```
语法；str.concat(str2, [, ...strN])
```

```JavaScript
let hello = 'Hello, '
console.log(hello.concat('Kevin', '. Have a nice day.'))
// Hello, Kevin. Have a nice day.

let greetList = ['Hello', ' ', 'Venkat', '!']
"".concat(...greetList)  // "Hello Venkat!"

"".concat({})    // [object Object]
"".concat([])    // ""
"".concat(null)  // "null"
"".concat(true)  // "true"
"".concat(4, 5)  // "45"
```

##### indexOf()

概述：indexOf() 方法返回调用它的 String 对象中第一次出现的指定值的索引，从 fromIndex 处进行搜索。如果未找到该值，则返回 -1

```
语法：str.indexOf(searchValue [, fromIndex])
```

```javascript
'hello world'.indexOf('') // 返回 0
'hello world'.indexOf('', 0) // 返回 0
'hello world'.indexOf('', 3) // 返回 3
'hello world'.indexOf('', 8) // 返回 8

//fromIndex 值大于等于字符串的长度，将会直接返回字符串的长度（str.length）：
'hello world'.indexOf('', 11) // 返回 11
'hello world'.indexOf('', 13) // 返回 11
'hello world'.indexOf('', 22) // 返回 11
```

##### lastIndexOf()

概述：lastIndexOf() 方法返回调用String 对象的指定值最后一次出现的索引，在一个字符串中的指定位置 fromIndex处从后向前搜索。如果没找到这个特定值则返回-1 。

```
语法：str.lastIndexOf(searchValue[, fromIndex])
```

**注意：lastIndexOf 方法区分大小写**

```javascript
var anyString = "Brave new world";

console.log("The index of the first w from the beginning is " + anyString.indexOf("w"));
// Displays 8
console.log("The index of the first w from the end is " + anyString.lastIndexOf("w"));
// Displays 10

console.log("The index of 'new' from the beginning is " + anyString.indexOf("new"));
// Displays 6
console.log("The index of 'new' from the end is " + anyString.lastIndexOf("new"));
// Displays 6
```

##### split()

概述：split() 方法使用指定的分隔符字符串将一个String对象分割成**子字符串数组**，以一个指定的分割字串来决定每个拆分的位置。

```
语法：str.split([separator[, limit]])
```

```JavaScript
var myString = "Hello World. How are you doing?";
var splits = myString.split(" ", 3);
console.log(splits); // ["Hello", "World.", "How"]

"hello world".split('l') // ["he","","o wor","d"]

"hello world".split('') // ["h","e","l","l","o"," ","w","o","r","l","d"]
```

##### slice()

概述：slice() 方法提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串。

```
语法：str.slice(beginIndex[, endIndex])  ——beginIndex：0开始
```

```javascript
var str1 = 'The morning is upon us.', // str1 的长度 length 是 23。
    str2 = str1.slice(1, 8),
    str3 = str1.slice(4, -2),
    str4 = str1.slice(12),
    str5 = str1.slice(30);
console.log(str2); // 输出：he morn
console.log(str3); // 输出：morning is upon u
console.log(str4); // 输出：is upon us.
console.log(str5); // 输出：""
//负值
var str = 'The morning is upon us.';
str.slice(-3);     // 返回 'us.'
str.slice(-3, -1); // 返回 'us'
str.slice(0, -1);  // 返回 'The morning is upon us'
```

##### substr()

概述：substr() 方法返回一个字符串中从指定位置开始到指定字符数的字符。

```
语法：str.substr(start[, length])
```

```javascript
//示例
var str = "abcdefghij";

console.log("(1,2): "    + str.substr(1,2));   // (1,2): bc
console.log("(-3,2): "   + str.substr(-3,2));  // (-3,2): hi
console.log("(-3): "     + str.substr(-3));    // (-3): hij
console.log("(1): "      + str.substr(1));     // (1): bcdefghij
console.log("(-20, 2): " + str.substr(-20,2)); // (-20, 2): ab
console.log("(20, 2): "  + str.substr(20,2));  // (20, 2):
```



##### toLowerCase()

概述：会将调用该方法的字符串值转为小写形式，并返回。

##### toUpperCase()

概述：会将调用该方法的字符串转为大写形式并返回

##### repeat(count)

概述：构造并返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本