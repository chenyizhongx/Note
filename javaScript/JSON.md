JSON (JavaScript Object Notation ,javaScript 对象表示法)
JSON 是JavaScript 的一个严格的子集，利用了JavaScript中的一些模式来表示结构化数据。
JSON是一种**数据格式，不是一种编程语言**
## 一 语法

- 简单值：使用于JavaScript相同的语法，但不支持undefined
- 对象：无序的键值对
- 数组：有序的值的列表

#### 1）简单值
必须使用双引号
#### 2）对象

    // JavaScript
    var object = {
      name: "zhong",
      age: 22
    };
    //  JSON
    {
      "name": "zhong",
      "age": 22
    }
#### 3）数组
    [22, "fdsa", "dsf"]

## 二 解析和序列化
####1）JSON对象

JSON 是Web服务开发中交换数据的事实标准。
早期的是用JSON解析器即 eval() 函数来解析，解释并返回JS数组和对象。

JSON有两种方法：
- stringify() 用于把JavaScript对象序列化为JSON字符串
- parse() 用于把JSON字符串解析为原生的JavaScript对象

#### 2）序列化选项

JSON.stringify() 还包含两个参数，第一个参数是一个过滤器，可以是一个数组，也可以是一个函数；第二个参数是一个选项，表示是否在JSON字符串中保留缩进。    

**１.过滤结果**

     var  jsonText = JSON.stringify(book, function(){....})
**2.字符串缩进**

      // 缩进四个空格
     var jsonText = JSON.stringify(book, null,  4)

**3.toJSON() 方法**
返回自身的JSON数据格式
#### 3）解析选项
JSON.parse() 方法返回 `undefined` ,表示要从结果中删除相应的键；如果返回其他的值，则将该值插入到结果中。


