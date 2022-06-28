---
title: "[front] 手写 promise" 
date: 2022-06-28
tags: 代码那些事
categories: 慢慢悠悠写代码
aplayer: false
---
> emmm，先贴代码

<!--more-->

```js
class _Promise {
    static PENDING = "pending";
    static FULFILLED = "fulfilled";
    static REJECTED = "rejected";
    constructor(executor) {
        this.state = _Promise.PENDING;
        this.value = null;
        this.stack = []; // 函数栈
        try {
            executor(this.resolve.bind(this), this.reject.bind(this));
        } catch (error) {
            this.reject(error);
        }
    }

    resolve(value) {
        if (this.state === _Promise.PENDING) {
            this.state = _Promise.FULFILLED;
            this.value = value;
            setTimeout(() => {
                this.stack.map(callback => {
                    callback.onFulfilled(value);
                })
            })
        }
    }
    reject(reason) {
        if (this.state === _Promise.PENDING) {
            this.state = _Promise.REJECTED;
            this.value = reason;
            setTimeout(() => {
                this.stack.map(callback => {
                    callback.onReject(reason);
                })
            })
        }
    }
    then(onFulfilled, onReject) {
        if (typeof onFulfilled !== "function") {
            onFulfilled = () => this.value
        }
        if (typeof onReject !== "function") {
            onReject = () => this.value
        }

        return new _Promise((resolve, reject) => {
            if (this.state == _Promise.PENDING) {
                this.stack.push({
                    onFulfilled: (value) => {
                        try {
                            let result = onFulfilled(value);
                            resolve(result);
                        } catch (error) {
                            reject(error)
                        }
                    }, onReject: (reason) => {
                        try {
                            let result = onReject(reason);
                            resolve(result)
                        } catch (error) {
                            reject(error)
                        }
                    }
                })
            }

            if (this.state == _Promise.FULFILLED) {
                setTimeout(() => {
                    try {
                        let result = onFulfilled(this.value);
                        resolve(result)
                    } catch (error) {
                        reject(error)
                    }
                })
            }
            if (this.state == _Promise.REJECTED) {
                setTimeout(() => {
                    try {
                        let result = onReject(this.value)
                        resolve(result)
                    } catch (error) {
                        reject(error)
                    }
                })
            }
        })
    }
}
```
