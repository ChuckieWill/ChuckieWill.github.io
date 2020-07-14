---
title: "Warning: findDOMNode is deprecated in StrictMode. findDOMNode was passed an instance of Wave which is inside StrictMode. Instead, add a ref directly to the element you want to reference."
date: 2020-07-08 21:06:17
tags:
- react
- StrictMode
categories:
- [react, BUG]
---

##  严格模式引起的警告

**警告原因：**

* 开启了**严格模式**

```js
ReactDOM.render(
  <React.StrictMode>
    <TodoList />
  </React.StrictMode>,
  document.getElementById('root')
);
```

**解决方案：**

* 关掉**严格模式**
  * 去掉`<React.StrictMode>`对根组件的包裹

```js
ReactDOM.render( <TodoList />, document.getElementById('root'));
```

**关于严格模式：**

> `StrictMode` 是一个用来突出显示应用程序中潜在问题的工具。与 `Fragment` 一样，`StrictMode` 不会渲染任何可见的 UI。它为其后代元素触发额外的检查和警告。 
>
> **[React官网-严格模式]( https://zh-hans.reactjs.org/docs/strict-mode.html#gatsby-focus-wrapper )**

