# JavaScript 笔记 -- 21.1.29

## 前言

本章会有一些 ES6 的语法和个人在代码写法上的建议，部分过于基础、个人认为不是重点的知识不会详述或直接忽略（懒），仅供有一定基础的人巩固知识用。

## 介绍

JavaScript 由 NetScape 公司开发，起初名为 LiveScript，后由 Sun 公司介入更名为 JavaScript

### ECMAScript 与 JavaScript

为了让各个浏览器有一个统一的脚本语言，NetScape 公司决定将 JavaScript 提交给国际标准化组织 ECMA，希望这种语言能够成为国际标准，于是 ECMAScript 诞生了

ECMAScript 与 JavaScript 的关系：

-   ECMAScript 是 JavaScript 的标准
-   JavaScript 是 ECMAScript 的实现

在日常生活中，二者可以互换使用（称呼）

### 引擎

各家浏览器都有自己用于执行 JavaScript 的引擎

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

注：alert()会停止当前页面代码的运行，在点击确定按钮后继续执行

```javascript
console.log('Hello World!');
```

## 基本语法

下面我们将 JavaScript 简称为 JS

-   JS 对大小写敏感
-   JS 语句末需加分号 ;
-   JS 忽略多个空格

## 变量

本章会介绍一些基础的变量类型

### 类型

#### 值类型

-   String: 字符串
-   Number: 数字
-   Boolean: 布尔值
-   Null: 空值
-   Undefined: 未定义

#### 引用类型

-   Object：对象

注：数组(Array)，函数(Function)是特殊的对象(Object)

值类型和引用类型的区别：

-   值类型：变量之间的互相赋值，是将原变量的值复制给另一个变量，两者值的改动互不影响

-   引用类型：变量之间的互相赋值，是将原变量引用的值的地址赋值给了另一个变量，两者中任意一者修改了值，会影响到另一者的值

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

