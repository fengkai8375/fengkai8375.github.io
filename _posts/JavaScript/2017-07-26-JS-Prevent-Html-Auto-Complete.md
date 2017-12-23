---
layout: post
title: "防止自动填充autocomplete"
date: 2017-07-26 09:00:00 +0800 
categories: JavaScript
tag:
 - Html5
---
* content
{:toc}

直接设置 `autocomplete='off'` 或 `autocomplete='nope'` 是不行的

最有效的方法先设置成`只读`，获取焦点时再取消只读。

```
<input readonly onfocus="this.removeAttribute('readonly');" />
```

