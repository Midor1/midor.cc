---
title: "在Hugo中使用KaTeX渲染公式"
date: 2021-06-01T20:22:00+08:00
categories:
- Notes
draft: false
---

为了在博客中渲染公式，需要引入LaTeX渲染器。[KaTeX]是一个不错的选择。

<!--more-->

KaTeX是[Khan Academy]开发的LaTeX解析插件，具有速度快，服务端渲染等优势。

# 引入KaTeX

在Hugo中引入KaTeX很容易，只需要在所使用的主题模板中加入KaTeX的CSS与JavaScript文件即可：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.13.11/dist/katex.min.css">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.13.11/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.13.11/dist/contrib/auto-render.min.js" onload="renderMathInElement(document.body);"></script>
```

上面的代码使用了CDN，对于博客这类静态网站我更倾向于本地托管静态文件。在Hugo中添加静态文件可以通过修改theme文件夹的static实现，也可以通过直接添加static的形式实现。这里我们使用第二种方法，将官网提供的katex.zip下载后解压到网站根目录下的`/static/`目录下，就可以在页面中用这样的形式引入了：

```html
<link rel="stylesheet" href="/katex/katex.min.css">
<script defer src="/katex/katex.min.js"></script>
<script defer src="/katex/contrib/auto-render.js" onload="renderMathInElement(document.body);"></script>
```

具体在哪一个页面中引入，则取决于博客的具体设计。如果不介意不必要的资源载入，可以在主题的`header.html`中引入。这种方法的优点是一次性引入，后续无需操作；但如果页面上不需要使用LaTeX，则可能会影响载入性能。我们可以利用Hugo的模版解析语法，对KaTeX进行有选择的引入，例如：

`header.html`:
```html
{{ if .Params.katex}}{{ partial "katex.html" . }}{{ end }}
```

在`katex.html`中引入上述的CSS和JS，接着，在Hugo内容文档的`front matters`中配置`katex`参数即可：
```toml
katex=true
```

# 使用KaTex

书写公式的过程与普通LaTeX语法基本一致，唯一需要注意的是定界符的配置可以按照个人习惯来：

```javascript
document.addEventListener("DOMContentLoaded", function() {
            renderMathInElement(document.body, {
                delimiters: [
                    {left: '$$', right: '$$', display: true},
                    {left: '$', right: '$', display: false}
                ],
                throwOnError : false
            });
        });
```

下面是一些例子：

例1：

```latex
...

$$pV=nRT$$

...
```
...

$$pV=nRT$$

...

例2：
```latex
Integrate x^2: $\int x^2 dx$
```

Integrate x^2: $\int x^2 dx$

[KaTeX]: 'https://katex.org/'
[Khan Academy]: 'https://www.khanacademy.org/'
