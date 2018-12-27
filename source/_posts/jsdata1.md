---
title: Javascript数据类型
date: 2018-12-03 09:27:04
tags:
	- js
categories: "js"
---

## 1.区分大小写
>Html/css不区分大小写，Html.data属性，名字会自动转小写，类似 `JQuery.data()` 方法

## 2.变量
>**var定义** 局部变量：不可被删除；变量可被提前；
**不用var** 全局变量（window一个属性）：可被删除（对象的属性可以被删除）；不可提前（属性是无序的）

## 3.数据类型
>**基础数据类型** 原始数据类型/不可改变数据类型
>**对象** 复杂数据类型/可变数据类型、可使用属性、方法

## 4. typeof
> 是操作符，返回值都是**字符串**
> `typeof(null) // object`

## 5.undefined、null、NaN
> **undefined** 一个变量已定义，但是没有值 `typeof未初始化，未声明变量，会返回该值`
> **null** 空指针对象 `undefined == null //true`
> **NaN** 是数值，不等于任何数值 `NaN == NaN //false`

<table class="tg"><tr><th class="tg-c6of"></th><th class="tg-3xi5">undefined</th><th class="tg-3xi5">null</th></tr><tr><td class="tg-c6of">相同点</td><td class="tg-c6of" colspan="2">都只有一个值，参与判断返回false,不能调用方法</td></tr><tr> <td class="tg-3xi5" rowspan="5">不同点</td><td class="tg-c6of">不是关键字</td><td class="tg-c6of">是关键字</td></tr><tr><td class="tg-c6of">是属性，值叫“未定义”</td><td class="tg-c6of">对象，空的</td></tr><tr><td class="tg-c6of">未初始化</td><td class="tg-c6of">已初始化</td></tr><tr><td class="tg-c6of">typeof返回undefined</td><td class="tg-c6of">typeof返回object</td></tr><tr><td class="tg-c6of">转数字结果为NaN</td><td class="tg-c6of">转数字结果为0</td></tr></table>

## 6.Boolean
>对一个变量使用Boolean，相当于使用`!!`
6种数据转Boolean为false：`undefined, null, 0, -0, "", NaN`
>[], {} 转Boolean为**true**

## 7.Number
>包括常规数字，无穷， NaN
>对于常规数字，尽量用十进制，少用十六进制，不用八进制
>**十进制** 数字范围：
```math
-2^{53} - 2^{53}
```
>**八进制** 严格模式下无效
>**浮点数** 计算机中，所有数字都是二进制，不是所有小数都能用二进制表示。（只能表示小数点后52位）**永远不要比较小数**，如果一定要比较，乘以10的倍数转成整数再比较
>** 除以0** 正常情况下返回`Infinity/-Infinity`; `0/0 -> NaN`
>**IsNaN()** 判断传入的值是否可以转为数字

## 8.数值转换
>几种特殊数据转换：
>`Number(undefined) -> NaN`
>`Number(null) -> 0`
>`Number("") -> 0`
>`Number([]) -> 0`
>`Number({}) -> NaN`
>**Number()** 适用任何类型
>**parseInt()** 适用于字符串
>**parseFloat()** 适用于字符串
>在使用parseInt(), parseFloat()，尽量使用“10”做第二个参数，代表**传进来**的第一个参数是用什么表示的。

## 9.String
>**转义字符** 长度1；\u03a3 长度1
>**string()** 适用于任何类型

<html><table class="tg"><tr><td class="tg-3xi5" rowspan="5">toString()</td><td class="tg-c6of">undefined, null没有该方法</td></tr><tr><td class="tg-c6of">number.toString(2~16的参数)转为对应进制的数字</td></tr><tr><td class="tg-c6of">number,bool,string,object都有该方法</td></tr></table></html>


**Number(),Boolean(),String()相同点** 

| Number(),Boolean(),String() |  |
| --- | --- |
|  都有同名函数，首字母大写|  |
| 可转换任意数据类型 |  |
|得到的结果都和自己对应  |  |
|结果分成两类（对象传到String()里面，转换出带object的字符串  |  |


| String() | new String() |
| --- | --- |
|  原生类型，一种内建函数|  转出object|

**valueOf** 适用于Boolean,Number,String,Object

## 10.Object
>各种属性构成的无序集合，由键-值（包括**普通数据**、**对象**、**方法**）对组成


| 原始数据类型 |  对象|
| --- | --- |
| 无属性 |有属性  |
| 无方法| 有方法 |
| 不可改变 | 可改变 |
| 比较值 | 比较引用 |


<html><table class="tg"><tr><td class="tg-3xi5" rowspan="5">对象分类</td><td class="tg-c6of">内部对象（17个）：常用对象（8个：bool,string,number,数组,Date,function,object,正则），错误对象，内置对象（Math,Global,Json：可不使用new)</td></tr><tr><td class="tg-c6of">数组对象（用得较多的是window,document)</td></tr><tr><td class="tg-c6of">自定义对象</td></tr></table></html>

### 基础数据类型转为object
```javascript
bool -> object // {'原始值'： true}
number -> object // {'原始值' ： 数字}
string -> object //  {
    '原始值' ：'abc',
    'length'：3,
    '0': 'a',
    '1': 'b',
    '2': 'c'
}
undefined/null -> object // {} （但是不能使用对象的方法）
new Object(undefined) // {}
new Object(null)  // {} (现在浏览器不报错，但是实际是不能转化)
```

