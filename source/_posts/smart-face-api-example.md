---
title: siroi-face.js—example
date: 2022-05-06
tags: 机器学习
categories: 慌慌张张跑代码
---

> 简单的例子，详见[github](https://github.com/Gao-pw/siroi-face)

<div class="success">

> 该 demo 不会收集用户信息！请放心使用

</div>

<div id="main">
    <button id="but">点击开始demo体验</button>
    <button id="but2">停止</button>
    <div class="siroi_main" style="display: flex; align-items:center; flex-wrap:wrap;">
        <div class="siroi_face" style="position: relative; width: 400px; height: 300px;">
            <video id="face" width="400" height="300" style="position: absolute; object-fit:fill;"></video>
        </div>
        <img id="img" width="200" height="200" style="object-fit:fill;">
    </div>
</div>

<script>
    document.querySelector("#but").addEventListener("click", (ele) => {
        let OnScript = document.createElement("script");
        OnScript.src = "https://cdn.jsdelivr.net/gh/Gao-pw/siroi-face@latest/dist/main.js";
        document.getElementById('main').appendChild(OnScript);
        OnScript.onload = ()=>{
            init();
        };
    });
    document.querySelector("#but2").addEventListener("click",()=>{
        let stream = document.getElementById('face').srcObject;
        stream.getTracks().forEach(element => {
            element.stop();
        });
    });
    const init = async () => {
        let face = new SiroiFace.Face(document.getElementById('face'));
        let img = document.getElementById("img")
        let init = await face.init();
        console.log(init)
        if (init != 'init') return false;
        setInterval(async () => {
            let picture = await face.draw();
            img.src = face.createPicture(picture[0]);
        }, 1000)
    }
</script>

<!--more-->