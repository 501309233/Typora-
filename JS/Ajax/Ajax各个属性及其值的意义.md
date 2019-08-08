[TOC]



##url

要求为String类型的参数，（默认为当前页地址）发送请求的地址。就是在此发送需求到对应的后台去处理，后台根据这个url来区别不同的请求。

## type

要求为String类型的参数，请求方式（post或get）默认为get。注意其他http请求方法，例如put和delete也可以使用，但仅部分浏览器支持。

###post

浏览器把各表单字段元素及其数据作为HTTP消息的实体内容发送给Web服务器，数据量要比使用GET方式传送的数据量大的多，安全

###get

get方式可传送简单数据，有大小限制，数据追加到url中发送（http的header传送）,url可以被客户端缓存，从浏览器的历史记录中得到客户数据，不安全。

##timeout

要求为Number类型的参数，设置请求超时时间（毫秒）。此设置将覆盖$.ajaxSetup()方法的全局设置

##async

要求为Boolean类型的参数，默认设置为true，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为false。注意，同步请求将锁住浏览器，用户其他操作必须等待请求完成才可以执行。

这里就会经常发生问题，就是异步了，所以有时候你会不小心，在请求还没成功，你就以为你成功了，然后再干其他事，就容易出问题，所以，一般要在success回调函数里面做一些事。

##cache

要求为Boolean类型的参数，默认为true（当dataType为script时，默认为false），设置为false将不会从浏览器缓存中加载请求信息。

##data

发送到服务器的数据，要求为Object或String类型的参数。如果已经不是字符串，将自动转换为字符串格式。get请求中将附加在url后。防止这种自动转换，可以查看　　processData选项。对象必须为key/value格式，例如{foo1:"bar1",foo2:"bar2"}转换为&foo1=bar1&foo2=bar2。如果是数组，JQuery将自动为不同值对应同一个名称。例如{foo:["bar1","bar2"]}转换为&foo=bar1&foo=bar2。

上面的例子中，就是把一个javascript对象给变成json然后传到后台去处理

##dataType

要求为String类型的参数，预期服务器返回的数据类型。如果不指定，JQuery将自动根据http包mime信息返回responseXML或responseText，并作为回调函数参数传递。可用的类型如下

### xml

返回XML文档，可用JQuery处理。

### html

返回纯文本HTML信息；包含的script标签会在插入DOM时执行。

### script

返回纯文本JavaScript代码。不会自动缓存结果。除非设置了cache参数。注意在远程请求时（不在同一个域下），所有post请求都将转为get请求

### json

返回JSON数据。起码我见到都是返回json类型。其他的没见用过。后台可以处理完之后返回一个bean的对象，然后将对象转换成json字符串形式的对象，就跟之最上面的例子中的stream对象一样，可以方便的操作各个属性，然后在前台操作的时候就灰常的方便。。。一句话概括：如果指定为json类型，则会把获取到的数据作为一个JavaScript对象来解析，并且把构建好的对象作为结果返回

### jsonp

JSONP格式。使用SONP形式调用函数时，例如myurl?callback=?，JQuery将自动替换后一个“?”为正确的函数名，以执行回调函数

### text

返回纯文本字符串

## beforeSend

要求为Function类型的参数，发送请求前可以修改XMLHttpRequest对象的函数，例如添加自定义HTTP头。在beforeSend中如果返回false可以取消本次ajax请求。XMLHttpRequest对象是惟一的参数。

​            function(XMLHttpRequest){

​               this;   //调用本次ajax请求时传递的options参数

​            }

## complete

要求为Function类型的参数，请求完成后调用的回调函数（请求成功或失败时均调用）。参数：XMLHttpRequest对象和一个描述成功请求类型的字符串。
          function(XMLHttpRequest, textStatus){
             this;    //调用本次ajax请求时传递的options参数
          }

## success

要求为Function类型的参数，请求成功后调用的回调函数，有两个参数。

​         (1)由服务器返回，并根据dataType参数进行处理后的数据。

​         (2)描述状态的字符串。

​         function(data, textStatus){

​            //data可能是xmlDoc、jsonObj、html、text等等

​            this;  //调用本次ajax请求时传递的options参数

​         }

例子中的data就是后台处理之后，返回的一个javascript对象，里面包含前台需要的各种信息，需要什么塞什么。

一般都是只用第一个参数，第二个基本没见过。

这个才是灰常常用的一个参数。

## error

要求为Function类型的参数，请求失败时被调用的函数。该函数有3个参数，即XMLHttpRequest对象、错误信息、捕获的错误对象(可选)。ajax事件函数如下：
       function(XMLHttpRequest, textStatus, errorThrown){
          //通常情况下textStatus和errorThrown只有其中一个包含信息
          this;   //调用本次ajax请求时传递的options参数
       }

## contentType

要求为String类型的参数，当发送信息至服务器时，内容编码类型默认为"application/x-www-form-urlencoded"。该默认值适合大多数应用场合。

multipart/form-data：有时候也会这个，上传下载可能会用到。

contentType: "application/json; charset=utf-8",

我想现在json这么简单易用，估计一般这个类型也很常用吧。

## dataFilter

要求为Function类型的参数，给Ajax返回的原始数据进行预处理的函数。提供data和type两个参数。data是Ajax返回的原始数据，type是调用jQuery.ajax时提供的dataType参数。函数返回的值将由jQuery进一步处理。
            function(data, type){
                //返回处理后的数据
                return data;
            }

## global

要求为Boolean类型的参数，默认为true。表示是否触发全局ajax事件。设置为false将不会触发全局ajax事件，ajaxStart或ajaxStop可用于控制各种ajax事件

## ifModified

要求为Boolean类型的参数，默认为false。仅在服务器数据改变时获取新数据。服务器数据改变判断的依据是Last-Modified头信息。默认值是false，即忽略头信息

## jsonp

要求为String类型的参数，在一个jsonp请求中重写回调函数的名字。该值用来替代在"callback=?"这种GET或POST请求中URL参数里的"callback"部分，例如{jsonp:'onJsonPLoad'}会导致将"onJsonPLoad=?"传给服务器。

## username

要求为String类型的参数，用于响应HTTP访问认证请求的用户名。

## password

要求为String类型的参数，用于响应HTTP访问认证请求的密码。

## processData

要求为Boolean类型的参数，默认为true。默认情况下，发送的数据将被转换为对象（从技术角度来讲并非字符串）以配合默认内容类型"application/x-www-form-urlencoded"。如果要发送DOM树信息或者其他不希望转换的信息，请设置为false。

## scriptCharset

要求为String类型的参数，只有当请求时dataType为"jsonp"或者"script"，并且type是GET时才会用于强制修改字符集(charset)。通常在本地和远程的内容编码不同时使用