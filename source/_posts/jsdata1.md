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
>
<style type="text/css">
.tg {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c6of{background-color:#ffffff;border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-3xi5{background-color:#ffffff;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
<tr>
<th class="tg-c6of"></th>
<th class="tg-3xi5">undefined</th>
<th class="tg-3xi5">null</th>
</tr>
<tr>
<td class="tg-c6of">相同点</td>
<td class="tg-c6of" colspan="2">都只有一个值，参与判断返回false,不能调用方法</td>
</tr>
<tr> <td class="tg-3xi5" rowspan="5">不同点</td>
<td class="tg-c6of">不是关键字</td>
<td class="tg-c6of">是关键字</td>
</tr>
<tr>
<td class="tg-c6of">是属性，值叫“未定义”</td>
<td class="tg-c6of">对象，空的</td>
</tr>
<tr>
<td class="tg-c6of">未初始化</td>
<td class="tg-c6of">已初始化</td>
</tr>
<tr>
<td class="tg-c6of">typeof返回undefined</td>
<td class="tg-c6of">typeof返回object</td>
</tr>
<tr>
<td class="tg-c6of">转数字结果为NaN</td>
<td class="tg-c6of">转数字结果为0</td>
</tr>
</table>

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