## 一 选择符 API
#### 1）querySelectorAll() 方法
    var selected = document.querySelector(".selected");
    
    var img = document.body.querySelector("img.button");
    
    //取得所有元素
    var selected = document.querySelectorAll(".selected");


    //取得返回的NodeList 中的么一个元素
    var i, len, strong;
    for (i = 0; len = strongs.length; i < len; i++) {
      strong = strong[i];
      strong.className = "important";
    }
#### 2）matchesSelector 方法
    //如果调用的元素和该选择符匹配，返回true
    
    if (document.body.matchesSelector("body.page1")) {
      //true
    }
## 二 元素遍历
    //跨浏览器遍历某元素的所有子元素
    var i, len, child = element.firstElementChild;
    while(child != element.lastElementChild) {
      processChild(child);
      child = child.nextElementSibling;
    }
## 三 HTML5
#### 1）与类有关的扩充
getElementsByClassName()

    //取得所有类中包含 "username" 和 "current" 元素的类名，类名的先后顺序无所谓。
    var allCurrentUsernames = document.getElementByClassName("username current");

#### 2) classList 属性
    //删除"disabled"类
    div.classList.remove("disabled");
    
    
    //添加"current" 类
    div.classList.add("current");
    
    //切换"user" 类
    div.classList.toggle("user");
    
    //确定元素中是否包含既定的类名
    if (div.classList.contains("bd") && !div.classList.contains("disabled")) {
      //执行操作
    }
    
    //迭代类名
    for (var i = 0, len = div.classList.length; i < len; i++) {
      doSomething(div.classList[i]);
    }

####3）自定义数据属性
添加与渲染无关的信息前缀 data- 
#### 4) 插入标记
innerHTML 属性
outerHTML 属性
insertAdjacentHTML() 方法
ScrollIntoView() 方法
## 四 专有扩展
#### 1）children 属性
与childNodes 没有什么区别
#### 2）contains() 方法
检测某个节点是不是另一个节点的后代
#### 3）插入文本
innerText 属性  ：返回文本值
outerText 属性： 替换整个元素
#### 4）滚动
scrollInttoView() 
    