---
title: js-函数防抖与函数节流
date: 2020-01-11 13:41:22
tags: ['节流', '防抖', '性能优化']
categories: web
---

>  一些dom或者bom事件,如`resize`,`scroll`,`input`...会频繁执行(采用最小的时间单位4ms-10ms),浪费性能也容易卡顿,可以用防抖和节流做些性能优化

## 函数节流(throttle)
> 显示器刷新率一般是60/120fps,人眼睛能捕捉的频率,如果特别高,没有实际意义
```javascript
// 节流(固定单位时间内触发一次)
function throttle(fn, delay = 1000) {
  let flag = true // 做一个判断标识,当是false,return
  return function () { //闭包
    if (!flag) {
      return
    }
    flag = false
    setTimeout(() => {
      fn.apply(this, arguments) // 改变指针和传入参数
      flag = true
    }, delay)
  }
}
```

## 函数防抖(debounce)

>如果有人进电梯（触发事件），那电梯将在10秒钟后出发（执行事件监听器），这时如果又有人进电梯了（在10秒内再次触发该事件），我们又得等10秒再出发（重新计时）。

```javascript
function debounce(fn, delay = 1000) {
  let timer = null
  return function () {
    clearTimeout(timer)

    timer = setTimeout(() => {
      fn.apply(this, arguments)
    }, delay)
  }
}
```

### 应用场景

对于函数防抖，有以下几种应用场景：

- 给按钮加函数防抖防止表单多次提交。
- 对于输入框连续输入进行AJAX验证时，用函数防抖能有效减少请求次数。
- 判断`scroll`是否滑到底部，`滚动事件`+`函数防抖`

> 总的来说，适合多次事件**一次响应**的情况

对于函数节流，有如下几个场景：

- 游戏中的刷新率
- DOM元素拖拽
- Canvas画笔功能

> 总的来说，适合**大量事件**按时间做**平均**分配触发。