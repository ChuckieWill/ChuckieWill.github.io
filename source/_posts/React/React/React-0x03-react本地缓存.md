---
title: React(0x03)-react本地缓存
date: 2020-07-31 11:42:56
tags:
- react
- 本地缓存
categories:
- [react, react]
---

##  React本地存储

**工具函数： `storage.js`**

```js
const storage = {
	//1 存储到本地
    set(key, value) {
        localStorage.setItem(key, JSON.stringify(value))
    },
    //2 从本地获取
    get(key) {
        return JSON.parse(localStorage.getItem(key))
    },
    //3 删除本地存储
    remove(key) {
        localStorage.removeItem(key)
    }
}

export default storage;
```

**使用：**

```js
import storage from '../utils/storage/storage'

storage.set("todoList", tempList);
```

