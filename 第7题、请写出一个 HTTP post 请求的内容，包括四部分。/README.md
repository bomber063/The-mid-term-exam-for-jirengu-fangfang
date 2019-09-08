# 题目请写出一个 HTTP post 请求的内容，包括四部分。其中
* 第四部分的内容是 username=ff&password=123
* 第二部分必须含有 Content-Type 字段
* 请求的路径为 /path

# 根据题目要求写出如下
POST /path HTTP/1.1  
Host: baidu.com  
Accept: application/json  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 24  
     
username=ff&password=123 

# 其他
* MDN上关于HTTP 请求方法的介绍[链接](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)  
* 自己写的HTTP请求和响应介绍博客[链接](https://zhuanlan.zhihu.com/p/51189096)  
* 自己写HTTP 入门博客[链接](https://zhuanlan.zhihu.com/p/56348309)
