---
layout: post
title: H5 上传图片
category: notes
tags: [javascript, web API]
---
h5上传图片已经不是什么很新鲜的功能了，下面就根据我在开发中遇到的一些场景或者功能点来，记录问题、解决方案以及当中所用的知识点。

### 一、修改 file input 标签元素的默认样式以及触发它的 change 事件

这块功能的实现对于前端工程师来说基本上已经不是什么问题了，之所以把它拎出来，是因为我在项目中遇到了这个问题，所以拿出来记录下。

修改 file input 标签元素的默认样式有两种：

1. file input 标签元素设置为透明，并设置高层次，覆盖显示出来的自定义的样式

    ```
     <!-- html 布局 -->
     <div class="upload-box">
         <a class="upload-btn" href="javascript: void(0);">上传</a>
         <input type="file" id="fileElem" multiple accept="image/*" class="upload-input" onchange="handleFiles(this.files)" />
     </div>

     <!-- 样式布局 -->
     <style>
         .upload-box{position: relative;display: inline-block;}
		 .upload-btn{display: block;width: 120px;height: 38px;border-radius: 6px;color: #fff;text-decoration: none;font-size: 14px;text-align: center;line-height: 38px;background-color: blue;}
		 .upload-input{position: absolute;left: 0;top: 0;width: 100%;height: 100%;opacity: 0;}
     </style>

     <!-- change 事件监听 -->
     <script>
         function handleFiles(files){
             console.log(files);
         }
     </script>
    ```
2. file input 标签元素隐藏，由其他标签的 click 方法触发 file input 标签
   
   ```
     <!-- 由其他标签元素触发（除label） -->
     <!-- html 布局 -->
     <div class="upload-box">
         <a class="upload-btn" href="javascript: void(0);">上传</a>
         <input type="file" id="fileElem" multiple accept="image/*" class="upload-input" />
     </div>

     <!-- 样式布局 -->
     <style>
         .upload-box{position: relative;display: inline-block;}
		 .upload-btn{display: block;width: 120px;height: 38px;border-radius: 6px;color: #fff;text-decoration: none;font-size: 14px;text-align: center;line-height: 38px;background-color: blue;}
		 .upload-input{display: none;}
     </style>
    
     <!-- click 事件监听，以及触发 file input 标签 -->
     <script>
         var fileSelect = document.querySelector('.upload-btn'),
             fileElem = document.getElementById('fileElem');

         fileSelect.addEventListener('click', function(e){
             if(fileElem){
                 fileElem.click();
             }
         }, false);
     </script>

     <!-- 由 label 标签触发 -->
     <!-- html 布局 -->
     <div class="upload-box">
         <label class="upload-btn" for="fileElem">上传图片</label>
         <input type="file" id="fileElem" multiple accept="image/*" class="upload-input" />
     </div>
     
     <!-- 样式布局 -->
     <style>
         .upload-box{position: relative;display: inline-block;}
		 .upload-btn{display: block;width: 120px;height: 38px;border-radius: 6px;color: #fff;text-decoration: none;font-size: 14px;text-align: center;line-height: 38px;background-color: blue;}
		 .upload-input{display: none;}
     </style>
   ```

### 二、上传数据格式以及编码转换


### 三、上传图片的预览图处理方式


### 四、多张图片的并发处理


### 五、上传进度处理



数据在计算机内部都是以二进制形式存在，无论是数字还是字符串。数字好说，就是进制的转换，怎么保证字符数据在不同计算机间的一致性呢？这就是编码存在的意义。拿最简单的ASCII编码举例，它编排了128个可打印和控制字符，其中包括大小写英文字母，数字和换行，制表符等。字母A的序号是65，计算机在存储字符A的时候，实际是用了8比特也就是一个字节存储了的序号65。那如果给你这样的一个字节，你既可以认为是10进制的数字65，也可以认为是字符A。同样，如果你使用了错误的编码方式去识别一串字符的数据，就会出现乱码显示。因为不同编码系统里，字符的序号是不一样的。这个情况适用于硬盘上的数据和我们程序里申明的变量。

base64 是用 64 个可打印字符来编码任意二进制数据的一种编码方式。js 语言里只提供了 atob 来解码，本质这个函数的意思是 ASCII to binary，但是由于历史原因,这个函数并没有真正返回 array buffer 形式的 binary，它返回了一段字符串。这个字符串其实叫 binary string，就是强行把这部分数据看作是字符串，用字符串类型的变量来接收了，但字符串背后的二进制确实是 base 64 编码前的二进制。js 字符串是 UTF16 编码的，charCodeAt 获取的就是字符的序号，其实也就是背后的二进制以数值形式读取所对应的值。这个时候把这个值以 typedArray 形式拼接回去，自然就能得到之前的二进制数据。

1. Javascript 使用Unicode字符集。Javascript引擎内部，所有字符都用Unicode表示。JavaScript 不仅以 Unicode 储存字符，还允许直接在程序中使用 Unicode 码点表示字符，即将字符写成\uxxxx的形式，其中xxxx代表该字符的 Unicode 码点。
2. 每个字符在Javascript内部都是以16位（即两个字节）的UTF-16格式存储的。
   
   但是，UTF-16 有两种长度：对于码点在U+0000到U+FFFF之间的字符，长度为16位（即2个字节）；对于码点在U+10000到U+10FFFF之间的字符，长度为32位（即4个字节），而且前两个字节在0xD800到0xDBFF之间，后两个字节在0xDC00到0xDFFF之间。举例来说，码点U+1D306对应的字符为𝌆，它写成 UTF-16 就是0xD834 0xDF06。


**Formdata**

Blob

File/Filelist/DataTransfer

FileReader

base64

Canvas toDataUrl 

Uint8Array

String charCodeAt

