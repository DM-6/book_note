# book_note
读书笔记

# 《JavaScript语言精粹》读书笔记——第四章 函数

函数包含一组语句，它们是JavaScript的基础块单元，用于代码复用、信息隐藏和组合调用。函数用于指定对象的行为。

------

## 函数对象
在JavaScript中，**函数**就是**对象**，可以像其他的值一样被使用。函数可以存放在变量、对象和数组中；可以被当作参数传递给其他函数；可以再返回函数；也可以像对象一样拥有方法。

## 函数字面量
函数对象可以通过函数字面量来创建：
```
//创建一个名为add的变量，并用来把两个数字相加的函数赋值给它。
var add = function(a,b){
    return a+b;
};
```
函数字面量包括四个部分：
> * 保留字function
> * 函数名（可省略）
> * 圆括号中的一组参数（参数之间用逗号分隔）
> * 花括号中的一组语句（函数的主体）

函数字面量可以出现在任何允许表达式出现的地方。函数也可以被定义在其他函数中。

## 调用
调用一个函数将暂停当前函数的执行，传递控制权和参数给新函数。除了声明时定义的形式参数，每个函数接收两个附加的参数：this和arguments。参数this的值取决于调用的模式。在JavaScript中，有四种**调用模式**：
> * 方法调用模式
> * 函数调用模式
> * 构造器调用模式
> * apply调用模式

### 一、方法调用模式
当一个函数被保存为对象的一个属性时，我们称它为一个**方法**。
```
//创建myObject对象。它有一个value属性和一个increment方法。
//increment方法接受一个可选的参数。如果参数不是数字，那么默认使用数字1。
var myObject = {
   value: 0;
   increment: function (inc) {
      this.value += typeof inc === 'number' ? inc : 1;
   }
};

myObject.increment();
document.writeln(myObject.value);     // 1

myObject.increment(2);
document.writeln(myObject.value);     // 3
```
方法可以使用this访问自己所属的对象，从对象中取值或者对对象进行修改。**“超级”延迟绑定**（very late binding）使的函数可以对this高度调用。通过this可取得它们所属对象的上下文的方法称为**公共方法**（public method）。

### 二、函数调用模式
当一个函数并非一个对象的属性时，那么它就是被当做一个函数来调用的：
```
var sum = add(3,4);     // sum的值为7。
```
语言设计错误，因为以此模式调用函数时，this被绑定到全局对象。

解决方案： 
该方法定义一个变量并给它赋值为this，那么内部函数就可以通过那个变量访问到this。按照约定，变量命名为that：
```
// 给myObject增加一个double方法。
myObject.double = function(){
   var that = this;     // 解决方法
   
   var helper = function(){
      that.value = add(that.value,that.value);
   };
   
   helper();      // 以函数的形式调用helper。
};
   
// 以方法的形式调用double。
myObject.double();
document.writeln(myObject.value);      // 6
```

### 三、构造器调用模式
JavaScript是一门基于**原型**继承的语言，对象可以直接从其他对象继承属性。该语言是**无类型**的。JavaScript提供了一套和基于类的语言类似的对象构建语法。

如果在一个函数前面带上new来调用，那么背地里将会创建一个连接到该函数prototype成员的新对象，同时this会被绑定到那个新对象上。

new前缀也会改变return语句的行为。
```
// 创建一个名为Quo的构造器函数。它构造一个带有status属性的对象。

var Quo = function(string){
   this.status = string;
};

// 给Quo的所有实例提供一个名为get_status的公共方法。

Quo.prototype.get_status = function(){
   return this.status;
};

// 构造一个Quo实例。

var mrQuo = new Quo("confused");document.writeln(myQuo.get_status());   // 打印显示“confused”。
```
一个函数，如果创建的目的是结合new前缀来调用，那它就被称为**构造器函数**。（不推荐使用）

### 四、apply调用模式
JavaScript是一门函数式的面向对象编程语言，可以拥有方法。

apply方法让我们构建一个参数传递给调用函数。允许选择this的值。apply方法接收两个参数：要绑定给this的值；一个参数数组。
```
// 构建一个包含两个数字的数组，并将它们相加。

var array = [3,4];
var sum = add.apply(null,array);     // sum值为7.

// 构造一个包含status成员的对象。

var statusObject = {
   status: 'A-OK'
};

//statusObject并没有继承自Quo.prototype，但我们可以在statusObject上调
//用get_status方法，尽管statusObject并没有一个名为get_status的方法。

var status = Quo.prototype.get_status.apply(statusObject);
// status值为 'A-OK'。
```

## 参数
函数被调用时，会得到一个“免费”配送的参数，就是**arguments**数组。

arguments并不是一个真正的数组，只是一个“类似数组（array-like）”的对象。拥有一个length属性，但没有任何数组的方法。

## 返回
**return**语句可以使函数提前返回。当return被执行时，函数立即返回而不再执行余下的语句。

一个函数会返回一个值。如果没有指定返回值，则返回undefined。

如果函数调用时在前面加上了new前缀且返回值不是一个对象，则返回this（该新对象）。

## 异常
JavaScript提供了一套**异常处理机制**。异常是干扰程序的正常流程的不寻常（但并非完全是出乎意料的）的事故。

## 给类型添加方法
## 递归
## 作用域
## 闭包
## 回调
## 模块
## 级联
## 套用
## 记忆