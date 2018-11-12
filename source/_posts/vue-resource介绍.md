---
title: VUE(六)：vue-resource HTTP介绍
date: 2018-10-28 16:12:48
categories: "vue" #文章分類目錄 可以省略
comments: true
tags:
- vue #文章標籤 可以省略
- web
---

## HTTP

可以使用全局的 Vue.http 或者在 Vue 实例中的 this.$http 调用 http 服务。

使用
Vue 实例提供了 this.$http 服务可用于发送 HTTP 请求。 A request method call returns a Promise that resolves to the response. Vue 实例在所有回调方法中都会自动绑定到 this 里。
```
{
  // GET /someUrl
  this.$http.get('/someUrl').then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });
}
```
方法
所有的请求类型都可以使用短函数，可以在全局或者 Vue 实例中使用。

// 全局 Vue 对象
```
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```
// Vue 实例
```
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```
短方法清单：
```
get(url, [options])
head(url, [options])
delete(url, [options])
jsonp(url, [options])
post(url, [body], [options])
put(url, [body], [options])
patch(url, [body], [options])
```
选项
```
参数	类型	描述
url	string	请求发送到的 URL
body	Object, FormData,  string	请求中要发送的数据
headers	Object	作为 HTTP 请求头发送的 Headers 对象
params	Object	作为 URL 参数发送的 Parameters 对象
method	string	HTTP 方法 (例如： GET, POST, ...)
timeout	number	请求超时的毫秒数 (0 为不超时)
before	function(request)	在请求发送之前用于修改请求选项的回调函数
progress	function(event)	上传时用于控制 ProgressEvent 的回调函数
credentials	boolean	Indicates whether or not cross-site Access-Control requests should be made using credentials
emulateHTTP	boolean	使用 HTTP POST 发送 PUT, PATCH 和 DELETE 请求并设置 X-HTTP-Method-Override 头
emulateJSON	boolean	以 application/x-www-form-urlencoded 内容类型发送请求报文
```
响应
通过下面的属性和函数将请求解析为响应对象：
```
属性	类型	描述
url	string	响应的源 URL
body	Object, Blob, string	响应的数据报文
headers	Header	响应头对象
ok	boolean	从 200 到 299 的 HTTP 状态码
status	number	响应中的 HTTP 状态码
statusText	string	响应中的 HTTP 状态文本
函数	类型	描述
text()	Promise	作为字符串解析报文
json()	Promise	作为 Json 对象解析报文
blob()	Promise	作为 Blob 对象解析报文
```
实例
```
{
  // POST /someUrl
  this.$http.post('/someUrl', {foo: 'bar'}).then((response) => {

    // get status
    response.status;

    // get status text
    response.statusText;

    // get 'Expires' header
    response.headers.get('Expires');

    // set data on vm
    this.$set('someData', response.body);

  }, (response) => {
    // error callback
  });
}
```
获取图像并使用 blob() 方法来从响应中提取图像正文内容。
```
{
  // GET /image.jpg
  this.$http.get('/image.jpg').then((response) => {

    // resolve to Blob
    return response.blob();

  }).then(blob) => {
    // use image Blob
  });
}
```
拦截器
可以全局定义一个拦截器，用于预处理和后处理的请求。

请求处理
```
Vue.http.interceptors.push((request, next) => {

  // modify request
  request.method = 'POST';

  // continue to next interceptor
  next();
});
请求和响应处理
Vue.http.interceptors.push((request, next)  => {

  // modify request
  request.method = 'POST';

  // continue to next interceptor
  next((response) => {

    // modify response
    response.body = '...';

  });
});
```
返回一个响应并停止处理
```
Vue.http.interceptors.push((request, next) => {

  // modify request ...

  // stop and return response
  next(request.respondWith(body, {
    status: 404,
    statusText: 'Not found'
  }));
});
```