我不推荐使用自增/自减运算符，这可能会导致无法预料的问题，请使用+= / -=代替 (ESLint: [no-plusplus](http://eslint.cn/docs/rules/no-plusplus))

let / const 关键字在不同作用域下声明相同名称的变量是合法的（块级作用域的特性）

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

在全局作用域中，var 关键字声明的变量属于 window 对象，可以用 window.变量名访问；但 let 关键字声明的变量不属于 window 对象，不可以用 window.变量名访问

```javascript
var a = 'hi';
let b = 'hello';
// 输出：'hi'
console.log(window.a);
// undefined
console.log(window.b);
```

const 关键字用于声明一个常量（一旦初始化便不能再改变值）。

```javascript
const a = 'hi';
// 报错（尝试赋值给常量）
a = 'hello';
```

但这并不意味着它所持有的值是不可变的，只是变量标识符不能重新分配。例如，在引用内容是对象的情况下，这意味着可以改变对象的键值。

```javascript
const obj = {
    firstName: 'John',
    lastName: 'Doe',
};

// 无报错
obj.firstName = 'Lee';
obj.lastName = 'Kun';
/* 
 * 输出：{
 * firstName: 'Lee',
 * lastName: 'Kun',
 }
 */
console.log(obj);
```

数组也同样适用

```javascript
const arr = [1, 2, 3, 4];
// 无报错
arr.push(5);
// 输出：[1,2,3,4,5]
console.log(arr);
```

但我们都不能对它们赋一个新的值

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

使用单/双引号括起来，使用 typeof 检查 String 类型会返回 string

这里列举一些常用的特殊字符：

-   使用斜杠 \\ 转义
-   使用 \\n 换行
-   使用 \\t 表示制表符(tab)

#### 模板字符串

还有另一种字符串叫模板字符串，它使用反引号 `` 括起来

在模板字符串中，变量可以通过 ${变量} 这样的表达式引用，同样的里面也可以是 JS 语句，这有些类似于 Shell 在字符串中引用变量的方式

```javascript
const a = 1;
const b = 2;
// 普通字符串
console.log('A plus B is: ' + (a + b));
// 模板字符串
console.log(`A plus B is: ${a + b}`);
```

这在字符串中使用变量更加方便了

与普通字符串不同的是，使用模板字符串来写多行字符串，不需要添加加号 +

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

你还可以使用 String.raw()方法获取 String 类型的值的原始字符串

你通常并不需要给这个方法加括号，直接在模板字符串前加入这个方法即可

```javascript
const name = 'John';
const a = String.raw`Hi\n${name}`;
// 输出：'Hi\nJohn'
console.log(a);
```

注：这里的 \n 并不是换行符

### Number

Number 包括整数和浮点数（小数）

使用 Number.MAX_VALUE 可以查看 Number 类型的最大值，相反使用 Number.MIN_VALUE 可以查看最小值

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

使用 typeof 检查 Infinity 类型会返回 number

#### NaN

NaN 是一个特殊的数字，表示 Not a Number

使用 typeof 检查 NaN 类型也会返回 number

注：使用 JS 进行浮点运算时，会出现精度丢失的情况，所以切忌在对精度要求高的业务场景中使用

### Boolean

布尔值只有两种类型：

-   true：真
-   false：假

布尔值常用于逻辑判断，使用 typeof 检查类型时会返回 boolean

### Null

Null（空值）类型的值只有一个，即 null

null 值用于表示一个空的对象

使用 typeof 检查 Null 类型时会返回 object

### Undefined

Undefined（未定义）类型的值也只有一个，即 undefined

当你声明一个变量而不赋值时，此时变量的值就是 undefined

使用 typeof 检查 Undefined 类型时会返回 undefined

## 强制类型转换

注：我建议在语句开始时执行类型转换

### 转换为 String

#### toString() 方法

-   用法：任何值.toString()

toString()方法不会影响到原变量的值，而时将转换结果作为返回值

注：Null 类型和 Undefined 类型没有 toString()方法，所以在调用这个方法时会报错

#### String() 函数

-   用法：String(参数)

参数可以是任何值，下面列举了 String() 函数对一些类型的处理方式：

-   Number & Boolean：直接调用 toString() 方法
-   Null & Undefined：转换为字符串形式的"null"或"undefined"

### 转换为 Number

#### Number() 函数

-   用法：Number(参数)

参数可以是任何值，下面列举了 Number() 函数对一些类型的处理方式：

-   String
    1. 如果是纯数字的字符串，直接将其转换为数字
    2. 如果字符串中含有非数字的内容，则转换为 NaN
    3. 如果字符串是一个空字符串或全是空格的字符串则转换为 0
-   Boolean
    -   true 转为 1
    -   false 转为 0

#### parseInt() & parseFloat() 函数

-   用法：parseInt(参数) / parseFloat(参数)

参数要求是字符串，如果传入的参数不是字符串，会先转换成字符串再操作

下面是两个函数的区别：

-   parseInt() 方法用于把一个字符串转换为整数
-   parseFloat() 方法用于把一个字符串转换为浮点数

注：在使用 parseInt() 函数时，建议传入第二个参数来指定以什么进制解析内容，并将其转换为 10 进制，原因可见 Toc "进制"

```javascript
// 输出：6
// 解析：把110当作为2进制的数字，转换为10进制
console.log(parseInt(110, 2));
```

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

注：

1. 不是所有浏览器都能正确地识别 8 进制的数字，有可能会当作 10 进制处理
2. 不是所有浏览器都支持 2 进制数字（的表达式）

## 运算符

### 算术运算符

当对非 Number 类型的值进行运算时，会先将其转换为 Number 类型再运算

注：

1. 任何值和 NaN 进行运算都得 NaN
2. 任何值与字符串做加法运算，会将其转换为 String 类型，并执行字符串拼接操作

```javascript
// 输出：NaN
console.log(NaN * 2);
// 输出：hi1
console.log('hi' + 1);
```

### 一元运算符

#### 自增

使变量自身加 1

自增分两种：

-   a++(后++)
-   ++a(前++)

两种方式的区别：

-   后++：先返回变量的值，自身再进行自增
-   前++：先自身进行自增，再返回变量的值

```javascript
const a = 1;
const b = a++;
// 输出：2 1
console.log(a, b);

const c = 1;
const d = ++c;
// 输出：2 2
console.log(c, d);
```

#### 自减

使变量自身减 1

自减也分两种：

-   a--(后--)
-   --a(前--)

两种方式的区别：

-   后--：先返回变量的值，自身再进行自减
-   前--：先自身进行自减，再返回变量的值

```javascript
const a = 1;
const b = a--;
// 输出：0 1
console.log(a, b);

const c = 1;
const d = --c;
// 输出：0 0
console.log(c, d);
```

### 逻辑运算符

#### 非

! 可以用来对一个布尔值进行取反操作

如果对一个非布尔值进行运算，则会先将其转换为布尔值再进行运算

```javascript
// 输出：false
console.log(!true);
```

#### 短路与

&& 可以对符号两侧进行短路与运算并返回结果

与运算的规则：

-   符号两侧的值都为 true 时，返回 true

-   也就是说，如果符号两侧中任意一侧或全部的值为 false，则返回 false

你可以当作短路与是与的优化版

在两侧都是布尔值的情况下，短路与的运算规则：

-   如果左侧的值为 true，继续运算右侧的值，根据右侧的结果返回不同的布尔值
-   如果左侧的值为 false，不继续运算右侧的值，直接返回 false

```javascript
// 输出：true
console.log(true && true);

// 输出均为 false
console.log(false && true);
console.log(true && false);
console.log(false && false);
```

#### 短路或

|| 可以对符号两侧进行短路与运算并返回结果

或运算规则：

-   符号两侧中任意一侧的值为 true，返回 true

-   也就是说，如果符号两侧的值都为 false，才返回 false

你可以当作短路或是或的优化版

在两侧都是布尔值的情况下，短路或的运算规则：

-   如果第一侧的值为 false，继续运算另一侧的值，根据右侧的结果返回不同的布尔值

-   如果第一侧的值为 true，不继续运算另一侧的值，直接返回 true

```javascript
// 输出均为 true
console.log(true || true);
console.log(true || false);
console.log(false || true);

// 输出：false
console.log(false || false);
```

#### 非布尔值运算

当 && || 对非布尔值进行运算时，会先将其转换为布尔值再进行运算，且返回值为原值

对于短路与，让我们来看看下面的代码：

```javascript
// true && true
// 输出：2
console.log(1 && 2);

// true && false
// 输出：0
console.log(1 && 0);

// false && true
// 输出：0
console.log(0 && 1);

// false && false
// 输出：0
console.log(0 && NaN);
```

可以看到，当左侧的值是 true 时，无论右侧的值是 true 还是 false，都是将右侧的值作为返回值；

当左侧的值是 false 时，无论右侧的值是 true 还是 false，都是将左侧的值作为返回值

上文写的对布尔值的运算规则着重介绍返回值的内容，在这里我们则着重介绍返回值的取值规则：

-   当左侧的值是 true 时，直接返回右侧的值
-   当左侧的值是 false 时，直接返回左侧的值

对于短路或，让我们来看看下面的代码：

```javascript
// true && true
// 输出：1
console.log(1 || 2);

// true && false
// 输出：1
console.log(1 || 0);

// false || true
// 输出：1
console.log(0 || 1);

// false || false
// 输出：NaN
console.log(0 || NaN);
```

可以看到，当左侧的值是 true 时，无论右侧的值是 true 还是 false，都是将左侧的值作为返回值；

当左侧的值是 false 时，无论右侧的值是 true 还是 false，都是将右侧的值作为返回值

同样，上文的运算规则也着重介绍返回值的内容，在这里我们则着重介绍返回值的取值规则：

-   当左侧的值是 true 时，直接返回左侧的值

-   当左侧的值是 false 时，直接返回右侧的值

### 赋值运算符

顾名思义，它是集赋值与运算于一体的运算符

```javascript
let a = 0;
// 等于 a = a + 1;
a += 1;
// 输出：1
console.log(a);

let b = 2;
// 等于 b = b * 3;
b *= 3;
// 输出：6
console.log(b);
```

### 关系运算符

通过关系运算符可以比较两个值的大小关系，如果关系成立，返回 true，如果不成立，返回 false

#### 非数值的运算

对非数值进行运算时，会先将其转换为数值再进行运算

注：

1. 任何值与 NaN 参与运算，返回值都是 false
2. 符号两侧都是字符串时，不会进行类型转换，而是比较两者的 Unicode 编码，对两个字符串中的每个字符的 unicode 编码一一进行数值比较，如果在某一位中两个字符相同，则在下一位字符进行比较，直到比较出结果后将其作为返回值

```javascript
// 输出均为false
console.log(1 > NaN);
console.log(Infinity > NaN);

/*
 * 输出：false
 * 解析：两个字符串的第一位字符1和5中，5的unicode编码数值大于1，结果为false
 * 比较出结果了，忽略掉下一位字符的比较，直接返回false
 */
console.log('11' > '5');

/*
 * 输出：true
 * 解析：两个字符串的第一位字符相同，对第二位字符进行比较
 * 第二位字符c与b中，c的unicode编码数值大于b，结果为true
 * 比较出结果了，也没有下一位字符去进行比较，返回true
 */
console.log('ac' > 'ab');
```

#### Unicode 编码

在 JS 中输出 Unicode 编码，需使用转义字符 \u

用法：\u + 四位数字编码

注：这里的四位数字编码的进制是 16 进制

在 HTML 中输出 Unicode 编码的方式：&# + 数字编码 + ;

注：这里的数字编码的进制是 10 进制

16 进制转 10 进制的方法：parseInt(数字编码, 16);

如果 16 进制的数字前面有 0，记得把 0 去掉

```javascript
// 输出：'a'
console.log('\u0061');
```

```html
<!-- 输出: a -->
<p>&#97;</p>
```

### 相等 & 不相等运算符

#### 相等运算符

相等运算符用于比较两个值是否相等，相等返回 true，不相等返回 false

-   ==：比较两个值是否相等，只比较值是否相等，不比较类型是否相等
-   ===：比较两个值是否相等，当值和类型都相等才成立

#### 不相等运算符

不相等运算符用于比较两个值是否不相等，不相等返回 true，相等返回 false

-   !=：比较两个值是否不相等，只比较值是否不相等，不比较类型是否不相等
-   !==：比较两个值是否不相等，当值和类型中任意一个不相等成立

我推荐使用严格相等 & 不相等运算符，即 === 和 !==，它们对类型的要求更加严格，可以帮助我们更快地发现问题

### 条件运算符

条件运算符也称为三元运算符，我们可以用这个代替一些简单的 if 语句

用法：条件表达式 ? 语句 1 : 语句 2

解析：

1. 当条件表达式的值是 true，执行语句 1，返回执行结果
2. 当条件表达式的值是 false，执行语句 2，返回执行结果

注：我不推荐使用嵌套的三元运算符，这降低了代码的可读性与维护性，你可以选择使用 if 语句代替

### 运算优先级

这里列了一套完整的符号运算优先级表格，[点击查阅](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#table)

表格不需要死记硬背，当使用多个运算符进行运算的结果与你的预期结果不符时，你可以使用括号 () 更改运算优先级

## 代码块

一条条语句构成了一个程序

在 JS 中，语句是按自上而下的顺序执行的，我们使用大括号 {} 为语句进行分组，这个 {} 所包含的语句就是一个代码块

## 流程控制

通过流程控制语句可以控制程序执行流程，使程序可以根据一定条件选择执行语句

### 条件判断语句

#### if

用法：

```javascript
if (条件表达式) {
    语句;
}
```

执行流程：

1. 对条件表达式求值
2. 如果值为 true，执行语句，false 则不执行

```javascript
const a = 1;
// 条件不成立，不执行
if (a > 1) {
    console.log('a > 1');
}
// 完整输出：'a is: 1'
console.log(`a is: ${a}`);

const b = 2;
// 条件成立，执行
if (b > 1) {
    console.log('b > 1');
}
// 完整输出：'b > 1'
// 			'b is: 2'
console.log(`b is: ${b}`);
```

#### if...else

用法：

```javascript
if (条件表达式) {
    语句;
} else {
    语句;
}
```

执行流程：

1. 对条件表达式求值
2. 如果值为 true，执行 if 后面的语句，false 则执行 else 后面的语句

```javascript
const a = 1;
// 不成立，输出：'a <= 1'
if (a > 1) {
    console.log('a > 1');
} else {
    console.log('a <= 1');
}
```

#### if...else if...else

```javascript
if (条件表达式) {
    语句;
} else if (条件表达式) {
    语句;
} else {
    语句;
}
```

执行流程：

1. 从上而下依次对表达式进行求值判断
2. 如果值为 true，执行当前语句，false 则继续向下判断
3. 如果所有条件都不成立，执行 else 后面的语句

```javascript
const a = 73;
// 输出：'a > 70'
if (a > 90) {
    console.log('a > 90');
} else if (a > 80) {
    console.log('80 < a <= 90');
} else if (a > 70) {
    console.log('70 < a <= 80');
} else if (a > 60) {
    console.log('60 < a <= 70');
} else {
    console.log('a <= 60');
}
```

在该语句中，只会有一个代码块会被执行，一旦代码块被执行了，则直接结束语句

相关练习：[if 语句练习](https://github.com/PikaSama/CodePractice/blob/master/JavaScript/if%E7%BB%83%E4%B9%A0/index.html)

### 条件分支语句

#### switch

用法：

```javascript
switch (条件表达式) {
    case 条件表达式: {
        语句;
        break;
    }
    default: {
        语句;
        break;
    }
}
```

执行流程：

1. 计算 switch 后面的表达式的值
2. 将 switch 后面表达式的值与每个 case 的表达式的值进行严格相等比较(===)，如果为 true，从当前 case 执行代码，false 则继续向下比较
3. 如果所有 case 的比较结果都是 false，执行 default 处的代码

注：break 语句用于退出 switch 语句，如果没有 break 语句，那么代码会从当前 case 处执行到 default 处

我推荐对每一个 case 以及 default 加上大括号 {}，这样在语句中有词法声明时，不会造成变量污染的问题

switch 语句可以与 if 语句有着相似的功能，两者在一定情况下可以互换使用，它们在不同的应用场景下有着各自的优势，可根据需求选择

```javascript
const a = 1;
// 输出：'壹'
switch (a) {
    case 1: {
        console.log('壹');
        break;
    }
    case 2: {
        console.log('贰');
        break;
    }
    default: {
        console.log('Error');
        break;
    }
}
```

### 循环语句

#### while

用法：

```javascript
while (条件表达式) {
    语句;
}
```

执行流程：

1. 对表达式求值，如果为 true 执行代码块，如果为 false 终止循环
2. 执行代码块后重复第一步

我们可以使用 break 语句终止循环，还可以使用 continue 语句跳过本次循环

```javascript
let num = 0;
// 输出：0....9
while (num < 10) {
    console.log(num);
    num += 1;
}
```

相关练习：

#### do...while

用法：

```javascript
do {
    语句;
} while (条件表达式);
```

执行流程：

1. 执行代码块
2. 对表达式求值，如果为 true 重复第 1、2 步，false 则终止循环

```javascript
let num = 0;
// 输出：0...9
do {
    console.log(num);
    num += 1;
} while (num < 10);
```

#### while 与 do...while 的区别

注：此处假设表达式的值一直为 true

while 的执行流程：求值判断 --> 执行语句 --> 求值判断 --> 执行语句...

do...while 的执行流程：执行语句 --> 求值判断 --> 执行语句 --> 求值判断...

可以看出，while 是先判断后执行，do...while 是先执行后判断

do...while 可以保证代码块至少执行一次

#### for

用法：

```javascript
for (初始化表达式; 条件表达式; 更新表达式) {
    语句;
}
```

执行流程：

1. 执行初始化表达式，初始化变量
2. 计算条件表达式的值，判断是否需要执行循环
3. 如果值为 true 执行循环，执行完毕后转到第 4 步，false 则终止循环
4. 执行更新表达式，执行完毕后回到第 2 步

```javascript
// 打印1-100的数
for (let i = 1; i <= 100; i += 1) {
    console.log(i);
}
```
