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

* 在苦逼的调试、搜索后，发现jquery的post，get，ajax默认会设置`Content-Type`为`application/x-www-form-urlencoded`，ajax为POST时，请求参数是放在请求体中的，当`Content-Type`为`application/x-www-form-urlencoded`，请求体是`Form Data`，而当用vue-resource时，请求体为`Request Payload`，原生ajax如果没有显式设置`Content-Type`，则默认为`text/plain`，请求体为`Request Payload`。

    当请求体为`Form Data`键值对和为`Request Payload`时，服务器的处理方式会有所不同，所以导致了这个问题。

### 解决方案：

* 设置ajax的contentType参数为`application/json;charset=UTF-8`；

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

* 暂时只记录问题，以及解决方案，延伸知识神马的以后补充...