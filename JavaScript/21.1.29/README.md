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

### console 控制台

我们可以用 console 对象中的方法向控制台输出信息

-   log：输出消息
-   info：同上
-   warn：输出警告
-   error：输出错误

在浏览器中，也可使用 alert()弹出窗口信息

document.write()方法可以向 html 页面输出信息

**注**：alert()会**停止**当前页面代码的运行，在点击确定按钮后继续执行

```javascript
console.log('Hello World!');
```

## 基本语法

下面我们将 JavaScript 简称为 JS

-   JS 对**大小写敏感**
-   JS 语句末需加**分号** ;
-   JS **忽略**多个空格

## 变量

本章会介绍一些**基础**的变量类型

### 类型

#### 值类型

-   String: 字符串
-   Number: 数字
-   Boolean: 布尔值
-   Null: 空值
-   Undefined: 未定义

#### 引用类型

-   Object：对象

**注**：数组(Array)，函数(Function)是特殊的对象(Object)

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
-   变量名不能和 JS 内置关键字或保留字相同

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

在**全局作用域**中，var 关键字声明的变量属于 window 对象，可以用 window.变量名访问；但 let 关键字声明的变量**不属于** window 对象，不可以用 window.变量名访问

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

let 关键字**可以先声明再赋值**，而 const 关键字**必须**在一开始就被**初始化**（声明并赋值）。

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

使用**单/双引号**括起来，使用 typeof 检查 String 类型会返回**string**

这里列举一些常用的特殊字符：

-   使用斜杠 \\ 转义
-   使用 \\n 换行
-   使用 \\t 表示制表符(tab)

#### 模板字符串

还有另一种字符串叫模板字符串，它使用**反引号** `` 括起来

在模板字符串中，变量可以通过 **${变量}** 这样的表达式引用，同样的里面也可以是 JS 语句，这有些类似于 Shell 在字符串中引用变量的方式

```javascript
const a = 1;
const b = 2;
// 普通字符串
console.log('A plus B is: ' + (a + b));
// 模板字符串
console.log(`A plus B is: ${a + b}`);
```

这在字符串中使用变量更加方便了

与普通字符串不同的是，使用模板字符串来写多行字符串，**不需要**添加加号 +

```javascript
// 普通字符串(还得自己加个换行符)
const str = '123\n' + '456';
// 模板字符串
const str2 = `123
456`;
// 输出一致：'123'
//			'456'
console.log(str, str2);
```

你还可以使用 String.raw()方法获取 String 类型的值的**原始字符串**

你通常并**不需要**给这个方法加括号，直接在模板字符串前加入这个方法即可

```javascript
const name = 'John';
const a = String.raw`Hi\n${name}`;
// 输出：'Hi\nJohn'
console.log(a);
```

**注**：这里的 \n **并不是换行符**

### Number

Number 包括整数和浮点数（小数）

使用 Number.**MAX_VALUE** 可以查看 Number 类型的**最大值**，相反使用 Number.**MIN_VALUE** 可以查看**最小值**

```javascript
// 输出：1.7976931348623157e+308
console.log(Number.MAX_VALUE);
// 输出：5e-324
console.log(Number.MIN_VALUE);
```

#### Infinity

如果数值超过最大值，则为无穷（Infinity），无穷有两种：

-   Infinity：正无穷
-   -Infinity：负无穷

使用 typeof 检查 Infinity 类型会返回**number**

#### NaN

NaN 是一个特殊的数字，表示 Not a Number

使用 typeof 检查 NaN 类型也会返回**number**

**注：**使用 JS 进行浮点运算时，会出现**精度丢失**的情况，所以**切忌**在对**精度要求高**的业务场景中使用

### Boolean

布尔值只有两种类型：

-   true：真
-   false：假

布尔值常用于**逻辑判断**，使用 typeof 检查类型时会返回**boolean**

### Null

Null（空值）类型的值只有一个，即**null**

null 值用于表示一个**空的对象**

使用 typeof 检查 Null 类型时会返回**object**

### Undefined

Undefined（未定义）类型的值也只有一个，即**undefined**

当你声明一个变量而不赋值时，此时变量的值就是**undefined**

使用 typeof 检查 Undefined 类型时会返回**undefined**

## 强制类型转换

### 转换为 String

#### toString() 方法

-   **用法：**任何值.toString()

toString()方法**不会**影响到原变量的值，而时将转换结果作为**返回值**

**注：**Null 类型和 Undefined 类型**没有**toString()方法，所以在调用这个方法时会**报错**

#### String() 函数

-   **用法：**String(参数)

参数可以是任何值，下面列举了 String() 函数对一些类型的处理方式：

-   Number & Boolean：**直接调用**toString() 方法
-   Null & Undefined：转换为**字符串**形式的"null"或"undefined"

### 转换为 Number

#### Number() 函数

-   用法：Number(参数)

参数可以是任何值，下面列举了 Number() 函数对一些类型的处理方式：

-   String
    1. 如果是**纯数字**的字符串，直接将其转换为**数字**
    2. 如果字符串中**含有非数字**的内容，则转换为**NaN**
    3. 如果字符串是一个**空字符串**或**全是空格的字符串**则转换为**0**
-   Boolean
    -   true 转为**1**
    -   false 转为**0**

#### parseInt() & parseFloat() 函数

-   用法：parseInt(参数) / parseFloat(参数)

参数要求是**字符串**，如果传入的参数不是字符串，会先**转换成字符串再操作**

下面是两个函数的**区别**：

-   parseInt() 方法用于把一个字符串转换为**整数**
-   parseFloat() 方法用于把一个字符串转换为**浮点数**

**注：**在使用 parseInt() 函数时，建议传入第二个参数来指定以什么**进制**解析内容，原因可见 Toc "进制"

### 转换为 Boolean

#### Boolean() 函数

-   用法：Boolean(参数)

参数可以是任何值，下面列举了 Boolean() 函数对一些类型的处理方式：

-   Number：除去 0 和 NaN，其余都返回 true
-   String：除去空字符串，其余都返回 true
-   Null & Undefined：返回 false

只要值是[Truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 的，那返回值就是 true

## 进制

JS 中，如果要表示 16 进制的数字，需以 0x 开头

```javascript
const num = 0x10;
// 输出：16
console.log(num);
```

如果要表示 8 进制的数字，需以 0 开头

```javascript
const num = 010;
// 输出：8
console.log(num);
```

如果要表示 2 进制的数字，需以 0b 开头

**注：**

1. **不是**所有浏览器都能正确地识别**8 进制**的数字，有可能会当作**10 进制**处理
2. **不是**所有浏览器都支持**2 进制**数字（的表达式）

## 运算符
