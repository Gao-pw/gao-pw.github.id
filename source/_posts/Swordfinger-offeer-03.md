---
title: 剑指offer 03 数组中重复的数字
date: 2022-05-03
tags: 剑指offer
categories: 夜深人静写算法
---
> 简单题 [题目地址](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

梭哈暴力解法 时间复杂度`O(n)`
```js
var findRepeatNumber = function(nums) {
    for(let i=0;i<nums.length;i++){
        if(nums.indexOf(nums[i])!== i){
            return nums[i];
        }
    }
};
```
<!-- more -->

<div class="info">

> 思考

</div>

在面试和日常开发中，经常会遇到数组去重的问题，我们将上面的题目变一下：`找到所有重复的数字，返回数组`，解法如下
```js
var findRepeatNumbers = function(nums) {
    return nums.filter((value,index,array)=>{
        return array.indexOf(value)!==index
    })
};
```
![RdFexQ.png](https://www.helloimg.com/images/2022/05/03/RdFexQ.png)_题解_