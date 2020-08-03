---
title: vscode使用
date: 2020-07-28 21:06:17
tags:
- vscode
categories:
- [tools, vscode]
---

#  vscode使用

##  0 快捷键

* 缩进

向右：tab  or  ctrl+]

向左：shift+tab   or   ctrl+[

* ! 
  * 自动生成一套HTML代码
* tab
  * 增加选中行的缩进
* tab+shift
  * 减小选中行的缩进
* alt + b
  * 快速在浏览器中浏览网页
* ctrl + c
  * 复制
* ctrl + x
  * 剪切
* ctrl + v
  * 粘贴
* ctrl + .
  * 让输入法在中英文标点符号之间来回进行切换
* ctrl + /
  * 在代码当中快速打出注释

* alt+shift+a
  * 多行注释

* ctrl+f
  * 在文件中查找



##  1 插件配置

###  1.1 基础插件配置

* 汉化

```
搜索chinese
Chinese (Simplified) Language Pack for Visual Studio Code
```

*  Atom One Dark Theme   一种主题风格

```
Atom One Dark Theme
```

* KoroFileHeader  ---  **函数头部解释快捷键**
  * 光标处添加函数注释  

    1.Windows：ctrl+alt+t
    2.mac:ctrl+cmd+t

  * 还可以给文件加描述
    1.Windows：ctrl+alt+i
    2.mac:ctrl+cmd+i







###  1.2  前端开发插件配置

*   vscode-icons    目录图标

```
vscode-icons
```

*  open in browser    方便打开html文件

```
open in browser  
```

###  1.3 react 开发相关插件

*  https://segmentfault.com/a/1190000019928571 
*  https://blog.csdn.net/qq_34586870/article/details/100079410 
*  Reactjs code snippets	react代码段
*  React Redux ES6 Snippets	代码段
*  ES7 React/Redux/GraphQL/React-Native snippets	代码段
*  JavaScript (ES6) code snippets	es6代码段
*  Auto Rename Tag	从名字就能看出，当你需要更改标签的时候，你改了第一个就会自动更改第二个
*  Document This	文档注释 快捷键Ctrl +Alt + D两次
*  Image preview	在代码行号前面显示图片预览
*  Path Intellisense	路径补全
*  vscode-icons	文件图标
*  npm Intellisense	对package.json内中的依赖包的名称提示
*  Bracket Pair Colorizer	用彩虹色标注括号
*  Settings Sync	将配置同步到github，更换开发环境的时候就不用重复配置了
*  Todo Tree	待办事项高亮，知乎配置介绍
   



##  2 设置

###  2.1 缩进设置

*  file-->preferenses-->setting   (文件-首选项-设置)
* 搜索`tabsize`

 ![img](vscode/8140656-62cf5ba118c0f093.webp) 



*  要是设置了之后发现没有生效的话，那么就继续在搜索框那边搜`editor.detectIndentation` ，将这一项修改为false即可 

 ![img](vscode/8140656-cf268cb03b01e407.webp) 



##  附录

###  1 vscode前端最佳配置

>[2020 vscode 前端最佳配置]( https://blog.csdn.net/win7583362/article/details/79315055/ )

