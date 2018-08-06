---
title: 文件下载和上传
date: 2018-06-14 10:15:04
tags:
    - FormData
    - 文件下载
    - 文件上传
---

### 前言

这几天做项目，需要文件下载及上传的功能。之前没有接触过这方面的内容，于是磕磕碰碰的开始了摸索。
<!-- more -->

### 正文


讲到文件下载和上传的话，就必须得提到 FormData 对象了。先来一段 MDN 对于 FormData 对象的解释：

>FormData对象用以将数据编译成键值对，以便用XMLHttpRequest来发送数据。其主要用于发送表单数据，但亦可用于发送带键数据(keyed data)，而独立于表单使用。如果表单enctype属性设为multipart/form-data ，则会使用表单的submit()方法来发送数据，从而，发送数据具有同样形式。

下载文件，一般的接口都是 get 请求，这个时候一般是动态生成一个 form 表单，然后添加属性，最后执行 submit 方法，开始下载。

    $('#download').click(function () {

      var form = $("<form>");

      //设置表单状态为不显示
      form.attr("style", "display:none");

      //method属性设置请求类型为post
      form.attr("method", "get");

      //action属性设置请求路径,
      //请求类型是post时,路径后面跟参数的方式不可用
      //可以通过表单中的input来传递参数
      form.attr("action", globalUrl + '/downloadTemplateFile.htm');
      $("body").append(form);//将表单放置在web中

      //在表单中添加input标签来传递参数
      //如有多个参数可添加多个input标签
      var input1 = $("<input>");
      input1.attr("type", "hidden");
      input1.attr("name", "downLoadFileName");
      input1.attr("value", "smsTemplate.txt");
      form.append(input1);
      var input2 = $("<input>");
      input2.attr("type", "hidden");
      input2.attr("name", "fileName");
      input2.attr("value", "smsTemplate.txt");
      form.append(input2);

      form.submit();//表单提交
    })

比较有意思的是如果直接 from 表单提交的话，在 form 表单里面的属性都会被当成参数传递，input 的 name 可以设置为对应的参数名，value 设置为对应值，最后执行 submit 方法的时候，会把需要的参数全部传过去。

来到文件下载了，还是直接上代码吧。

html代码

    <form action="" style="display: none" id="upload-form" method="post">
        <input type="file" name="file" id="upload_file">
        <input type="text" name="couponDirectId" id="couponDirectId">
    </form>

js代码

    var dataParams = new FormData(document.getElementById('upload-form'));
    $.ajax({
      type: 'POST',
      url: globalUrl + '/test.htm',
      processData: false,
      contentType: false,
      data: dataParams,
      success: function (res) {
        console.log(res)
      },
      error: function (err) {
        console.log(err)
      }
    })

之前说过执行 form 表单的 submit 方法的话，会直接把 form 标签里面的表单元素的 name 和 value 作为参数传过去。也可以设置 form 表单里面元素的属性，然后在 js 里面执行

    var dataParams = new FormData(document.getElementById('upload-form'));


将 form 表单里面的元素转化为 FormData 对象，进行传递。