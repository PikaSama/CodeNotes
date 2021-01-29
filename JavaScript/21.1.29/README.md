# JavaScript 笔记 -- 21.1.29

## 介绍

JavaScript 由 NetScape 公司开发，起初名为 LiveScript，后由 Sun 公司介入更名为 JavaScript

### ECMAScript 与 JavaScript

为了让各个浏览器有一个统一的脚本语言，NetScape 公司决定将 JavaScript 提交给国际标准化组织 ECMA，希望这种语言能够成为国际标准，于是 ECMAScript 诞生了

ECMAScript 与 JavaScript 的关系：

-   **ECMAScript 是 JavaScript 的标准**
-   **JavaScript 是 ECMAScript 的实现**

在日常生活中，二者可以互换使用（称呼）

### 引擎

| 浏览器         | 引擎                                                                    |
| -------------- | ----------------------------------------------------------------------- |
| IE / Edge (旧) | Jscript (3-8) / Chakra (9+)                                             |
| Chrome         | V8                                                                      |
| Safari         | JavaScriptCore / Nitro (4+)                                             |
| Firefox        | SpiderMonkey (1.5-3.0) / TraceMonkey (3.5-3.6) / JaegerMonkey (4+)      |
| Opera          | Linear A (4.0-6.1) / B (7.0-9.2) / Futhark (9.5-10.2) / Carakan (10.5+) |

## Hello World

-   console：控制台
    -   log：输出消息
    -   warn：输出警告
    -   error：输出错误

在浏览器中，也可使用 alert()弹出窗口信息

document.write()方法可以向 html 页面输出信息

注：alert()会**停止**当前页面代码的运行，在点击确定按钮后继续执行

```javascript
console.log('Hello World!');
```

## 基本语法

下面我们将 JavaScript 简称为 JS

-   JS 对大小写敏感
-   JS 语句末需加分号';'
-   JS 忽略多个空格

## 变量

### 类型

**值类型：**

-   String: 字符串
-   Number: 数字
-   Boolean: 布尔值
-   Null: 空值
-   Undefined: 未定义

**引用类型：**

-   Object：对象

注：数组(Array)，函数(Function)是特殊的对象(Object)

**值类型和引用类型的区别：**

-   值类型：变量之间的互相赋值，是将原变量的值**复制**给另一个变量，两者值的改动**互不影响**

-   引用类型：变量之间的互相赋值，是将原变量引用的值的**地址**赋值给了另一个变量，两者中任意一者修改了值，**会影响**到另一者的值

举个栗子：

值类型的赋值

```javascript
const a = '1';
const b = a;
// 输出：1 1
console.log(a, b);
b = 2;
// 输出：1 2
console.log(a, b);
```

引用类型的赋值

```javascript
const obj1 = {
    name: 'John',
    age: 18,
};
const obj2 = obj1;
// 输出：{name:'John'...} {name:'John'...}
console.log(obj1, obj2);
obj2.name = 'Tom';
// 输出：{name:'Tom'...} {name:'Tom'...}
console.log(obj1, obj2);
```

如果你用过 Linux，那么你可以联想到软链接(symbolic link)，在这里，obj1 相当于一个指向对象的软链接，当 obj1 赋值到 obj2 时，相当于创建一个同样指向这个对象的软链接

### 命名规范

-   变量名可以是字母、数字、下划线、美元符号的组合
-   变量名第一个字符不可以是数字
-   变量名遵循驼峰命名法，第一个单词开头字母小写，后面的单词开头字母大写
-   变量名不能和 JS 内置关键字相同
-   变量名最好不和 JS 预定的关键字相同（未来可能会出现的功能）

### 定义

在 ES6 中，有了 let 和 const 关键字，你可以完全抛弃 var 了

相比 var，let 和 const 拥有块级作用域，即变量只作用在当前的代码块中(大括号里面就是代码块），这样避免了 var 带来的全局变量污染的问题

看个例子：

使用 var 关键字

```javascript
for (var i = 0; i < 10; i += 1) {
    // do something...
}
// 无报错，正常输出
console.log(i);
```

使用 let 关键字

```javascript
for (let i = 0; i < 10; i += 1) {
    // do something...
}
// 报错(变量i未定义)
console.log(i);
```

我**不推荐**使用自增/自减运算符，这可能会导致无法预料的问题，请使用+= / -=代替

let / const 关键字在**不同作用域**下声明相同名称的变量是**合法**的（块级作用域的特性）

```javascript
let a = 'hi';
{
    // 无报错，合法
    let a = 'hello';
    // 如果这个代码块没有定义a，JS会往外面找这个变量，那么输出就会是'hi'
    // 输出：'hello'
    console.log(a);
}
// 输出：'hi'
console.log(a);
```

在**全局作用域**中，var 关键字声明的变量属于 window 对象，可以用 window.变量名访问；但 let 关键字声明的变量不属于 window 对象，不可以用 window.变量名访问

```javascript
var a = 'hi';
let b = 'hello';
// 输出：'hi'
console.log(window.a);
// undefined
console.log(window.b);
```

const 关键字用于声明一个**常量**（一旦初始化便**不能**再改变值）。

```javascript
const a = 'hi';
// 报错（尝试赋值给常量）
a = 'hello';
```

但这并不意味着它所持有的值是不可变的，只是**变量标识符不能重新分配**。例如，在引用内容是对象的情况下，这意味着**可以**改变对象的键值。

```javascript
const obj = {
    firstName: 'John',
    lastName: 'Doe',
};
// 无报错
obj.firstName = 'Lee';
obj.lastName = 'Kun';
/* 输出：{
 * firstName: 'Lee',
 * lastName: 'Kun',
 }
 */
console.log(obj);
```

数组也**同样适用**

```javascript
const arr = [1, 2, 3, 4];
// 无报错
arr.push(5);
// 输出：[1,2,3,4,5]
console.log(arr);
```

但我们**都不能对它们赋一个新的值**

```javascript
const obj = {
    name: 'John',
    age: 18,
};
// 报错（尝试赋值给常量）
obj = {
    name: 'Tom',
    age: 19,
};
const arr = [1, 2, 3, 4, 5];
// 报错（尝试赋值给常量）
arr = ['1', '2', '3', '4', '5'];
```

let 关键字可以先声明再赋值，而 const 关键字必须在一开始就被初始化（声明并赋值）。

```javascript
// 合法
let a;
a = 10;
// 不合法
const b;
// 合法
const c = 10;
```

### String

使用单/双引号括起来

-   使用斜杠 \\ 转义
-   使用 \\n 换行
-   使用 \\t 表示制表符(tab)

### Number