---
title: React(0x05)-react中iconfont的使用
date: 2020-05-15 12:17:35
tags:
- react
- iconfont
categories:
- [react, react]
---

#  React中iconfont的使用

**使用流程：**

1. 在`cionfont`官网-我的项目将图标下载至本地

2. 在下载的文件中选项一下后缀的文件， 并复制到项目中相应的资源文件夹中

   ```js
   iconfont.css
   iconfont.eot
   iconfont.svg
   iconfont.ttf
   iconfontwoff
   ```

3. 修改`iconfont.css`

   * 将文件后缀改为：`.js`

   * url前加`./`  以data开头的url不用加`./`
   * 删除`:before`的部分
   * 引入`Styled-Components`将样式设置为全局样式
     * 将修改后的文件在`App`组件中引入，使全局样式生效

   **初始文件：**

   ```js
   //iconfont.css
   @font-face {font-family: "iconfont";
     src: url('iconfont.eot?t=1594284017934'); /* IE9 */
     src: url('iconfont.eot?t=1594284017934#iefix') format('embedded-opentype'), /* IE6-IE8 */
     url('data:application/x-font-woff2;charset=utf-8;base64,d09GMg...') format('woff2'),
     url('iconfont.woff?t=1594284017934') format('woff'),
     url('iconfont.ttf?t=1594284017934') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
     url('iconfont.svg?t=1594284017934#iconfont') format('svg'); /* iOS 4.1- */
   }
   
   .iconfont {
     font-family: "iconfont" !important;
     font-size: 16px;
     font-style: normal;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
   }
   
   .iconfangdajing:before {
     content: "\e614";
   }
   
   .iconAa:before {
     content: "\e636";
   }
   
   .iconPensyumaobi:before {
     content: "\e708";
   }
   
   
   ```

   **修改后的文件：**

   ```js
   import { createGlobalStyle } from 'styled-components'
   
   export const Globalstyle = createGlobalStyle`
     @font-face {font-family: "iconfont";
     src: url('./iconfont.eot?t=1594284017934'); /* IE9 */
     src: url('./iconfont.eot?t=1594284017934#iefix') format('embedded-opentype'), /* IE6-IE8 */
     url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAA....') format('woff2'),
     url('./iconfont.woff?t=1594284017934') format('woff'),
     url('./iconfont.ttf?t=1594284017934') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
     url('./iconfont.svg?t=1594284017934#iconfont') format('svg'); /* iOS 4.1- */
     }
   
     .iconfont {
     font-family: "iconfont" !important;
     font-size: 16px;
     font-style: normal;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     }
   `
   ```

4. 在项目中使用`iconfont`

   * 在需要使用的位置使用一下语法使用
   * icon标识: 在iconfont官网-我的项目-图标下方可以看到  

   ```html
   <i calssName="iconfont">icon标识;</i>
   ```

   