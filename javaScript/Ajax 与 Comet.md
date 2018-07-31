## 一 XMLHttpRequest 对象
创建XHR 对象

    var xhr = new XMLHttpRequest();
#### 1) XHR 的用法

1. open()

        xhr.open("get", "example.php", false);
        xhr.send(null);
三个参数，要发送的请求的类型，请求的URL和表示是否异步发送请求的布尔值。

send接收一个参数，即要作为请求主体发送的数据，如果不需要，则必须传入null.

- responseText : 作为响应主体被返回的文本
- responseXML: 如果响应的内容是 .xml 类型，这个属性中就会保存着响应数据的XML　DOM　文档。
- status： 响应的HTTP状态
- statusText: HTTP状态的说明。

接到响应的第一步是检查 Status 属性。
上面是发送同步请求。

要发送异步请求，可以检测XHR对象的readyState 属性，该属性表示当前的活动状态。

- 0： 未初始化
- 1： 启动
- 2：发送
- 3：接收
- 4：完成

#### 2) HTTP 头部信息
XHR对象提供了操作**请求头部**和**响应头部**的方法。

使用setRequestHeader() 方法可以设置自定义的请求头部信息。
要想成功发送头部信息，必须在调用open() 方法后调用send() 方法之前调用setRequestHeader() 

调用 XHR 对象的 getResponseHeader() 方法并传入头部字段名称，可以取得相应的响应头部信息。

而调用 getAllResponseHeaders() 方法可以取得一个包含所有的头部信息的长字符串。
 
#### 3）GET请求
用于向服务器查询某些信息
必要时，可以将查询字符串追加到URL 的末尾，以便将信息发送给服务器。

    ///请求
    function addURLParam(url, name, value) {
      url += (url.indexOf("?") == -1 ? "?" : "&");
      url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
      return url;
    }

#### 4） POST 请求
用于向服务器发送应该被保存的数据
初始化 POST 请求

    xhr.open("post", "example.php", true);
表单数据要序列化
POST 数据的格式与查询字符串的格式相同，如果需要将表单中的数据序列化，然后再通过XHR发送到服务器，可以使用 serialize() 函数来创建这个字符串。

##  二 XMLHttpRequest 2 级
#### 1）FormData 
FormData为序列化表单以及创建与表单格式相同的数据（通过XHR传输） 提供了便利。

    var data = new FormData();
    data.append("name", "zhong");
创建了FromData的实例后，可以将它直接传给 XHR　的send()方法。

    xhr.open("post", "postexample.php", true);
    var form = document.getElementById("user-info");
    xhr.send(new FormData(form));

#### 2) 超时设定
IE8 为XHR对象添加了一个　timeout 属性，表示请求在等待响应了多少毫秒后就终止。

    //超时设定
    xhr.timeout = 1000;
    xhr.ontimeout = function() {
      alert("...");
    }
IE8+ 是唯一支持超时设定的浏览器。
#### 3）overrideMimeType () 方法

用于重写XHR响应的MIME类型。

    var xhr = createXHR();
    xhr.open("get", "text.php", true);
    xhr.overrideMimeType("text/xml");
    xhr.send(null);

## 三　进度事件
#### 1) load 事件
用于替代  readystatechange 事件。
#### 2) progress 事件
这个事件会在浏览器接收新数据期间周期性的触发。

    //进度指示器
    xhr.onprogress = function(event) {
      var divStatus = document.getElementById("status");
      if(event.lengthComputable) {
        divStatus.innerHTML = "Received " + event.position + " of " + event.totalSize + " bytes";
      }
    };

## 跨域源资源共享
CORS　基本思想就是通过使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。

#### １）IE 对 CORS的实现
IE8 中引入了XDR 类型，这个对象与XHR类似，但能够实现安全可靠的跨域通信。
属性也基本相同

    //IE对CORS的实现
    var xdr = new XDomainRequest();
    xdr.onload = function() {
      alert(xdr.responseText);
    };
    //检测错误
    xdr.onerror = function() {
      alert("An error occurred.");
    };
    
    //超时
    xdr.timeout = 1000;
    xdr.ontimeout = function() {
      alert("Request took too long.");
    }
    
    xdr.open("get", "http://www.somewhere-else.com/page/");
    //表示发送数据的格式
    xdr.contentType = "application/x-www-form-urlenccoded";   //影响头部信息的唯一方式
    xdr.send("name1=value1&name2=value2");
    xdr.send(null);
#### 2）其它浏览器对CORS的实现

    //除IE外的浏览器对CORS的实现
    var xhr = createXHR();
    xhr.onreadystatechange = function() {
      if(xhr.readyState == 4) {
        if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
          alert(xhr.responseText);
        } else {
          alert("Request was uncessful: " + xhr.status);
        }
      }
    };
    xhr.open("get", "http://www.somewhere-else.com/page/", true);
    xhr.send(null);
#### 3）Comet 
Ajax是一种从页面向服务器请求数据的技术，而Comet 则是一种服务器向页面推送数据的技术。
Comet 能够让信息近乎实时地被推送到页面上。

实现Comet 的方式：

- 长轮询：浏览器定时向页面推送数据的技术。
- 流：即HTTP流，浏览器向服务器发送一个请求，而服务器保持连接打开。

使用 XHR 对象实现HTTP流的典型代码

    function createStreamingClient(url, progress, finished) {
    
      var xhr = new XMLHttpRequest();
        received = 0;
    
      xhr.open("get", url, true);
      xhr.onreadystatechange = function() {
        var result;
    
        if(xhr.readyState == 3) {
    
          //只取得最 新的数据并调整计数器
          result = xhr.responseText.substring(received);
          received += result.length;
    
          //调用progress 回调函数
          progress(result);
        } else if(xhr.readyState == 4) {
          finished(xhr.responseText);
        }
    
      };
      xhr.send(null);
      return xhr;
    }
    
    var client = createStreamingClient("streaming.php", function(data) {
      alert("Received: " + data);
    }, function(data) {
      alert("Done!");
    });

#### 4）服务器发送事件
1. SSE API
2. 事件流

#### 5）Web Sockets

在一个单独的持久连接上提供全双工，双向通信。


