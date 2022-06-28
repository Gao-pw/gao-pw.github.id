---
title: "[front] 有趣的 form 之同步提交" 
date: 2022-05-09
tags: 代码那些事
categories: 慢慢悠悠写代码
---

> "不管花开的多漂亮，人们最终还是会忘却"

正式进入 **「前端领域」** 满打满算一年了，去年的这时候刚去 **「新东方」** 实习，没错，当我刚去的时候，**教育行业** 还在风口上，但两个月后就迎来了 **「双减」**。但在新东方的日子让我学到了很多，之后有机会再把这段故事写下来。

如今的前端框架选择太多了，之前开发用 **「react」** 作为主力，现在投奔 **「vue」** 了，二者在组件库上的选择很多。这些组件库都对 **「form」** 表单有着良好的封装，在提交表单的时候只需拿到表单里的值再用 **fetch** 或者 **axios** 提交到后端即可，很是方便。

但！在维护一个远古的 **「PHP」** 的项目时，框架用多了 **form** 的原生写法都忘了!!借此机会好好复习一下。

![](https://www.helloimg.com/images/2022/05/09/RNipoT.png)

<!--more-->

## 简单的前端和后端
```html
<form action="/api/v1/show" method="post" >
    <input type="text" value="111" name="input1" />
    <button type="submit" >提交</button>
</form>
```
```php
public function showAction(){
    var_dump($_POST)
}
```
![](https://www.helloimg.com/images/2022/05/09/RNi8dD.png)

这是一个标准的表单，点击 **提交** 后端会接受到如下信息

![](https://www.helloimg.com/images/2022/05/09/RNiKYS.png)

这一切都是那么正常，此刻将前端代码稍加改一下

```html
<form action="/api/v1/show" method="post" >
    <input type="text" value="111" name="input1" />
    <button>提交</button>
</form>
```
把 <kbd>submit</kbd> 去了，点一下，还是可以提交的

![](https://www.helloimg.com/images/2022/05/09/RNigeC.png)

这是因为如果不在表单中指定 **submit** 会默认将里面的 button 添加 submit 属性，有时候会造成困扰。

接下来再改一下代码，并进行提交
```html
<form action="/api/v1/show" method="post" >
    <input type="text" value="111" name="input1" />
    <button name="sub" value="1">提交</button>
</form>
```

![](https://www.helloimg.com/images/2022/05/09/RNik2Q.png)

表单成功提交了，但结果好像不一样，后端会把 button 里的 value 也提交上去，在多分支选项时，这种方法有用。

<div class="info">

> 说了这么多，我们还停留在同步处理任务阶段，一点按钮，表单立马进行提交。若我们在表单提交前处理一条异步请求，之后根据异步请求在提交表单怎么办？下一节分解！

</div>

