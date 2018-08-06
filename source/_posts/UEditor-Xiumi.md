---
title: UEditor对接秀米
date: 2018-07-06 13:48:57
tags:
    - UEditor
    - 秀米
---

### 前言

在上一篇博客里，介绍了在页面中对 UEdiotr 的使用。但是项目里还让对接秀米的编辑器。先看一下期待的[效果](http://hgs.xiumi.us/uedit/)。在页面里点击工具栏最后的秀米图标，会弹出秀米的组件弹窗，选中组件后点击对号，会发现 UEditor 中出现了秀米的组件。好吧，知道了效果就开始艰苦的编码过程吧。
<!-- more -->
### 正文

1. 下载[http://xiumi.us/connect/ue/v5/xiumi-ue-dialog-v5.js](http://xiumi.us/connect/ue/v5/xiumi-ue-dialog-v5.js)和[http://xiumi.us/connect/ue/v5/xiumi-ue-v5.css](http://xiumi.us/connect/ue/v5/xiumi-ue-v5.css)，放在 assets/xiumi 目录下，接着在我们上一篇新建的 ueditor.vue 文件里面引用。

2. 下载[http://xiumi.us/connect/ue/v5/xiumi-ue-dialog-v5.html](http://xiumi.us/connect/ue/v5/xiumi-ue-dialog-v5.html)和[http://hgs.xiumi.us/uedit/dialogs/internal.js](http://hgs.xiumi.us/uedit/dialogs/internal.js)，在 /static 目录下新建 xiumi 文件夹，将这两个文件放进去。这时候要修改上面的 xiumi-ue-dialog-v5.js，把里面的 iframeurl 替换成 '/static/xiumi/xiumi-ue-dialog-v5.html'。

3. 刷新页面，大功告成。