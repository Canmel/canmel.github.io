---
layout: post
title: "HTML 中 placeholder提示换行"
description: ""
category: 
tags: [Html, Js]
---

## 需求

- 准备的H5界面是在微信中分享朋友圈的
- 在`textarea`中`placeholder`提示可能需要换行
- 下面要将`文本1`,`文本2`分两行显示

```html
<textarea id="TextArea1" placeholder="文本1~ 文本2~"></textarea>
```

## 代码

```html
<textarea id="TextArea1" placeholder="文本1~&#13;&#10;文本2~"></textarea>
```
**上述代码即可实现换行**

## 问题

- **安卓手机完全可以实现换行操作，ios不支持**

### 解决
1. 添加js

```
<script src="js/jquery.watermark.js"></script>
```

2. textarea添加样式 `class="jq_watermark`

```
 <textarea id="TextArea1" class="jq_watermark" placeholder="文本1~文本2~"></textarea>
```

3. 在文本中添加标签`<br/>`

```
 <textarea id="TextArea1" class="jq_watermark" placeholder="文本1~<br/>文本2~"></textarea>
```

### OK 解决了

{% include JB/setup %}
