---
title: Ie Hates Css
author: Devric
date: 2014-04-16 14:23
tag: ie, css, workaround
template: article.jade
---

Ie Hates css, it's fustrating and we all know that... So for those who not yet using a css preprocessor and need to break css down into components or pages during the development of a large application, you wouldn't want to cocat the whole css file every time you make a small change, so to make your development process faster you would just link those css files one by one into your app. But ie has a limit of how many css files it can load. heres how to do it.

<span class="more"></span>

To make IE loads all the css file you can use inline @import

```
<style>
@import 'button.css'
@import 'page.css'
</style>
```

But it will still hit a limit if you keep on @importing more than 30 files per style tag, what you do is do a loop and make sure there no more than 20 @import per style tags (I don't remember the excat limit, but as long as it kept under 20 @imports, it should work fine. you can easily break your css down to more than 100 files).

Like this

```
<style>
@import 'button.css'
...
...
...
@import 'layout-app.css'
@import 'layout-body.css'
@import 'layout-footer.css'
</style>

<style>
@import 'page-home.css'
@import 'pages-omething.css'
...
...
...
@import 'widget-slider-one.css'
</style>
```
