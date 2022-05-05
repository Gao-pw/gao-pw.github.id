---
title: siroi-face.js——聪明的人脸检测
date: 2022-05-04
tags: 机器学习
categories: 慌慌张张跑代码
---
> 写在前面  

OK，在好多`后端`同学眼里，`JavaScript`就是个垃圾，要计算精度不够，要写业务这货的`this`特别奇怪，按我导的思想：JavaScript能显示字符串就行。

![](https://www.helloimg.com/images/2022/05/04/RwGen5.png)

作为一名前端垃圾，自然不能忍这种语言歧视，毕竟要靠 **js** 吃饭的，只有老子才能证明 **js** 就是垃圾！
<!--more-->

<div class="yellow">

> 缘起  ~(明明vue写的好好的，你说你换PHP干什么🖕)~

</div>

因为本人的毕设是用 `FPGA` 做人脸识别神经网络加速，而作为神经网络的输入使用笔记本摄像头实现的，第一版是用 `vue` 写的，本来好好的，诶，我导就不，非要整什么`PHP`，tmd，都2022了，`npm` 不香吗，而且 `PHP` 即便用框架，`html` 里的 `<script>` 标签没法用 `npm` 包。
![](https://www.helloimg.com/images/2022/05/05/RwR7Rb.png)
本来想着前后端分离，最后还是一锅端😞

<div class="info">

> 开整

</div>

由于`PHP`框架带来的缺点，因此首先第一步需要先将要实现的功能从 `npm` 中打包出来 

|  工具   | 干啥的  |
|  ----  | ----  |
| vscode  | 写代码的 |
| webpack  | 打包js的 |

## webpack 配置

为了方便代码阅读和项目开发，使用 `dev` 环境

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'siroi-face.js',
    path: path.resolve(__dirname, 'dist'),
    library: "siroi",
    libraryTarget:"var"
  },
  mode:'development'
};
```

emmmm, 突然不想写怎么实现的了，耗时 `one day`，封装了一个类，里面有两个方法：
1. init()
> 方法初始化，加载模型参数以及调用电脑摄像头

2. draw()
> 这个方法是为了拿到截取好的人脸图像，返回一个 `canvas` 对象(promise)

<details>
  <summary>例子</summary>
  这个例子是为了不断的拿到截取到的图像

```html
<video id="face" width="400" height="300" style="position: absolute;object-fit:fill;"></video>
<script src="siroi-face.js"></script>
<script>
    let face = new siroi.Face(document.getElementById('face'));
    const init = async () => {
        await face.init();
        setInterval(async () => {
            let a = await face.draw();
            console.log(a);
        }, 1000)
    }
    init();
</script>
```

放一张 👴 心中的导师。~有些人为啥能当老师呢，明明人都做不好，艹~
![](https://www.helloimg.com/images/2022/05/05/RwZh2r.png)

</details>


