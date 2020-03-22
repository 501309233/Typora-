爬虫

[toc]

# 实战

## 找数据格式信息

在网页上的Network中的Post请求中找到数据格式和URL



```python
import urllib.request
import urllib.parse
import json

url = 'http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule'
data = {}
data['i'] = input("请输入需要翻译什么内容：")
data['from'] = 'AUTO'
data['to'] = 'AUTO'
data['smartresult'] = 'dict'
data['client'] = 'fanyideskweb'
data['salt'] = '15846683313101'
data['sign'] = '4c1624efc051aa2bbb2ae77a13966d3a'
data['ts'] = '1584668331310'
data['bv'] = '85d772a06741ae9950b2a7b061f7e4a4'
data['doctype'] = 'json'
data['version'] = '2.1'
data['keyfrom'] = 'fanyi.web'
data['action'] = 'FY_BY_CLICKBUTTION'
#用utf-8来编码
data = urllib.parse.urlencode(data).encode('utf-8')


request = urllib.request.Request(url,data)

request.add_header('User-Agent',
                    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36')

#用response来接收返回的结果
response = urllib.request.urlopen(request)
#用utf-8来解码
html = response.read().decode('utf-8')
#用json的loads方法来家在返回数据
result = json.loads(html)
result = result['translateResult'][0][0]['tgt']
print(result)

```





#隐藏

## 修改header

* 通过Request的header参数修改

* 通过Request.add_header()方法修改

* ```python
  request.add_header('User-Agent',
                      'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36')
  ```

  

## 延迟时间

```python
import time
#延迟3秒
time.sleep(3)
```



## 代理

* 参数上一个字典{'类型':'代理ip:端口号'}

Proxy_support = urllib.request.kProxyHandler({})

* 定制、创建一个opener

Opener = urllib.request.build_opener(proxy_support)

* 安装opener

urllib.request.install_opener(opener)

* 调用opener

opener.open(url)

