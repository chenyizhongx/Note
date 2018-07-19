
BOM(BOM Browser Object Model) 即浏览器对象模型。
ECMAScript 是JavaScript的核心，但如果要在Web中使用JAvascript，那么BOM则无疑是真正的核心。
浏览器对象模型的图解如下：
![](https://upload-images.jianshu.io/upload_images/11428632-6e1daee2df0d6bb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

##  一  window 对象
`BOM` 的核心对象是`window`，它表示浏览器的一个实例。在浏览器中，`window` 对象有双重角色，它既是通过`JavaScript` 访问浏览器窗口的一个接口，又是`ECMAScript` 规定的Global 对象。    
这意味着在网页中定义的任何一个对象、变量和函数，都以`window` 作为其`Global `对象。
#### 1）全局作用域
由于window对象同时扮演着ECMAScript中Global对象的角色，因此所有的全局作用域中声明的变量，函数都会变成window对象的属性和方法。

全局变量不能通过delete 操作符删除，而直接在window 对象上的定义的属性可以  

    var age = 29;
    window.color = "red";
    //在IE < 9 时抛出错误，在其他所有浏览器中都返回false
    delete window.age;
    //在IE < 9 时抛出错误，在其他所有浏览器中都返回true
    delete window.color; //returns true
    alert(window.age); //29
    alert(window.color); //undefined
尝试访问未声明的变量会抛出错误，但是通过查询window 对象，可以知道某个可能未声明的变量是否存在

    //这里会抛出错误，因为oldValue 未定义
    var newValue = oldValue;
    //这里不会抛出错误，因为这是一次属性查询
    //newValue 的值是undefined
    var newValue = window.oldValue;

#### 2）窗口关系及框架
如果页面中包含框架，则每个框架都拥有自己的`window `对象，并且保存在`frames `集合中。在`frames`集合中，可以通过数值索引（从0 开始，从左至右，从上到下）或者框架名称来访问相应的`window `对象。每个`window `对象都有一个`name `属性，其中包含框架的名称。

    <html>
        <head>
            <title>Frameset Example</title>
            <meta charset="UTF-8">
        </head>
    <frameset rows="160,*">
        <frame src="frames/frame1.html" name="topFrame">
        <frameset cols="50%,50%">
            <frame src="frames/frame2.html" name="leftFrame">
            <frame src="frames/frame3.html" name="rightFrame">
        </frameset>
    </frameset>
    </html>
这个框架中一个框架集包含了另一个框架集
`self`对象始终指向`window`;实际上，`self`和`window`可以相互交换使用。
在使用框架的情况下，浏览器中会存在多个Global对象，每个框架中定义的全局变量会自动成为框架中`window`对象的属性。由于每个`window`对象都包含原生类型的构造函数，因此每个对象都有一套自己的构造函数，这些构造函数一一对应，但并不相等。
#### 4）窗口位置
使用下列代码可以跨浏览器取得窗口左边和上边的位置。

    var leftPos = (typeof window.screenLeft == "number") ?
    window.screenLeft : window.screenX;
    var topPos = (typeof window.screenTop == "number") ?
    window.screenTop : window.screenY;
#### 5）窗口大小
- `document.documentElement.clientWidth`与`document.documentElement.clientHeight`：获得的是屏幕可视区域的宽高，不包括滚动条与工具条
- `window.innerWidth`与`window.innerHeight`：获得的是可视区域的宽高，但是`window.innerWidth`宽度包含了纵向滚动条的宽度，`window.innerHeight`高度包含了横向滚动条的高度(IE8以及低版本浏览器不支持)。
- `window.outerWidth`与`window.outerHeight`：获得的是加上工具条与滚动条窗口的宽度与高度。
- `document.body.clientWidth`与`document.body.clientHeight`：`document.body.clientWidth`获得的也是可视区域的宽度，但是`document.body.clientHeight`获得的是body内容的高度，如果内容只有200px，那么这个高度也是200px
#### 6) 导航和打开窗口
使用`window.open() `方法可以导航到一个特定的URL。
`window.close()` 方法可以关闭心打开的窗口。
新建的open 对象有一个`opener`属性，其中保存着打开它的原始窗口对象。

    wroxWin.opener = null;
将`opener`属性设置为`null`,就是告诉浏览器新创建的标签页不需要与打开它的标签页通信。

**屏蔽浏览器窗口**       
 
    var wroxWin = window.open("https://www.google.com", "_blank");
    if(wroxWin == null) {
      alert("The popup was blocked");
    } 
也可以用`try-catch` 块

    var blocked = false;

    try {
      var wroxWin = window.open("https://www.google.com", "_blank");
      if (wroxWin == null){
        blocked = true;
      }
    } catch (ex) {
      blocked = true;
    }

    if (blocked) {
      alert("The popup was blocked!");
    }
#### 7) 间歇调用和超时调用
JS是单线程语言，但它允许通过设置超时值（指定的时间过后执行代码）和间歇时间值（每隔指定的时间就执行一次代码）来调度代码在特定的时刻执行。
超时调用有`setTimeout()` 方法

    var timeoutId = setTimeout(function(){
      alert("Hello world!");
    },4000);

    clearTimeout(timeoutId);

设置间歇调用的方法是`setInterval() ` 同样会返回一个间歇调用ID，该ID会在将来某个时刻取消间歇调用。
一般认为，**使用超时调用来模拟间歇调用**是一种最佳方式。在开发环境下，很少使用间歇调用，原因时后一个间歇调用可能在前一个间歇调用结束之前启动。

    var count = 0; 
    function doSoming(){
        count ++;
        if(count<=10){
           setTimeout(doSoming,1000);    
        }
    }
    setTimeout(doSoming,1000);
#### 8）系统对话框
浏览器通过 `alert() `，`confirm() `和 `prompt() `方法可以调用系统对话框向用户显示信息。

`confirm() `和`alert() `的区别是多了一个取消按钮。

    if (confirm("Are you sure?")) {
      alert("I'am sure you're sure");
    } else {
      alert("I'am sorry to hear that you're not sure");
    }
    
    prompt("What's your name?");
最后一种对话框是通过调用`prompt() `方法生成的，这是一个提示框，用于提示用户输入一些文本。输入框中除了显示OK和Cancel按钮外，还会显示一个文本输入域，以供用户在其中输入内容。
## 二 location 对象　　
location 是最有用的BOM对象之一，它提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。
下面是location对象的属性列表.
hash 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串,例"#contents"

- `host `返回服务器名称和端口号(如果有).   例 "www.zhong.com:80"

- `hostname` 返回不带端口号的服务器名称. 例 "www.zhong.com"

- `href ` 返回当前页面的完整url. 例 "www.github.com/index.html"

- `pathname` 返回url中的目录或文件名, 例"/zhong/github"

- `port `返回url的端口号,如果没有则返回空字符串 .例"8080"

- ` protocol `返回页面使用的协议。 通常是http:或https:

- `search `返回URL的查询字符串。 这个字符串以问号开头,   '?id=100'

#### 1) 查询字符串参数
通过URL逐个访问其中包含的查询字符串属性。

    function getQueryStringArgs() {
    
      //取得查询字符串并去掉开头的问号
      var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
    
      //保存的数据
      args = {},
    
      //取得每一项
      items = qs.length ? qs.split("&") : [],
      item = null,
      name = null,
      value = null,
    
      //在for循环中使用
      i = 0,
      len = items.length;
    
      //逐个将每一项添加到args对象中
      for (i = 0; i < len; i++) {
        item = items[i].split("=");
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);
    
        if (name.length) {
          args[name] = value;
        }
      }
    
      return args;
    #### 2）位置操作
通过以下任何一种方式都会导致页面的跳转或重载

- location.href = "http://www.baidu.com";
- location.hash = "#section1";
- location.search = "?q=javascript";
- location.hostname = "www.yahoo.com";
- location.pathname = "mydir";
- location.port = 8080;

通过reload可以重新加载当前显示的页面。

      location.reload();     //重新加载(有可能从缓存中加载)
      location.reload(true);    //重新加载(直接从服务器加载)
## 三 navigator 对象

是识别客户端浏览器的事实标准。navigator 对象的属性通常用于检测网页的浏览器类型。
#### 1）检测插件
对于非IE浏览器，可以使用plugins数组来达到这个目的，该数组中的每一项都包含下列属性。

- name :插件的名字
- description：插件的描述
- filename:  插件的文件名
- length： 插件所处理的MIME类型的数量



  	    ///检测插件(在IE中无效)
        function hasPlugin(name) {
  	    name = name.toLowerCase();
   	 	for (var i = 0; i < navigator.plugins.length; i++) {
  	      if(navigator.plugins[i].name.toLowerCase().indexOf(name) > -1) {
 	          return true;
	        }
	      }
		    
 	     return false;
  	    }
  	  
 	    //检测flash
 	    alert(hasPlugin("Flash"));
    
  	    //检测QUickTime
  	    alert(hasPlugin("QuickTime"));

下面的函数可以用来检测IE中是否安装相应的插件。

    //检测IE中的插件
    function hasIEPlugin(name) {
      try {
        new ActiveXObject(name);
        return true;
      } catch(ex) {
        return false;
      }
    }
    
    //检测Flash
    alert(hasIEPlugin("ShockwaveFlash.ShockwaveFlash"));
    
    //检测QuickTime
    alert(hasIEPlugin("QuickTime.QuickTime"));
也可以针对每个插件分别创建检测函数，而不是使用前面提到过的通用的检测方法。

    //检测所有浏览器的Flash
    function hasFlash() {
      var result = hasPlugin("Flash");
      if (!result) {
        result = hasIEPlugin("ShockwaveFlash.ShockwaveFlash");
      }
      return result;
    }
    
    //检测所有浏览器中的QuickTime
    function hasQuickTime() {
      var result = hasPlugin("QuickTime");
      if (!result) {
        result = hasIEPlugin("QuickTime.QuickTime");
      }
      return result;
    }
    
    //检测Flash
    alert(hasFlash());
    
    //检测QuickTime
    alert(hasQuickTime());

## 四 screen 对象

 screen 对象基本上只用来表明客户端的能力，其中包括浏览器窗口外部的显示器的信息，如像素宽度和高度等。 

## 五 history 对象

history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起，因为history是window对象的属性，因此每个浏览器窗口，每个标签页乃至每个框架，都有自己的history对象与特定的window对象相关联。

    //后退一页
    history.go(-1);

    //前进一页
    history.go(1);

    //前进两页
    history.go(2);

    //跳转到最近的包含'wrox.com'字符的页面
    history.go("wrox.com");

    //后退一页
    history.back();
    //前进一页
    history.forward();

    if (history.length == 0){
    //这应该是用户打开窗口后的第一个页面
    }


