---
title: Javascript(0x05)-常用功能实现方案
date: 2020-08-02 10:50:50
tags:
- Javascript
categories:
- [Javascript]
---

###  PC端一键回到顶部的功能实现

* 回到顶部

  * `window.scrollTo() `接口

  ```js
      window.scrollTo(0, 0)
  ```

* 监听页面的滚动

  * 绑定`window`的滚动事件

  ```js
  //绑定
  window.addEventListener('scroll', this.props.onChangeShow)
  
  //处理
  onChangeShow() {
    const scrollY = document.documentElement.scrollTop  //取到滚动的实时位置
    if (scrollY > 400) {
      //修改控制按钮显示的值为true
    }else{
      //修改控制按钮显示的值为false
    }
  }
  ```

* 组件页面销毁时需要移除监听`window`滚动的事件

  ```js
  //componentWillUnmount: react中组件销毁的生命周期函数
  componentWillUnmount() {
      window.removeEventListener('scroll', this.props.onChangeShow)
  }
  ```









###  时间1的日期和时间2的时分秒拼接

```js
  //选择日期
  onDateChange(date, dateString) {
    console.log(dateString)
    let dataTime = new Date(dateString)  //dateString  时间1： 2020-07-14
    let dataMilli = (dataTime.getTime()) - 8*60*60*1000
    this.setState(() => {
      return {
        dataMilli
      }
    })
  }
  //选择时间   ----formatDate是时间工具函数----
  onTimeChange(time, timeString) {
    console.log(new Date(time))  //time  date标准时间
    let timeDate = formatDate(new Date(time), 'yyyy-MM-dd')  //获取日期部分
    let newTime = (new Date(timeDate)).getTime() - 8*60*60*1000  //获取日期部分毫秒数
    let oldTime = (new Date(time)).getTime()
    let timeMilli = oldTime - newTime
    this.setState(() => {
      return {
        timeMilli
      }
    })
  }
```