### 创建对象的方法
1. 对象直接量
```javascript
var obj = {
    a: 123,
    b: "hello"
}
```
2. new Object() （括号可省略，不推荐这么使用）
3. create方法（ES5）

### 属性查询
`obj.key` <=> `obj[key]`
1. 先计算obj是否`undefined`或者`null`，是的话报错
2. 先计算obj是否对象，不是，转对象
3. 是对象后，若是`.`操作，把`.`后属性对应的值返回；若是`[]`操作，先计算括号内内容，转字符串，然后返回字符串的值
4. 找不到值，返回`undefined`

**`object -> 数字` 先valueOf(), 再toString()；**

**`object -> 字符串` 先toString(), 再valueOf()**

```javascript
[]（转换）->数字（先valueOf()) -> [](再toString()) -> "" (再转数字) -> 0
{}（转换）->数字（先valueOf()) -> {}(再toString()) -> "[object object]" (再转数字) -> NaN
object -> bool //全是true
Array -> toString() // 逗号组合项的字符串
function -> toString() //源代码
日期 -> toString() //日期时间组合字符串
valueOf(): 有原始值，返回值；无原始值，返回object本身（Date除外，返回至1970年以来的毫秒数）
```

## 11. 表达式
> `1+1` 两个1是表达式， + 是操作符

1. 原始表达式 **常量、变量、直接量、关键字**
2. 初始化表达式 **对象、数组**
3. 函数表达式
4. 函数调用表达式
5. 属性访问表达式 **. []**
6. new对象创建表达式

## 12.一元操作符
**只能操作一个值的操作符**

**一元加** 把想操作的内容转成数字，对一个数字使用一元加，结果仍是数字本身

**一元减** 将数字转成负数（可把内容转为数字，取反）

>一元加，一元减，都会先转数字

**递增/递减** 
```javascript
var age = 29; ++age
var age = 29; age = age + 1
//如果age是数字，两者一样；如果age是字符串，两者不一样
```
>前置：先加减，再运算；后置：先运算，再加减

>副效应：做递增或递减时，会产生副作用，对后续操作造成影响

**优先级** 

1. `. [] `
1. 一元操作符 
1. `* / `
1. `+ - `
1. `> <` 
1. 相等 
1. `& `
1. `||`
1. 三目操作符
1. 赋值

**结合性**

正常情况下**左结合**， 只有*一元操作符、三目操作符、赋值*是**右结合**

**运算顺序** 
表达式内包含子表达式，永远从左往右运行

**逻辑非** 把内容转为布尔值，取反

**逻辑与&&** 相当于“如果。。。并且。。。”，用于判断语句；返回值并不一定是布尔；当第一个操作数为true，有可能返回第二个操作数

**逻辑或||** 相当于“如果不。。。那么。。。”，用于赋值
```javascript
a && b || c //如果a，那么b，否则c （if else)
// a?b:c
```

<table class="tg"><tr><th class="tg-c6of"></th><th class="tg-3xi5">&&</th><th class="tg-3xi5">||</th></tr><tr><td class="tg-c6of">相同点</td><td class="tg-c6of" colspan="2">返回值是运行到的表达式的返回值</td></tr><tr><td class="tg-3xi5" rowspan="5">不同点</td><td class="tg-c6of">判断前面条件是否为true</td><td class="tg-c6of">返回值比较重要，用于赋值</td></tr></table></html>

**typeof、void、delete也是一元操作符，不是函数**

## 13. 乘性操作符
`*，除，模运算`
>模运算，结果不一定是整数；当参与运算中涉及小数，需先转整数，再运算

## 14. 加性操作符
**+** 用于数字/字符串，返回结果不同
>如果有一个字符串，把另一个也转字符串

`[]+[] // ""`

`{}+{} // "[object object][object object]"`

`undefined + undefined // NaN`

`null + null // 0`

**-** 与乘性操作符用法相同，会先把两边的内容先转数字，再运算

**隐式类型转换** +，-，++，--（转数字）

**显示类型转换** Number(), Boolean(), String()

## 15. 关系操作符 
可比较数字、字符串。结果是布尔。字符串比较，是对编码序列做比较
>如果有一个数值，把另一个也转数值；有一个`NaN`，结果为false

## 16. 相等操作符
>混合运算时（数字vs字符串，数字vs布尔，数字vs布尔），需要先转数字，再比较；如果有对象，先转原始值；如果上述均不成立，返回false；

`undefined/null == false // false`

`undefined == null // true`

`"1" == true // true`

`对象不等于对象`对象比较，判断引用，任何两个独立对象不相等；

`NaN不等于NaN`

`undefned, null和自身相等`

## 17. 赋值操作符
>左侧必须是变量或对象的属性，右边任意类型

**不要使用连等赋值** 

```javascript
var a=b=1 // b变成全局变量，没有var
num = num + 10 <=> num += 10 //没有性能提升 +可替换为-，*，、，%
```
### 扩展
**+**
- 偏向于字符串 **操作数里有一个字符串，就把另一个也转字符串**
- undefined, null, bool, number 4者进行混合运算 **都会先转数字**
- 对象参加加法运算，需先转原始值 **先调用对象的valueof()方法，然后toString(),但是Date除外，直接调用toString()**


**{}**

除了代表对象，也代表代码区域
```javascript
{a:1} + 1 // 1
//{}在前，JS解析时会认为是一段代码结束，直接变成“+1”，得到1
```
```javascript
[]+{} // "[object object]"
{}+[] // 0 (相当于+[])
```
