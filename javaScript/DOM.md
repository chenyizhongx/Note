##  一 节点层次
### 1）**Node 类型**
DOM1级定义了一个`Node`接口。JavaScript中的所有节点类型都继承自`Node`类型。所有的节点类型都共享着相同的基本属性和方法。

每个节点都有一个 `nodeType`属性，用于表明节点的类型。节点类型由在 `Node`类型下定义的12个数值常量来表示，任何节点类型必居其一：

- Node.ELEMENT_NODE(1)　　　　　　　　　　代表element元素节点
- Node.ATTRIBUTE_NODE(2) 　　　　　　　　　attr类型
- Node.TEXT_NODE(3) 　　　　　　　　　　　　text节点 文本节点
- Node.CDATA_SECTION_NODE(4)
- Node.ENTITY_REFERENCE_NODE(5)
- Node.ENTITY_NODE(6)
- Node.PROCESSING_INSTRUCTION_NODE(7)
- Node.COMMENT_NODE(8) 　　　　　　　　　　注释节点
- Node.DOCUMENT_NODE(9)　　　　　　　　　　表示文档类型
- Node.DOCUMENT_TYPE_NODE(10) // doctype 类型
- Node.DOCUMENT_FRAGMENT_NODE(11)
- Node.NOTATION_NODE(12)

通过上面的属性，可以直接确定节点的属性

    if (someNode.nodeType == 1) {
        alert("Node is an element.");    
    }

##### 1.nodeName 和 nodeValue 属性
要了解元素节点，可以使用nodeName 和 nodeValue 属性。对于元素节点，nodeName 中保存的始终都是元素的标签名，而 nodeValue 的值则始终是 null.
##### 2. 节点关系
每个节点都有一个 childNodes 属性，其中保存着一个NodeList 对象。NodeList 对象也有length属性，但它不是 Array的实例。NodeList 对象是DOM结构动态执行查询的结果。

