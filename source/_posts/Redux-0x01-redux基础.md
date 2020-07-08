---
title: Redux(0x01)-redux基础
date: 2020-07-08 20:52:05
tags:
- react
- redux
categories:
- [react, redux]
---

#   Redux基础

##  1 Redux简介

* **[Redux官方文档]( http://cn.redux.js.org/ )**

* 安装

  ```
  npm install --save redux
  ```

  

![1594003759128](Redux-0x01-redux基础/1594003759128.png)





##  2 Redux 使用流程

![1594004464974](Redux-0x01-redux基础/1594004464974.png)

1. 新建`store`和`reducer` 目录结构如下：

   * src (项目的源文件夹)
     * store  (用于管理全局数据的文件夹)
       * index.js (定义store)
       * reducer.js (定义reducer)

2. 编辑`index.js`

   ```js
   import { createStore } from 'redux'
   import reducer from './reducer'
   
   const store  = createStore(reducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
   
   export default store
   ```

3. 编辑`reducer.js`

   * state:  修改前的全局数据
     * state = defaultState  为全局数据设置默认值
   * action: 组件传来的需要修改的数据

   ```js
   const defaultState = {
       //在此处自定义全局数据的默认值
   }
   
   export default (state = defaultState, action) => {
     return state
   }
   ```

4. 组件中操作全局数据

* 4.1 组件中引入全局数据仓库

  ```js
  import store from './store/index'
  ```

* 4.2 组件中获取全局数据

  ```
  store.getState()
  ```

* 4.3 组件中修改全局数据

  * 通过`action`来间接修改  ----  大项目一般使用[`actionCreaters`](#4-actioncreators)来间接修改
  * 组件中定义的`action`本质上只是告诉`store`要做何修改，修改的数据什么，真正的修改是在`reducer`中进行的
  * **通过`store.dispatch(action)`将`action`提交给`store`处理**

  ```js
  const action = {
    type: '修改操作简称',  //在reducer中对比这个简称 做出相应的修改操作
    属性:  值     //设置此次修改的数据名（属性）、数据值 
  }
  store.dispatch(action)
  
  //案例
  const action = {
    type: 'change_input_value',
    value: 'hello'
  }
  store.dispatch(action)
  ```

* 4.4 在`reducer`根据`action`修改全局数据

  * `reducer`中其实也不能修改`store`的数据，所以需要**深拷贝**原来的数据，然后修改拷贝来的数据，将修改后的数据返回给`store`，`store`中再做数据更新
  * **`reducer`必须是纯函数**
    * 给定确定的输入就一定会有确定的输出，给定输入：state、action  则确定输出：newState || state
      * 函数中不能有网络请求、时间相关操作（new Date()  setTimeout等）
    * 不能有副作用，只是修改newState  不要有其它操作

  ```js
  const defaultState = {
    inputValue: '1232131',
  }
  
  export default (state = defaultState, action) => {
    if(action.type === 'change_input_value'){  //判断是什么操作（与组件定义一致）
      const newState = JSON.parse(JSON.stringify(state))  //深拷贝
      newState.inputValue = action.value  // 修改数据
      return newState  //返回给了stroe
    }
    return state
  }
  ```

* 4.5 组件中监听`store`中数据的变化，并实时获取，刷新组件中的数据

  * `store.subscribe()`: 当`store`中的数据发生变化时，该函数就会执行
  * `handleStoreChange` : 自定义函数，当`store.subscribe()`执行时 在该自定义函数中获取`store`更新后的数据

  ```js
  store.subscribe(this.handleStoreChange)
  
   handleStoreChange() {
      this.setState(store.getState())  // store.getState()获取store数据 
    }
  ```

  

##  3 action types的拆分

**使用流程：**

1. 在`src/store`文件夹下新建`actionTypes.js`

   * 将所有`action`的`type`定义成常量并导出

   ```js
   //案例
   export const CHANGE_INPUT_VALUE = 'change_input_value'
   export const ADD_TO_LIST = 'add_to_list'
   export const DEL_LIST = 'del_list'
   ```

2. 在`reducer.js`、`actionCreators.js`等文件文件中使用 需要导入

   ```js
   //导入
   import {CHANGE_INPUT_VALUE, ADD_TO_LIST, DEL_LIST} from './actionTypes'
   
   ```

**拆分的好处：**

* 当`type`用常量时，如果拼写错误，编译时会报错，便于快速定位BUG
* 如果`type`使用的是字符串，当字符串拼写错误时，编译时不会报拼写的错误，很能快速定位BUG



##  4 actionCreators

**`actionCreators`用于管理`action`，避免`action`在组件中直接定义**

**使用流程：**

1. 在`src/store`下新建`actionCreators.js`

   * 在文件中定义好`action`，实例化`action`时调用相应的函数并传入参数见即可

   ```js
   //案例
   import {CHANGE_INPUT_VALUE, ADD_TO_LIST, DEL_LIST} from './actionTypes'
   
   export const getInputChangeAction = (value) => ({
     type: CHANGE_INPUT_VALUE,
     value
   })
   
   export const getAddItemAction = () => ({
     type: ADD_TO_LIST
   })
   
   export const getDelItemAction = (index) => ({
     type: DEL_LIST,
     index
   })
   ```

2. 在组件中使用

   ```js
   import {getInputChangeAction, getAddItemAction, getDelItemAction} from './store/actionCreators'
   
   //e.target.value是传入的参数
   changeInputValue(e){
       const action = getInputChangeAction(e.target.value)
       store.dispatch(action)
   }
   ```

