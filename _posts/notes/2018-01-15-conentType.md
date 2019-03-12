---
layout: post
title: Content-Type导致的问题
category: notes
tags: [http, ajax, javascript]
---
记录下前段时间做项目时遇到的一些问题，解决问题的方案，以及问题背后自己缺失的知识点！（考验毅力的时候到了...-_-&#124;&#124;）

### 场景：

* 做项目的在线支付模块的时候，本来是由一个后台的小伙伴做的（vuejs），由于浏览器兼容性的要求，所以就抛给了我来修改。

    项目的页面是用vue+vue-resource来写，因为vuejs只兼容到ie10，但是公司要求兼容到ie8，就不能用vuejs了，所以就选用了jquery来写。

    接口请求方式为POST，参数是字符串，于是就麻溜的写好了：  

    ```
    $.post(url, param)
    .success((data) => {
        // balabala....
    })
    .error((err) => {
        throw(err);
    });
    ``` 

    自以为完事大吉了，没想到报错了...

### 不是知识点的知识点：

* 在苦逼的调试、搜索后，HTTP请求方法为POST，才会有请求体，当 `Content-Type` 为 `application/x-www-form-urlencoded` 和 `multipart/form-data` 时，请求体是 `Form Data`，而`Content-Type` 为其他MIME类型时，请求体是 `Request Payload`。
    
    Jquery的post，默认设置 `Content-Type` 为 `application/x-www-form-urlencoded`，所以请求体是 `Form Data`；而vue-resource的post，默认设置`Content-Type` 为 `application/json`，所以请求体为`Request Payload`；同时XMLHttpRequest实例如果没有显式设置`Content-Type`，则默认为`text/plain`，所以请求体为`Request Payload`。

    当请求体为 `Form Data` 键值对和 `Request Payload `时，服务器的处理方式会有所不同，所以导致了这个问题。

### 解决方案：

* 根据服务器的处理方式，设置 Jquery ajax 的contentType参数为 不同的MIME类型；我们服务器用的是 `Request Payload`，同时为希望是json数据格式来传递数据，所以我的解决方案是设置 `Content-Type` 为 `application/json;charset=UTF-8`；

    ```
    $.ajax({
        url: url,
        data: data,
        type: 'post',
        contentType: 'application/json;charset=UTF-8',
    })
    .success((data) => {
        // balabala....
    })
    .error((err) => {
        throw(err);
    });
    ```

    说起来，还是前后端没有沟通好的，以及自己知识点缺失的问题，以此勉励。

### 延伸：

`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain` 需要先了解Form标签的 `enctype`, `MIME`类型，浏览器自动的表单提交以及利用XMLHttpRequest来提交表单的方式。

#### 一、 浏览器的表单提交

一个form表单可以用以下4中方式发送:

1. 使用 `POST` 方法，`encType` 设置为 `application/x-www-form-urlencoded` (默认，对特殊字符编码)
2. 使用 `POST` 方法，`encType` 设置为 `multipart/form-data` (用于文件上传，不对特殊字符编码)
3. 使用 `POST` 方法，`encType` 设置为 `text/plain` (不对特殊字符编码)
4. 使用 `GET` 方法，`encType` 将被忽略。

Form标签的 `enctype` 属性规定在发送到服务器之前如何对表单数据进行编码。所以当我们提交一个表单时，服务器端会接受到不同的一个字符串。

假设表单有两个字段，分别被命名为 `foo` 和 `baz`。

**方法：`POST`**

**编码类型：`application/x-www-form-urlencoded`**

```
 Content-Type: application/x-www-form-urlencoded

 foo=bar&baz=The+first+line.&#37;0D%0AThe+second+line.%0D%0A
```

`application/x-www-form-urlencoded` 请求数据被编码成以 '&' 分隔的键-值对, 同时以 '=' 分隔键和值. 非字母或数字的字符会被 [`percent-encoding`](https://developer.mozilla.org/zh-CN/docs/Glossary/percent-encoding?_blank): 这也就是为什么这种类型不支持二进制数据的原因 (应使用 `multipart/form-data` 代替)。


**方法：`POST`**

**编码类型：`multipart/form-data`**

```
 Content-Type: multipart/form-data; boundary=---------------------------314911788813839

 -----------------------------314911788813839
 Content-Disposition: form-data; name="foo"

 bar
 -----------------------------314911788813839
 Content-Disposition: form-data; name="baz"

 The first line.
 The second line.

 -----------------------------314911788813839--
```

`multipart/form-data` 作为多部分文档格式，它由边界线（一个由'--'开始的字符串）划分出的不同部分组成。每一部分有自己的实体，以及自己的 HTTP 请求头，`Content-Disposition`和 `Content-Type` 用于文件上传领域，最常用的 (`Content-Length` 因为边界线作为分隔符而被忽略）。

**方法：`POST`**

**编码类型：`text/plain`**

```
 Content-Type: text/plain

 foo=bar
 baz=The first line.
 The second line.
```
**方法：`GET`， 则会把字符串简单的附加到URL**

```
 ?foo=bar&baz=The%20first%20line.%0AThe%20second%20line.
```

#### 二、利用 XMLHttpRequest 来提交表单

利用 XMLHttpRequest 来提交表单有两种方式：

1. 使用AJAX
2. 使用FormData API

第二种方式（使用FormData API）是最简单最快捷的，缺点是被收集的数据只能使用FormData API提供的方法操作。

第一种方式是最复杂的，但同时也是最灵活最强大的。

当使用 FormData API 提交表单时，XMLHttpRequest 默认设置 `Content-Type` 为 `multipart/form-data`。

使用 AJAX 提交表单，参考[提交表单和上传文件](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest#%E6%8F%90%E4%BA%A4%E8%A1%A8%E5%8D%95%E5%92%8C%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6?_blank)
   

### 参考链接

+ [[ Using XMLHttpRequest - MDN ]](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest#%E6%8F%90%E4%BA%A4%E8%A1%A8%E5%8D%95%E5%92%8C%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)，by MDN