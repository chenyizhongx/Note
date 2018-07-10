##一 变量和数据类型
ECMAScript的变量是松散类型的，可以保存任何类型的数据。
#####1）定义局部变量
注意var操作符定义的变量将成为定义该变量的作用域中的局部变量。
用var在函数中定义一个变量，则该变量在函数退出后会销毁。这种情况下就是定义了局部变量。

#####2）定义全局变量
-   直接使用var，注意该句不能包含在function内，否则是局部变量

          var test;
          var test = 5;   
- 不使用var，隐式声明全局变量，注意只有调用过test() 函数才能创建该全局变量。不推荐使用这种方法定义全局变量。

        function test() {
            message = "hi";
        }   
        test();
        alert(message);
## 二 数据类型
ECMAScript有五种简单的数据类型（Undefined， Null，Boolean，Number，String)和一种复杂的数据类型Object

#####1）typeof 操作符
typeof是一种操作符而不是函数，可以用来检测给定变量的数据类型
#####2）Undefined 类型
Undefined类型只有一个值 undefined，在使用var声明变量但未对其加以初始化的时候，变量的值就是undefined。
但也存在区别，若运行alert(message); 如果message已经被声明但未被初始化，则弹出undefined，反之则会产生错误。
#####3）Null 类型
Null类型同样只有一个值 null，null实际上是一个空对象指针，因此用typeof检测null时会返回object。
位于null和undefined之间的相等操作符(==)总是返回true，但两者还是存在区别的。如果定义的变量准备在将来用于保存对象，则最好将该变量保存为null而不是undefined或者其他值。
#####4）Boolean 类型
  Boolean 类型有两个值 true和false ，注意区分大小写，True和False不是Boolean值。
#####5）Number 类型
- 浮点数值运算存在舍入误差
- 数值范围
Number.MIN_VALUE ~ Number.MAX_VALUE(5e-324 ~ 1.7976e+308)
超出范围则转换为（-Infinity ~ Infinity）
- NaN(not a number)
  该数值用于表示一个本来要返回数值的操作数未返回数值的情况，防止抛出错误。例如在其他编程语言中任何数值除以非数值会导致错误，导致代码停止执行，但在ECMAScript中会返回NaN，不影响代码的执行。

        alert(NaN == NaN)  //false
        alert(isNaN(true))    //false
        alert()isNaN("blue")   //true
- 数值转换  
三个函数可将数值转换为非数值：Number(),  parseInt(),  parseFloat()

   (1) Number()

        var n1 = Number("");       //0
        var n2 = Number("0003");   //3
        var n3 = Number(true);     //1
   (2) parseInt()

        var n1 = parseInt("123blue")    //123
        var n2 = parseInt("")      //NaN
        var n3 = parseInt("10",  8);    //8   (按八进制解析)
   (3) parseFloat()

        var n1 = parseFloat("22.34.5")   //22.34
     parseFloat()只解析十进制
#####6）String 类型
将一个值转化为一个字符串有两种方式，一种是toString(),一种是String()

    var age = 11;
    var ageAsString = age.toString();    //字符串11
    var num = 10;
    alert(num.String(8));          //"12"    八进制输出
toString()方法会改变输出的值，而String()能够将任何类型的值转化为字符串。null返回null, undefined 返回 undefined
#####7）Object 类型
    var o = new Object();
Object的每个实例都具有下列属性和方法

- constructor：保存着用于创建当前对象的函数
- hasOwnProperty(propertyName):  用于检查给定的属性在当前对象的实例中是否存在 如：0.hasOwnProperty("name")
- isPrototypeOf(object): 用于检查传入的对象是否为当前对象的原型
- propertyIsEnumberable(propertyName): 用于检查给定的属性能否使用for-in 来枚举
- toLocaleString(): 返回对象的字符串表示，该字符串与执行环境的地区对应
- toString(): 返回对象的字符串表示
- valueOf(): 返回对象的字符串，数值或布尔值表示。通常与toString()方法的返回值相同
##三 操作符
- 一元操作符
-  位操作符
- 布尔操作符
   共有三个：非(NOT)  与(AND)   或(OR)
- 乘性操作符
- 加性操作符
- 关系操作符
 
        var result = "23"  < "3";   //true 比较字符编码
        var result = "23"  < 3;   //false "23" 会被转化位23再和3进行比较
        //任何操作数和NaN进行关系比较，结果都是false
        var result1 = NaN < 3;     //false
        var result2 = NaN >= 3;    /false
- 相等操作符
        
        //相等和不相等： ==     会进行类型转换
        var result = (true == 1)    // true
        //全等和不全等 ===  不会进行类型转换
        var result = (true === 1)     //false
- 条件运算符
- 赋值运算符
- 逗号操作符
  
        var num = (5, 1, 3, 4);    //num的值位4 ，去最后一位

      
##四 语句
#####1）for-in 语句    
          
      for ( property in expression) statement

   可以用来枚举对象的属性
#####2）label 语句
    start：for (var i = 0; i < count; i++)  {
          alert(i);    
    }
start 标签可以将来由break或continue语句引用
#####3）with 语句
将代码的作用域设置在一个特定的对象中

    with (expression)  statement;   //大量使用易导致性能下降，不建议使用
##五 函数参数
ECMAScript函数可以传递多个参数，而与定义的函数接收的参数数无关。
参数的永远与对应命名参数的值保持同步。
注意： ECMAScript函数没有重载
