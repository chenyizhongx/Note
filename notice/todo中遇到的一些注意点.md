#### 一 [HTML]显示/隐藏DIV的技巧(visibility与display的差别)

`div`的`visibility`可以控制`div`的显示和隐藏，但是隐藏后页面显示空白:

    style="visibility: none;"
    //  隐藏
    document.getElementById("typediv1").style.visibility="hidden";
    // 显示
    document.getElementById("typediv1").style.visibility="visible";
通过设置`display`属性可以使div隐藏后**释放占用的页面空间**，如下

    style="display: none;"    
    //隐藏
    document.getElementById("typediv1").style.display="none";
    // 显示    
    document.getElementById("typediv1").style.display="";     

#### 二 如何根据父级元素进行相对定位
**注意相对定位的概念**

`position:relative`是指元素相对于在文档流中位置进行移动的，而**不是以父元素为基准**进行移动的。

如何解决？ 
方法是：**将父元素的position设为relative，将子元素的定位方法设为absolute.**
解释：当元素的position设为absolute时，如果父级元素没有设置position属性，则元素的基准点就为浏览器的左上角，而当父元素设置了position属性时，那么元素将以父元素为基准点。


注意当父元素设置为了relative之后，子元素相对定位时将会顾及到padding的值。本质上，padding的设定实际上是将元素的边界外延。因此，如果父元素设定了padding之后，子元素相对父元素进行定位时很容易跑偏。如何解决呢？

我们看到父元素既然已经设定了relative，relative就是相对于原来位置进行移动的，那自然可以使用left和top等属性来调整父元素的位置，但是父元素的边界并没有变化。

#### 三 实现内联滚动条（前端底部固定方法 ）
https://www.jianshu.com/p/f49d5b6475a9
#### 四 DOM的增删查改
[Javascript进阶篇——(DOM—节点---插入、删除和替换元素、创建元素、创建文本节点)](https://www.cnblogs.com/alice-shan/p/5287652.html)    

 [javascript获得和设置以及移除元素属性的三个方法](https://blog.csdn.net/cangkukuaimanle/article/details/7674180)

#### 五 CSS的一些文本属性
要在文字中间实现有一条[横线](https://www.ylefu.com/tags/211/ "横线")的效果可以使用CSS中CSS text-decoration 属性就可以达到效果。

CSS text-decoration 属性的值：

- none    默认。定义标准的文本。
- underline    定义文本下的一条线。 
- overline    定义文本上的一条线。
- line-through    定义穿过文本下的一条线。
- blink    定义闪烁的文本。
- inherit    规定应该从父元素继承 text-decoration 属性的值。

#### 六 JS生成 img 标签的三种方式

    <div id="d1"></div>
    <script>
    //HTML
    function a(){
        document.getElementById("d1").innerHTML="<img     src='http://baike.baidu.com/cms/rc/240x112dierzhou.jpg'>";
    }
    a();

    //方法
    function b(){
      var d1=document.getElementById("d1");
      var img=document.createElement("img");
      img.src="http://baike.baidu.com/cms/rc/240x112dierzhou.jpg";
      d1.appendChild(img);
    }
      b();

    //对象
    function c(){
      var cc=new Image();
      cc.src="http://baike.baidu.com/cms/rc/240x112dierzhou.jpg";
      document.getElementById("d1").appendChild(cc);
    }
    c();
    </script>
#### 七
    // 在单个语句中设置多个样式
    elt.style.cssText = "color: blue; border: 1px solid black"; 

#### 八 事件委托
[js中的事件委托或者事件代理详解](https://www.cnblogs.com/liugang-vip/p/5616484.html)





