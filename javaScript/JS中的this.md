在绝大多数情况下，函数的调用方式决定了`this`的值。在绝大多数情况下，函数的调用方式决定了`this`的值。`this`不能在执行期间被赋值，并且在每次函数被调用时`this`的值也可能会不同。ES5引入了[bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)方法来设置函数的`this`值，而不用考虑函数如何被调用的 
## 一 隐式绑定

`this`是非常特殊的关键词标识符，在每个函数的作用域中被自动创建。    

当函数被调用时，一个 activation record（或者说执行上下文）被创建。这个record包涵信息：函数在哪调用（call-stack），函数怎么调用的，参数等等。record的一个属性就是`this`，指向函数执行期间的`this`对象。  

- 无论是否在严格模式下，在全局执行上下文中（在任何函数体外部）`this` 都指代全局对象。
- `this`的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定`this`到底指向谁
- `this`的上下文基于函数调用的情况。和函数在哪定义无关，而和函数怎么调用有关。
- 实际上`this`指向的是那个调用它的对象。


        function foo() {
          console.log( this.a )
  	    }

  	    var obj2 = { 
   	     a: 42,
  	      foo: foo
 	     }

 	     var obj1 = {
  	      a: 2,
  	      obj2: obj2
   		 }

  	    obj1.obj2.foo(); // 42   即前一个调用它的对象obj2

结合上面的例子，`this`的指向可以总结为一下三种情况。

**情况1**：如果一个函数中有`this`，但是它没有被上一级的对象所调用，那么`this`指向的就是`window`，在js的严格版中this指向的不是`window`，而是`undefined`。

**情况2**：如果一个函数中有`this`，这个函数有被上一级的对象所调用，那么`this`指向的就是上一级的对象。

**情况3**：如果一个函数中有`this`，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，`this`指向的也只是它上一级的对象，如下例子：

    var o = {
        a:10,
        b:{
            // a:12,
            fn:function(){
                console.log(this.a); //undefined
            }
        }
    }
    o.b.fn();
**还有一种比较特殊的情况：**
  
    var o = {
    a:10,
    b: {
           a:12,
           fn:function(){
            console.log(this.a);   //undefined
            console.log(this);    //window
        }
      }
    }
    var j = o.b.fn;
    j();
赋值操作和调用不是同一个概念，上面说的 `this`是指向上一级调用它的对象，跟赋值操作没有上面关系。例子中虽然函数 fn 是被对象b 所引用，但是在将 fn 赋值给变量 j 的时候并没有执行所以最终指向的是 window。
### 隐式丢失
一个最常见的`this`绑定问题就是 被隐式绑定的函数会丢失绑定对象，也就是说它会应用默认绑定，从而把`this`绑定到全局对象或者`undefined`上，取决于是否是严格模式。

    function foo() {
        console.log( this.a )
    }
    
    var obj1 = {
        a: 2,
        foo: foo
    }

    var bar = obj1.foo; // 函数别名！

    var a = "oops, global"; // a是全局对象的属性

    bar(); // "oops, global"
虽然`bar`是`obj.foo`的一个引用，但是实际上，它引用的是 foo函数本身，因此此时的 bar() 其实是一个不带任何修饰的函数调用，因此应用了默认绑定。


    function foo() {
        console.log( this.a )
    }

    function doFoo( fn ){
        // fn 其实引用的是 foo
        fn(); // <-- 调用位置！
    }
    
    var obj = {
        a: 2,
        foo: foo
    }    

    var a = "oops, global"; // a是全局对象的属性

    doFoo( obj.foo ); // "oops, global"

参数传递其实就是一种隐式赋值，因此我们传入函数时也会被隐式赋值，所以结果和上一个例子一样，如果把函数传入语言内置的函数而不是传入自己声明的函数（如`setTimeout`等），结果也是一样的.

