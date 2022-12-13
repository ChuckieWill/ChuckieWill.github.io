---
title: vue实战经验
date: 2022-05-23 10:43:17
tags:
- vue
categories:
- [Index]
---



#  组件化开发

##  上传图片组件

###  上传文件的两种实现方式

> - 一种是使用 传统的 form submission，表单提交的方式
> - 第二种是使用 Javascript 发送异步请求的方式

####  **传统表单模式**

* form method设置为post
* enctype="multipart/form-data"  ： multipart/form-data表示提交的文件以二进制传输
  * [关于 POST encType 的描述](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST)
  * 文件也只能以二进制进行传输
* action为后端接受上传文件的接口
* input框的type设置为file， 此时界面的输入框为上传文件的选择，点击后就会弹出文件夹进行选择
* 点击button时就会触发向action地址发送请求

```html
<form method="post" encType="multipart/form-data" action="https://jsonplaceholder.typicode.com/posts">
  <input type="file" />
  <button type="submit">Submit</button>
</form>  
```

####  js发送异步请求的方式

* 当点击上传文件后就会触发input的change事件回调函数
* 在回调函数中通过target可以拿到上传的文件
  * 文件可以上传多个所以是以类似数组的形式存储的  若只上传一个文件 则通过`files[0]`拿到数组第一个元素即为上传的文件
  * 通过`uploadedFile.name`可以拿到文件名
* 通过`new FormData()`创建编码二进制的对象
  * `formData.append(uploadedFile.name, uploadedFile)`函数将要编码的文件进行编码
    * 第一个参数为：要编码的文件名
    * 第二个参数为:  要编码的文件
* 发送post请求时加上header：`'Content-Type': 'multipart/form-data'`, 声明传输的数据是二进制的

```html
<input type="file" @change="handleFileChange"/>
```



```js
  const handleFileChange = (e: Event) => {
    // 在这个 input 组件中，我们可以拿到它选择的 file 对象
      const target = e.target as HTMLInputElement
      const files = target.files    
      // 注意这个是一个 files 列表，也就是 fileList 对象，它是一个 array-like 的 object，但是不是一个数组，它支持选择多个文件，所以它可能有多个
      if (files) {
         // 我们拿到它的第一项，就是我们选择的文件
          const uploadedFile = files[0]
          // 然后让我们来模拟表单的数据我们可以使用 FormData 对象，这是另一种针对 XHR2 设计的新数据类型。使用 FormData 能够很方便地实时以 JavaScript 创建 HTML <form>。
          // 文档 https://developer.mozilla.org/zh-CN/docs/Web/API/FormData
          const formData = new FormData()
          // 并通过调用 append 方法为其附加了 <input> 值。
          // 就这样我们通过 FormData 对象添加了 input 的值
          formData.append(uploadedFile.name, uploadedFile)
          // 现在有了表单数据，我们可以发送 post 请求了，注意 axios post 的第二个参数，即支持普通的 object，也支持 formData。在这里我们需要添加一个额外的 header，就是Content-type，这个对应的表单的 encType，为了传文件，我们修改成 mutilpart
          axios.post("/upload", formData , {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }).then(resp => {
            console.log(resp)
          })
      }
  }
```

###  组件开发

####  input处理成需求样式

> 需求：`<input type="file" @change="handleFileChange"/>`原本的样式是类似一个按钮，点击就可以触发选择文件，但是组件的需求可能是点击某个区域或自定义样式的组件触发文件的选择
>
> 解决方案：通过`display: none`隐藏inout框，然后通过点击自定义样式的标签触发事件，在事件处理过程中通过js拿到input标签并触发它的点击事件，从而触发文件的选择
