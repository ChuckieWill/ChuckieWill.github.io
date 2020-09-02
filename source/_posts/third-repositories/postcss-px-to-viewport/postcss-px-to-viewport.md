---
title: postcss-px-to-viewport
date: 2020-02-04 14:45:50
tags:
- postcss-px-to-viewport
categories:
- [third-repositories, postcss-px-to-viewport]
---

#  postcss-px-to-viewport

**`postcss-px-to-viewport`用于适配，将单位px---->vw**

##  单位基础知识

*  设计稿标准：iPhone6（750x1334）（375x667）

##  postcss-px-to-viewport的使用

* github:  [postcss-px-to-viewport]( https://github.com/evrone/postcss-px-to-viewport )

* 安装

```
npm install postcss-px-to-viewport --save-dev
```

* postcss.config.js中设置（项目根目录下自建文件)

```
module.exports = {
  plugins: {
    autoprefixer: {},
    "postcss-px-to-viewport": {
      viewportWidth: 375,  //视窗宽度，对应设计稿的宽度 ，单位变换以此为基础
      viewportHeight: 667, //视窗高度，对应设计稿的高度
      unitPrecision: 5, // 指定'px'--->'vw'时保留小数位数 
      viewportUnit: 'vw', //希望使用的视口单位(vw或vh),建议vw
      minPixelValue: 1, //设置最小的转换数值，如果为1的话，只有大于1的值会被转换
      mediaQuery: false, //
      selectorBlackList: ['tab-bar', 'tab-bar-item'],//需要忽略的CSS选择器，不会转为视口单位，使用原有的px等单位。
    }
  }
}
```

##  配置参数及意思

- `unitToConvert` (String) 需要转换的单位，默认为"px"

- `viewportWidth` (Number) 设计稿的视口宽度

- `unitPrecision` (Number) 单位转换后保留的精度

- `propList`(Array) 能转化为vw的属性列表

  - 传入特定的CSS属性；
  - 可以传入通配符"*"去匹配所有属性，例如：['*']；
  - 在属性的前或后添加"*",可以匹配特定的属性. (例如['*position*'] 会匹配 background-position-y)
  - 在特定属性前加 "!"，将不转换该属性的单位 . 例如: ['*', '!letter-spacing']，将不转换letter-spacing
  - "!" 和 "*"可以组合使用， 例如: ['*', '!font*']，将不转换font-size以及font-weight等属性

- `viewportUnit` (String) 希望使用的视口单位

- `fontViewportUnit` (String) 字体使用的视口单位

- `selectorBlackList`(Array) 需要忽略的CSS选择器，不会转为视口单位，使用原有的px等单位。

  - 如果传入的值为字符串的话，只要选择器中含有传入值就会被匹配
    - 例如 `selectorBlackList` 为 `['body']` 的话， 那么 `.body-class` 就会被忽略
  - 如果传入的值为正则表达式的话，那么就会依据CSS选择器是否匹配该正则
    - 例如 `selectorBlackList` 为 `[/^body$/]` , 那么 `body` 会被忽略，而 `.body` 不会

- `minPixelValue` (Number) 设置最小的转换数值，如果为1的话，只有大于1的值会被转换

- `mediaQuery` (Boolean) 媒体查询里的单位是否需要转换单位

- `replace` (Boolean) 是否直接更换属性值，而不添加备用属性

- `exclude`(Array or Regexp) 忽略某些文件夹下的文件或特定文件，例如 'node_modules' 下的文件

  - 如果值是一个正则表达式，那么匹配这个正则的文件会被忽略
  - 如果传入的值是一个数组，那么数组里的值必须为正则

- `include`(Array or Regexp) 如果设置了

  ```
  include
  ```

  ，那将只有匹配到的文件才会被转换，例如只转换 'src/mobile' 下的文件 (

  ```
  include: /\/src\/mobile\//
  ```

  )

  - 如果值是一个正则表达式，将包含匹配的文件，否则将排除该文件
  - 如果传入的值是一个数组，那么数组里的值必须为正则

- `landscape` (Boolean) 是否添加根据 `landscapeWidth` 生成的媒体查询条件 `@media (orientation: landscape)`

- `landscapeUnit` (String) 横屏时使用的单位

- `landscapeWidth` (Number) 横屏时使用的视口宽度

###  selectorBlackList属性的简巧用法

* 将需要忽视单位变化的组件添加一个calss----:ignore
* 而不是把需要忽视单位变化的组件的类名一个个添加到下面的属性数组中

```
selectorBlackList: ['ignore']
```

