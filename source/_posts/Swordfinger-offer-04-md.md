---
title: 剑指offer 04 二维数组中的查找
date: 2022-05-02
tags: 剑指offer
categories: 夜深人静写算法
---
> 中等题 [题目地址](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

# 暴力法
即二重循环 `o(n^2)` 不适合面试

# 二分查找
二分算法的应用，将一维空间抽象到二维空间中，有两种解题思路

<!--more-->

## flat
将高维数组展开为一维并进行排序，这是一种最为简单的做法，在实际开发中不一定能将如题中这么有序，在涉及到要找高维数组中的某一项时，用该方法可以取巧完成业务，但效率不高。
```js
let flatFindNumberInArray = (matrix, target)=>{
    let arr = matrix.flat().sort((a,b)=>a-b) // 从小到大
    let i = 0, j = arr.length-1;
    while(i<j){
        let temp = Math.floor((i+j)/2)
        if(target==arr[temp]) return true
        if(target < arr[temp]){
            j = temp;
        }else {
            i = temp;
        }
    }
    return false
}
```
## 按照矩阵的特点

这种解法适合就题论题，首先我们需要确认一个二分查找的`起点`，即第一次和`target`进行比较的数。起点需要有一个特性，就是一定要比相邻的一个数大，比相邻的另一个数小。

![](https://www.helloimg.com/images/2022/05/03/RdX9hX.png)

由本题可知，符合起点的有两个地方，分别是矩阵的左下角和右上角，这样我们便可以根据它与`target`值的特点进行“流动”起来。

```js
var findNumberIn2DArray = function(matrix, target) {
    let i = matrix.length - 1, j = 0;
    while (i >= 0 && j < matrix[0].length) {
        if (target < matrix[i][j]) i--;
        else if (target > matrix[i][j]) j++;
        else return true;
    }
    return false;
};
```