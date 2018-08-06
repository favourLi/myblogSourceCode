---
title: UEditor编辑器的使用
date: 2018-07-06 10:18:51
tags:
    - UEditor
    - 秀米
    - UEditor插入视频
---

### 前言

百度编辑器 UEditor 目前来讲算是国内运用比较广泛的一个编辑器了，不仅开源而且还有中文文档。最近也在使用它，正好把自己遇到的问题和解决的过程都记录下来。
<!-- more -->

### 正文

首先是下载 [UEditor](http://ueditor.baidu.com/build/build_down.php?n=ueditor&v=1_4_3_3-src)，下载完成之后解压文件，进入该文件夹后，打开命令行工具，输入

    npm i
安装结束后，执行

    grunt

就会打包代码

>{% asset_img 1.png %}

打包完成之后，会生成一个 dist 文件夹，进入该文件夹，复制 utf8-php 文件夹，粘贴到项目里面的 src/assets 目录下面，同时也要粘贴在项目的 static 目录里面，assets 目录下面除了 lang 文件夹和与其平行目录的js文件外都删除。项目目录大致如下图

>{% asset_img 3.png %}

文件都处理妥当之后，打开 assets 目录下面的 ueditor.config.js，修改下图代码

>{% asset_img 2.png %}

    // 这里是配置Ueditor内部进行文件请求时的静态文件服务地址
    window.UEDITOR_HOME_URL = "/static/utf8-php/";

配置完成之后,在 components 目录里新建 ueditor.vue 文件，引入

      import '../assets/utf8-php/ueditor.config.js'
      import '../assets/utf8-php/ueditor.all.min.js'
      import '../assets/utf8-php/ueditor.all.js'
      import '../assets/utf8-php/lang/zh-cn/zh-cn.js'
      import '../assets/utf8-php/ueditor.parse.min.js'
      import '../assets/utf8-php/ueditor.parse.js'

然后在 vue 文件，template标签里面写入

    <script id="editor" type="text/plain"></script>

script标签 mounted 方法里面写入

    this.editor = UE.getEditor('editor', {
      initialFrameWidth: 460,
      initialFrameHeight: 772
    });

destroyed方法里面写入

    this.editor.destroy();

这个时候引用该组件，打开项目首页，在控制台会发现下图错误

>{% asset_img 4.png %}

这是因为 ueditor 的 js 方法里引用了这些方法，但是 babel 在转码的时候默认为所有 js 文件加了 use stric，而严格模式下诸如

    arguments.callee

就会报错。解决的方法在[这里](https://github.com/JiayiLi/TristaLee-vue-blog/issues/1)。

解决之后打开首页

>{% asset_img 5.png %}

我们会发现控制台还是有错误提示，这是因为上传的路径不正确，但是我们的项目是要上传到自己的 oss 服务器的，所以只需要在 ueditor.config.js 里面，把

    , serverUrl: URL + "php/controller.php"

这行代码注释掉就好了。

但是这么多按钮，我需要的只有几个，这时候打开 ueditor.config.js, 找到 toolbars 属性，把不需要的全部删除，

>{% asset_img 6.png %}
>{% asset_img 7.png %}

清爽很多了。

我的项目需要插入图片和视频的形式和 ueditor 是不一样的，所以需要我添加自定义按钮和事件。查看文档后找到了 UE.registerUI 方法，在 mounted 方法里写入下列代码

    let that = this;
    UE.registerUI('插入图片 插入视频', function(editor, uiName) {
        //注册按钮执行时的command命令，使用命令默认就会带有回退操作
        editor.registerCommand(uiName, {
            execCommand: function() {
              if (uiName == '插入图片') {
                // 你自己的上传图片事件
              } else {
                // 你自己的上传视频事件
              }
            }
        });
        //创建一个button
        var btn = new UE.ui.Button({
            //按钮的名字
            name: uiName,
            //提示
            title: uiName,
            //添加额外样式,指定icon图标,这里使用一个原始的插入图片和插入视频icon
            cssRules: uiName == '插入图片'?'background-position: -380px 0;':'background-position: -320px -20px;',
            //点击时执行的命令
            onclick: function() {
                //这里可以不用执行命令,做你自己的操作也可
                editor.execCommand(uiName);
            }
        });
        
        //当点到编辑内容上时，按钮要做的状态反射
        editor.addListener('selectionchange', function() {
            var state = editor.queryCommandState(uiName);
            if (state == -1) {
                btn.setDisabled(true);
                btn.setChecked(false);
            } else {
                btn.setDisabled(false);
                btn.setChecked(state);
            }
        });
        //因为你是添加button,所以需要返回这个button
        return btn;
    });

然后在上面插入自己的上传图片和视频的方法就完工了。