![](https://upload-images.jianshu.io/upload_images/11428632-b42dd53802f1b065.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
hasChildNodes()方法可以在节点包含一个或者多个节点的情况下返回 true.
##### 3. 操作节点

    //DOM7
    var returnedNode = someNode.appendChild(newNode);
    alert(returnedNode == newNode);    //true
    alert(someNode.lastChild == newNode);  //true
    
    //有多个子节点
    var returnedNode = someNode.appendChild(someNode.firstChild);
    alert(returnedNode == someNode.firstChild);   //false
    alert(returnedNode == someNode.lastChild);   // true
    
    //插入后成为最后一个子节点
    returnedNode = someNode.inserBefore(newNode, null);
    alert(newNode == someNode.lastChild);  //true;
    
    //插入后成为第一个子节点
    var returnedNode = someNode.inserBefore(newNode, someNode.firstChild);
    alert(returnedNode == newNode);   // true
    alert(returnedNode == someNode.firstChild);   //true
    
    //插入到最后一个子节点前面
    returnedNode = someNode.inserBefore(newNode, someNode.lastChild);
    alert(newNode == someNode.childNodes[someNode.childNodes.length-2]);
    
    //替换第一个子节点
    var returnedNode = someNode.replaceChild(newNode,someNode.firstChild);
    
    //替换最后一个子节点
    returnedNode = someNode.replaceChild(newNode, someNode,lastChild);

    // 移除第一个子节点
    var returnedNode = someNode.removeChild(newNode, someNode.firstChild);

    //替换最后一个子节点
    returnedNode = someNode.removeChild(newNode, someNode.lastChild);

removeChild() 和 replaceChild() 移除的节点仍然为文档所有，只不过在文档中已经没有了自己的位置。

##### 4. 其他方法
cloneNode() 复制节点
normalize() 处理文本节点，删除空文本节点或合并相邻的文本节点。

### 2）**Document 类型**

document对象是window对象的一个属性。也是HTMLDocument的一个实例

#####1. 文档中的子节点
访问子节点的快捷方式：
- documentElement() 属性
- 通过childNodes 列表访问文档元素

第一个子节点就是 <html> 元素
document 对象还有一个body属性，直接指向 <body> 元素

`var body = document.body;  // 取得对body的引用`  
`var doctype = document.body;   // 取得对<!DOCTYPE>的引用`
##### 2. 文档信息
**属性：title** ，包含着<title> 元素中的文本——显示在浏览器的标题栏或标签页上。，通过这个属性可以取得或修改当前页面的标题
还有三个与对网页的请求有关的属性，分别是**URL， domain 和 referrer**

URL  包含完整的地址URL，domain 属性只包含页面的域名，referrer属性中保存着链接到当前页面的那个页面的URL，如果没有，那么该属性可能会包含一个空字符串。

三个属性中只有domain是可以设置的。将每个页面的值设置为同一个domain 可以实现 JavaScript对象的跨域通信。
##### 3. 查找元素

**getElementById()**
这个方法将放回一个与那个有着给定id属性值的元素节点对应的对象。参数为标签的名字。

**getElementByTagName()**
getElementByTagName 方法返回一个对象数组，它的参数是标签的名字。

**getElementsByName()**
会返回一个带有name 特性的所有元素。常用的是通过该方法取得单选按钮

**namedItem()**
这个方法可以通过元素中的name特性来取得集合中的项。

##### 4. 特殊集合
这些集合都是HTMLCollection对象。
document.links 等
##### 5. DOM 一致性检测
由于DOM包含多个级别，多个部分，因此需要检测浏览器实现了哪些部分。
`document.implementation` 属性就是为此提供相应的信息和功能的对象。
其中规定了一个hasFeature() 方法   

`document.implementation.hasFeature("XML","1.0")   // true `  

不过最好要同时使用能力检测。

##### 6. 文档写入
就是将输入流写入到网页的能力。总共有四个方法：

- writer()    接受字符串参数，并写入
- writeln()   接受字符串参数，添加换行符，并写入
- open()   打开网页的输入流
- close()   关闭网页的输入流

### 3）**Element 类型**
Element 类型用于表现XML或NTML元素，提供了对元素标签名，子节点以及特性的访问
##### 1. HTML元素
每个HTML元素中的标准特性。

- id   唯一标识符
- title  有关元素的附加说明信息
- lang  语言代码
- dir   语言的方向 从左到右（ltr）
- className  与元素的class特性对应。
##### 2. 特性
主要使用三种方法

- **getAttribute()**  
`getAttribute`是一个函数，只有一个参数——你打算查询的属性的名字。`getAttribute`方法不属于`document`对象，所以不能通过`document`对象调用，它*只能通过元素节点对象调用*
- **setAttribute()** 
`setAttribute()`方法允许我们对某些节点的属性做出更改，和`getAttribute`一样，`setAttribute`只能用于元素节点。
- **removeAttribute()**      
彻底删除元素的特性，调用这个方法不仅会清除特性的值，而且也会从元素中完全删除元素的特性。
##### 3. attributes 属性
Element 类型是使用`attributes `属性的唯一一个DOM节点类型，元素中的每一个特性都由一个`Attr `节点来表示，每个节点都保存在`NameNodeMap`对象中。`NamedNodeMap`对象拥有下列方法：

- getNamedItem(name) : 返回`nodeName` 属性中等于` name` 的节点
- removeNamedItem(name) : 从列表中移除 `nodeName `属性等于 `name` 的节点；
- setNamedItem(node): 向列表中添加节点，以节点`nodeName `属性为索引；
- item(pos): 返回位于数字`pos`位置处的节点。
##### 4. 创建元素
 使用 `document.createElement("div") `元素可以创建新元素，一旦将元素添加到文档树中，浏览器就会立即呈现该元素。
##### 5. 元素的子节点

元素的childNodes属性中包含它的所有子节点

### 4）**Text 类型**
文本节点由Text类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的HTML字符，但不能包含HTML代码。
##### 1）创建文本节点
可以使用 document.createTextNode() 创建新的文本节点  

`var textNode = document.createTextNode( "Some <strong>other</strong> world!");`   

如果同时有两个相邻的文本子节点，那么这两个节点中的文本就会连起来显示，中间不会有空格。
##### 2）规范化文本节点
能够将相邻文本节点合并的方法 **normalize()**
##### 3)  分割文本节点
**splitText()** 该方法可以将一个文本节点分割成两个文本节点。该方法会返回一个新文本节点，新的节点包含剩下的部分。
分隔文本节点是从文本节点中提取数据的一种常用的DOM解析技术。
### 5）**Comment 类型**
使用documen.createComment () 可以传递注释文本也可以创建注释节点，
该类型很少使用。
### 6）**CDATASection 类型**
CDATASection 类型只针对基于XML的文档，有与comment类型类似的注释功能。
### 7) **DocumentType 类型**
不常用，DocumentType中包含着与文档的doctype有关的所有信息。
### 8)  **DocumentFragment 类型**
DocumentFragment 在文档中没有相应的标记，DOM规定文档片段是一种轻量级的文档，可以包含和控制节点。
用于一次性批量打包添加节点，防止导致浏览器反复渲染。
### 9) **Attr** 类型
元素的特性在DOM中以`Attr` 类型来表示
## 二 DOM操作技术
#### 1）动态脚本
即用元素本身包含代码，创建动态脚本有两种方式：插入外部文件和直接插入JS代码。
动态加载的外部JS文件能够立即运行。
#### 2)  动态样式
与动态脚本类似，动态样式的在页面加载完成后动态添加到页面中的。
#### 3）操作表格
#### 4）使用NodeList