## 二 显示绑定
就是给this指定值，如：`new`，`call`,   `apply`, `bind`绑定等
#### 1)  call 和 apply 绑定
如果想把 `this` 的值从一个上下文传到另一个，就要用 `call `或者 `apply`方法。

    function foo( something ) {
        console.log( this.a, something)
        return this.a + something
    }

    var obj = {
        a: 2
    }

    var bar = function() {
        return foo.apply( obj, arguments)
    }

    var b = bar(3); // 2 3
    console.log(b); // 5


使用 `call `和`apply`函数时要注意，如果传递给`this`的值不是一个对象，JS会尝试使用内部的`ToObject`操作将其转化为对象。因此，如果传递的值是一个原始值比如` 7` ，那么就会使用相关构造函数将它转换为对象，所以原始值` 7 `会被转换为对象，像` new Number(7) `这样。

    function bar() {
       console.log(Object.prototype.toString.call(this));
    }

    //原始值 7 被隐式转换为对象
    bar.call(7); // [object Number]
#### 2) bind 绑定

调用`f.bind(someObject)`会创建一个与f具有相同函数体和作用域的函数，但是在这个新函数中，`this`将永久地被绑定到了`bind`的第一个参数，无论这个函数是如何被调用的。返回由指定的`this`值和初始化参数改造的原函数拷贝

    var module = {
      x: 42,
      getX: function() {
        return this.x;
      }
    }
    console.log(module.getX());     // expected output: 42
    var unboundGetX = module.getX;
    console.log(unboundGetX()); // The function gets invoked at the global         scope
    // expected output: undefined

    var boundGetX = unboundGetX.bind(module);
    console.log(boundGetX());
    // expected output: 42
注意 `bind`指生效一次

    function f(){
      return this.a;
    }

    var g = f.bind({a:"azerty"});
    console.log(g()); // azerty

    var h = g.bind({a:'yoo'}); // bind只生效一次！
    console.log(h()); // azerty

    var o = {a:37, f:f, g:g, h:h};
    console.log(o.f(), o.g(), o.h()); // 37, azerty, azerty
下面这个例子会更好理解一点

    function foo( something ) {
        console.log( this.a, something)
        return this.a + something
    }

    var obj = {
        a: 2
    }

    var bar = foo.bind(obj)

    var b = bar(3); // 2 3
    console.log(b); // 5
#### 3）new 绑定

在传统面向类的语言中，使用`new`初始化类的时候会调用类中的构造函数，但是JS中`new`的机制实际上和面向类和语言完全不同。
使用`new`来调用函数，或者说发生构造函数调用时，会自动执行下面的操作：

- 创建（或者说构造）一个全新的对象
- 这个新对象会被执行`[[Prototype]]`连接
- 这个新对象会绑定到函数调用的`this`

##### 当 this 遇到 return 时
在构造函数中使用`this`分为两种情况

-  函数当作构造函数使用且函数内没有返回值，或返回值是5种基本型（Undefined类型、Null类型、Boolean类型、Number类型、String类型）之一，指向实例后的对象

        function fn()  
        {  
            this.user = 'zhong';      // 没有返回值
        }
        var a = new fn;  
        console.log(a.user);    //zhong

    以及

        function fn()  
         {  
            this.user = 'zhong';  
            return undefined;        // 返回值为五种基本类型，null 等
         }
         var a = new fn;  
         console.log(a.user);    // zhong
- 函数当作构造函数使用且有`return`值，返回是数组、对象等，`this`指向返回值

        function fn()  
        {  
            this.user = 'zhong';   
            return [1,2,3,4];
        }
        var a = new fn;  
        console.log(a);      // 注意这里不是 a.user



## 三 总结

   **this 的优先级**   
`new`绑定  >  显式绑定  >  隐式绑定  >  默认绑定

如果要判断一个运行中的函数的this绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断this的绑定对象。

1. 由new调用？绑定到新创建的对象。
2. 由call或者apply（或者bind）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到undefined，否则绑定到全局对象。

***
**Reference**

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this
- http://web.jobbole.com/85198/
- https://github.com/creeperyang/blog/issues/16
- https://github.com/SimonZhangITer/MyBlog/issues/12

