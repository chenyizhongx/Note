

## 一 JS的声明

- 变量声明，无论发生在什么时候，都会在执行任何代码前进行处理。
- 用var声明的变量的作用域是它当前的执行上下文，它可以是嵌套的函数，也可以是声明在任何函数外的变量。



        console.log(a); // undefined
        var a = 1;
上面的代码不会报错，而是输出 undefined，这是由于js 中变量的声明提升，所以不会报错，但也只是声明提升，**赋值操作没有提升**，所以不会输出 1，而是undefined。


        function do_something() {
          console.log(bar); // undefined
          var bar = 111;
          console.log(bar); // 111
        }
    
        // is implicitly understood as: 
        function do_something() {
   		  var bar;
  	      console.log(bar); // undefined
          bar = 111;
          console.log(bar); // 111
        }


- 赋值给未声明的变量，当执行时会隐式创建全局变量（成为global的属性）。

###### 声明变量和未说明变量的区别
- 声明变量通常是局部的，未声明变量通常全局的。
- 声明变量在任意代码执行前创建，*未声明变量直到赋值时才存在*。
- 声明变量是execution context（function/global）的non-configurable 属性，未声明变量则是configurable。

## 二 变量的初始化和解析顺序
##### 1）留意顺序
    var x = y, y = 'A';
    console.log(x + y); // undefinedA
在这里，x 和 y 在函数执行前就已经创建了，而赋值操作发生在创建之后，当 x = y 执行时， y已经存在，所以不抛出ReferenceError，并且它的值是'undefined'。所以x被赋予 undefined 值。然后，y被赋予'A'。于是在执行完第一行之后，x === undefined && y === 'A'才出现了这样的结果。
##### 2）多个变量的重复初始化

    var x = 0;

    function f(){
      var x = y = 1; // x在函数内部声明，y不是！
      console.log(x, y);    //  1, 1
    }
    f();

    console.log(x, y);   // 0, 1
    // x是全局变量。
    // y是隐式声明的全局变量。 
在js中，变量的声明和初始化是俩个发生时间不同的过程，一定要分开讨论。

实际上上面的例子可以引出 JS 中的重要特性：
  在函数题内部，局部变量的优先级比同名的全局变量要高。那么**在声明局部变量的函数体范围**内，局部变量将覆盖同名的全局变量。

    var variable = 'global';
    function checkVariable () {
    	var variable = 'local';
    	console.log(variable);      //  local
    }
    checkVariable();     
    console.log(variable);     //  global
和下面这个例子对比

    variable = 'global';
    function checkVariable() { 
	    variable = 'local';
    	console.log(variable); // local
        myVariable = 'local'; 
        console.log(myVariable); // local
    }
    checkVariable(); 
    console.log(variable); // local
    console.log(myVariable)  // local
在全局作用域中声明变量可以省略 var 关键字，但如果在函数体内，即局部作用域中声明变量时省略 var 关键字，则会导致声明覆盖，使得输出值全部为local。
## 三 声明提升
在JS的声明提升中，需要综合考虑一般变量和函数。
在JavaScript中，一个变量名进入作用域的方式有 4 种：

- Language-defined：所有的作用域默认都会给出 this 和 arguments 两个变量名（global没有arguments）;
- Formal parameters（形参）：函数有形参，形参会添加到函数的作用域中;
- Function declarations（函数声明）：如 function foo() {};
- Variable declarations（变量声明）：如 var foo，包括_函数表达式_。

**重点：** 
**函数声明和变量声明总是会被移动（即hoist）到它们所在的作用域的顶部（这对你是透明的）。**

**而变量的解析顺序（优先级），与变量进入作用域的4种方式的顺序一致。**

下面具体来分析一下声明提升的具体实现。

	function cheak() {
		var a = 1;
		function inner() {
			a = 100;
		}
		inner();
		console.log(a); //100
	}
	cheak();
变量a 在整个函数体内都能够使用，并可以重新赋值。但要注意分析，如下例子。

	var a = 1;
	function cheak() {
		console.log(a);   //undefined
		var a = 2;
		console.log(a); // 2
	}
	cheak();
实际上等同于

	var a = 1;

	function cheak() {
		var a;
		console.log(a);   //undefined
		a = 2;
		console.log(a); // 2
	}
	cheak();
## 四 总结
JS变量的域使根据方法块来划分的（也就是说以function的一对 { }来划分）
注意：**是function块，而不是for、while和if块**，在JS中没有块级作用域

	var a = 1;
	
	function cheak() {
		console.log(a);   　 // undefined
		a = 2;
		console.log(a);       //  2
		var a;
		console.log(a);     // 2
	}
	
	cheak();
	console.log(a);     // 1
以及


    function testOrder(arg) {
	  console.log(arg); // arg是形参，不会被重新定义
	  console.log(a);        // 因为函数声明比变量声明优先级高，所以这里a是函数
	  var arg = 'hello'; // var arg;变量声明被忽略， arg = 'hello'被执行
	  var a = 10; // var a;被忽视; a = 10被执行，a变成number
	  function a() {
	    console.log('fun');
	  } // 被提升到作用域顶部
	  console.log(a); // 输出10
	  console.log(arg); // 输出hello
	}; 
	testOrder('hi');
	/* 输出：
	hi 
	function a() {
	        console.log('fun');
	    }
	10 
	hello 
	*/
注意：函数声明比变量声明优先级高

参考资料：
- https://github.com/creeperyang/blog/issues/16
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/var
- https://www.w3cplus.com/javascript/the-basics-of-variable-scope-in-javascript.html

