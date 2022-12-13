---
title: koa
date: 2020-03-02 10:44:17
tags:
- koa
categories:
- [node, koa]
---



#  一、koa基础

##  1 koa简介

> * **`koa`是基于`node`环境的后端开发框架**
>
> * [koa官网]( https://koa.bootcss.com/ )

**安装:**

```js
//前提先安装node.js
npm install koa
```

* bug 

```js
//问题（终端）：
npm install出现Unexpected end of JSON input while parsing near错误

//解决方案  终端输入命令：
npm cache clean --force
```

**技术：**

* Node.js 
  * Node.js不能重复安装  若计算机上已有老版本 安装新版本之前要把老版本先卸载  或者 用nvm管理两个不同的版本
  * Node.js 的本质是让JS 可以脱离浏览器运行  而非是为了web开发 web开发只是其能力的一个方面
  * Node.js的能力与应用
    * 脱离浏览器运行JS
    * NodeJS Stream（前端工程化基础）------开源的、框架级别的产品 
    * 服务端API
    * 作为中间层 ------------ 中大型项目中才有中间层
  * Node.js 对ES6-10支持情况  
    * 目前Node.js  对ES6-10的一些特性是不支持的：1、import from导入方式不支持  需要用require  2、decorator装饰器 不支持  3、class类的属性不支持  必须用this.x = x  设置属性   （相对其他语言没有属性这一说  必须在构造函数中设置属性）
* npm:Node.js中的一个工具包
* koa：基于Node.js 专业开发web的框架
  * koa特点：洋葱圈模型  精简   使用时需要二次开发，否则会很难用

##  2 koa使用基础及流程

###  2.1 入口文件

> 在项目根目录下新建入口文件   一般为app.js (文件名自定)   在入口文件中导入koa、new koa、设置端口

* 一般称koa对象为  应用程序对象
* 以下3行代码即可启动koa框架
* 前端发送请求地址：localhost：3000（本地时）

```js
const koa = requrie('koa')
const app = new koa()
app.listen(3000)  //该函数 接收端口号 启动koa框架
```



###  2.2 koa中间件

* 理解koa中间件：
  * koa中间件本质就是函数，当前端向服务端发送请求时，由这些函数接受请求、处理并返回结果
  * koa中间件是按代码顺序执行的，在前面的中间件就先执行，需要执行后面的中间件就在前面的中间件中调用next()函数

* 注册中间件：
  * 通过`app.use()`方法，传入匿名函数，将该函数注册为中间件
  * koa中间件返回的总是一个promise对象
    * async await是标配， 确保返回的一定是promise对象，维持洋葱模型
  * 参数：
    * ctx:  上下文
    * next:  next()函数用于调用下一个函数(简单的指中间件代码顺序上的下一个中间件)

```js
//app.js
const koa = requrie('koa')
const app = new koa()

//ctx:上下文  next:下一个中间件
app.use(async(ctx,next)=>{
	console.log('中间件1')
	await next()    //-----调用下方的’中间件2‘  若在此处接收next() 一定是一个promise对象
    //const res = await next()   //也可以接受下一个函数（中间件）返回的结果
})

app.use(async (ctx,next)=>{
	console.log('中间件2')
})

app.listen(3000)  //该函数 接收端口号 启动koa框架
```



###  2.3 koa-router

* 安装

  ```js
  npm install koa-router
  ```

* 使用

  > [koa-router文档](https://github.com/koajs/router/blob/HEAD/API.md)
  >
  > [koa-router官网](https://www.npmjs.com/package/koa-router)

  ```js
  const Router = require('koa-router')
  const router = new Router()
  
  // 路由传递两个参数：第一个：路由地址  第二个：匿名函数，即满足方法和路由要求后的处理函数，就是中间件
  router.get('/classic/latest', async (ctx, next) => {
    ctx.body = {key : 'chuckie'}
  })
  
  app.use(router.routes())//将所有路由的匿名函数注册为中间件
  ```

* 理解koa-router:

  * 是对处理网络请求中间件的封装，根据不同的请求方法、请求地址，通过匿名函数处理并返回结果
  * 定义后需要注册成中间件：`app.use(router.routes())`



##  3 koa获取请求参数

```js
router.get('/v1/:id/classic/latest', async (ctx, next) => {
  const path = ctx.params
  const query = ctx.request.query
  const headers = ctx.request.header
  const body = ctx.request.body
})

```

1. 路劲参数获取

   * koa中定义：`'/v1/:id/classic/latest'`
     * 其中`/:id`即为路劲中传来的参数
   * 获取：`const path = ctx.params`

2. `?param`参数

   * koa中不用定义
   * 前端请求样式：`/v1/classic/latest?param=maxthon`
   * 获取：`const query = ctx.request.query`

3. `headers`参数

   * 获取：`const headers = ctx.request.header`

4. `body`参数

   * 前提：**安装`require-directory`第三方库**

     * 1 安装：`npm i require-directory`

     * 2 `app.js`文件中注册：

       ```js
       const Koa = require('koa')
       const parser = require('koa-bodyparser')
       
       const app = new Koa()
       
       app.use(parser()) //该库返回结果本质是一个中间价，所以需要注册
       
       app.listen(3000)
       ```

   * 获取：`const body = ctx.request.body`



#  二、项目框架

> **[koa开发基础框架模板Github地址](https://github.com/ChuckieWill/node-server-koa2)**

##  1 全局异常处理中间件

> **全局异常处理原理：**
>
> * 步骤一中的`catchError`中间件，注册在最前面，当后面的中间件都执行完后如果有异常抛出都会在这里被捕捉到

###   1.1 目录结构

* middlewares
  * exception.js    全局异常处理中间件
* core
  * http-exception.js    http错误类
* app.js        注册全局异常处理中间件
* config
  * config.js   配置信息-环境变量的配置

###  1.2 使用前提

* 自定义http错误类

  * 参考http错误类源码

  ```js
  const {HttpException} = require('../core/http-exception')
  ```

###  1.3 源码

1. 在根目录下新建`middlewares`文件夹，文件夹下新建`exception.js`文件

   * 返回给前端的内容包括
     * msg: 错误信息
     * error_code: 自定义错误码
     * request:  发生错误的请求地址
     * status:  http状态码

   ```js
   const {HttpException} = require('../core/http-exception')
   
   //全局错误处理中间件
   const catchError = async (ctx, next) => {
     try {
       await next()
     } catch (error) {
       //判断为开发环境则抛出异常，便于终端查看错误原因------环境变量判断-------
       if(global.config.environment === 'dev'){
         throw error
       }
       //判断为已知错误，抛出的错误是封装的http错误类的实例
       if(error instanceof HttpException){
         //已知错误的处理
         ctx.body = {
           msg: error.msg,  //错误描述
           error_code: error.errorCode,  //自定义错误码
           request: `${ctx.method} ${ctx.path}`   //发送错误的请求地址
         }
         ctx.status = error.code  //http状态码
       }else{
         //未知错误的处理
         ctx.body = {
           msg: '服务器内部错误 未知错误',
           error_code: 99999,
           request: `${ctx.method} ${ctx.path}`
         }
         ctx.status = 500
       }
     }
   }
   
   module.exports = catchError
   ```

2. `app.js`中注册中间件

   ```js
   const Koa = require('koa')
   const catchError = require('./middlewares/exception')
   
   
   const app = new Koa()
   app.use(catchError)//注册中间件
   
   app.listen(3000)
   ```





##  2 http错误类

> **应用场景：**
>
> * 每次有异常或错误抛出时，直接实例化http错误类即可，更加方便，**抛出的错误和异常也更加规范**

###  2.1 目录结构

* core
  * http-exception.js

###  2.2 源码

* 根据具体业务的要求可以继续添加特定的错误类
* 使用时，导入并实例化错误类，然后抛出错误即可

```js
//统一封装的http错误类
class HttpException extends Error{
  constructor(errorCode=10000,code=400, msg='服务器异常'){
    super()
    this.errorCode = errorCode //项目自定义错误码
    this.code = code //http状态码
    this.msg = msg   //错误描述
  }
}

//参数错误类，一般在参数校验错误时使用
class ParameterException extends HttpException{
  constructor(msg, errorCode){
    super()
    this.code = 400
    this.msg = msg || '参数错误'
    this.errorCode = errorCode || 10000
  }
}

module.exports = {HttpException,ParameterException}
```



##  3 koa路由框架

###  3.1 目录结构

> api版本管理：
>
> * 通过v1、v2、v3、、、、、方式表示不同版本的路由
> * URL也通过`/v1/,,,`、`/v2/,,,,`来区分不同版本

* app
  * api
    * v1  版本1的路由
      * user.js   相关用户的路由（案例）
    * v2  版本2的路由
  * models
    * user.js  相关用户的操作数据库的数据模型（案例）
  * validators
    * validator.js  定义参数校验类

###  3.2 使用前提

* 安装`koa-router`

  ```js
  npm i koa-router
  ```

* 参数校验器
* 数据模型操作数据库

###  3.3 源码

**路由核心步骤：**

* 1 参数校验 ，依赖参数校验器
* 2 对数据库数据进行增删改查，依赖实例化的数据模型（sequelize）
* 3 将对数据库的操作结果整理后返回前端

```js
const Router = require('koa-router')
const {RegisterValidater} = require('../../validators/validator')
const { User } = require('../../models/user')
const router = new Router({
  prefix: '/v1/user'//设置路由的基地址 
})

//用户注册路由   
//拼接后的路由地址为：/v1/user/register
router.post('/register', async (ctx) => {
  //1 参数校验
  const v = await new RegisterValidater().validate(ctx)
  //2 获取并整理参数
  const user = {
    nickname : v.get('body.nickname'),
    password : v.get('body.password2'),
    email : v.get('body.email')
  }
  //3 用sequelize实例化的模型将数据插入数据库
  await User.create(user)
  //4 返回前端操作成功
  throw new global.errs.Success() 
})

module.exports = router
```





##  4 校验器(参数校验)

> 封装了[LinValidator](https://doc.cms.talelin.com/server/koa/validator.html)校验类，方便参数的校验
>
> `LinValidator`校验类整合了第三方库[validator.js](https://github.com/validatorjs/validator.js)
>
> `LinValidator`校验类使用教程查看->[LinValidator使用教程](https://doc.cms.talelin.com/server/koa/validator.html)
>
> `LinValidator`中`Rule`参数中第一个参数(校验函数)可查看[`validator.js官方文档`](https://github.com/validatorjs/validator.js)

###  4.1 目录结构

* core
  * lin-validator.js      封装的LinValidator类
  * utils.js        
  * http-exception.js   封装的http错误类
* app
  * validators
    * validator.js          继承LinValidator类，自定义具体业务的校验类

###  4.2 使用前提

* 安装`validator`

  ```js
  npm install validator
  ```

* 安装`jsonwebtoken`

  ```js
  npm i jsonwebtoken
  ```

* 安装`lodash`   

  *  [lodash官方文档](https://www.lodashjs.com/docs/latest)
  * 便于获取http请求中的参数，具体使用参考[LinValidator使用教程](https://doc.cms.talelin.com/server/koa/validator.html)

  ```js
  npm i lodash
  ```

* 自定义参数错误http错误类

  * 参考http错误类源码
  
  ```js
  const {ParameterException} = require('./http-exception')
  ```

###  4.3 源码

####  4.3.1 `lin-validator.js`

```js
//lin-validator.js

const validator = require('validator')//第三方库 ，需要安装
const {ParameterException} = require('./http-exception')
const {get,last,set,cloneDeep} = require("lodash")  //第三方库 ，需要安装
const {findMembers} = require('./utils')


class LinValidator {
    constructor() {
        this.data = {}
        this.parsed = {}
    }


    _assembleAllParams(ctx) {
        return {
            body: ctx.request.body,
            query: ctx.request.query,
            path: ctx.params,
            header: ctx.request.header
        }
    }

    get(path, parsed = true) {
        if (parsed) {
            const value = get(this.parsed, path, null)
            if (value == null) {
                const keys = path.split('.')
                const key = last(keys)
                return get(this.parsed.default, key)
            }
            return value
        } else {
            return get(this.data, path)
        }
    }

    _findMembersFilter(key) {
        if (/validate([A-Z])\w+/g.test(key)) {
            return true
        }
        if (this[key] instanceof Array) {
            this[key].forEach(value => {
                const isRuleType = value instanceof Rule
                if (!isRuleType) {
                    throw new Error('验证数组必须全部为Rule类型')
                }
            })
            return true
        }
        return false
    }

    async validate(ctx, alias = {}) {
        this.alias = alias
        let params = this._assembleAllParams(ctx)
        this.data = cloneDeep(params)
        this.parsed = cloneDeep(params)

        const memberKeys = findMembers(this, {
            filter: this._findMembersFilter.bind(this)
        })

        const errorMsgs = []
        // const map = new Map(memberKeys)
        for (let key of memberKeys) {
            const result = await this._check(key, alias)
            if (!result.success) {
                errorMsgs.push(result.msg)
            }
        }
        if (errorMsgs.length != 0) {
            throw new ParameterException(errorMsgs)
        }
        ctx.v = this
        return this
    }

    async _check(key, alias = {}) {
        const isCustomFunc = typeof (this[key]) == 'function' ? true : false
        let result;
        if (isCustomFunc) {
            try {
                await this[key](this.data)
                result = new RuleResult(true)
            } catch (error) {
                result = new RuleResult(false, error.msg || error.message || '参数错误')
            }
            // 函数验证
        } else {
            // 属性验证, 数组，内有一组Rule
            const rules = this[key]
            const ruleField = new RuleField(rules)
            // 别名替换
            key = alias[key] ? alias[key] : key
            const param = this._findParam(key)

            result = ruleField.validate(param.value)

            if (result.pass) {
                // 如果参数路径不存在，往往是因为用户传了空值，而又设置了默认值
                if (param.path.length == 0) {
                    set(this.parsed, ['default', key], result.legalValue)
                } else {
                    set(this.parsed, param.path, result.legalValue)
                }
            }
        }
        if (!result.pass) {
            const msg = `${isCustomFunc ? '' : key}${result.msg}`
            return {
                msg: msg,
                success: false
            }
        }
        return {
            msg: 'ok',
            success: true
        }
    }

    _findParam(key) {
        let value
        value = get(this.data, ['query', key])
        if (value) {
            return {
                value,
                path: ['query', key]
            }
        }
        value = get(this.data, ['body', key])
        if (value) {
            return {
                value,
                path: ['body', key]
            }
        }
        value = get(this.data, ['path', key])
        if (value) {
            return {
                value,
                path: ['path', key]
            }
        }
        value = get(this.data, ['header', key])
        if (value) {
            return {
                value,
                path: ['header', key]
            }
        }
        return {
            value: null,
            path: []
        }
    }
}

class RuleResult {
    constructor(pass, msg = '') {
        Object.assign(this, {
            pass,
            msg
        })
    }
}

class RuleFieldResult extends RuleResult {
    constructor(pass, msg = '', legalValue = null) {
        super(pass, msg)
        this.legalValue = legalValue
    }
}

class Rule {
    constructor(name, msg, ...params) {
        Object.assign(this, {
            name,
            msg,
            params
        })
    }

    validate(field) {
        if (this.name == 'isOptional')
            return new RuleResult(true)
        if (!validator[this.name](field + '', ...this.params)) {
            return new RuleResult(false, this.msg || this.message || '参数错误')
        }
        return new RuleResult(true, '')
    }
}

class RuleField {
    constructor(rules) {
        this.rules = rules
    }

    validate(field) {
        if (field == null) {
            // 如果字段为空
            const allowEmpty = this._allowEmpty()
            const defaultValue = this._hasDefault()
            if (allowEmpty) {
                return new RuleFieldResult(true, '', defaultValue)
            } else {
                return new RuleFieldResult(false, '字段是必填参数')
            }
        }

        const filedResult = new RuleFieldResult(false)
        for (let rule of this.rules) {
            let result = rule.validate(field)
            if (!result.pass) {
                filedResult.msg = result.msg
                filedResult.legalValue = null
                // 一旦一条校验规则不通过，则立即终止这个字段的验证
                return filedResult
            }
        }
        return new RuleFieldResult(true, '', this._convert(field))
    }

    //数据类型转换
    _convert(value) {
        for (let rule of this.rules) {
            if (rule.name == 'isInt') {
                return parseInt(value)
            }
            if (rule.name == 'isFloat') {
                return parseFloat(value)
            }
            if (rule.name == 'isBoolean') {
                return value ? true : false
            }
        }
        return value
    }

    _allowEmpty() {
        for (let rule of this.rules) {
            if (rule.name == 'isOptional') {
                return true
            }
        }
        return false
    }

    _hasDefault() {
        for (let rule of this.rules) {
            const defaultValue = rule.params[0]
            if (rule.name == 'isOptional') {
                return defaultValue
            }
        }
    }
}



module.exports = {
    Rule,
    LinValidator
}
```

####  4.3.2 `utils.js`

```js
const jwt = require('jsonwebtoken')//第三方库 ，需要安装
/***
 * 
 */
const findMembers = function (instance, {
    prefix,
    specifiedType,
    filter
}) {
    // 递归函数
    function _find(instance) {
        //基线条件（跳出递归）
        if (instance.__proto__ === null)
            return []

        let names = Reflect.ownKeys(instance)
        names = names.filter((name) => {
            // 过滤掉不满足条件的属性或方法名
            return _shouldKeep(name)
        })

        return [...names, ..._find(instance.__proto__)]
    }

    function _shouldKeep(value) {
        if (filter) {
            if (filter(value)) {
                return true
            }
        }
        if (prefix)
            if (value.startsWith(prefix))
                return true
        if (specifiedType)
            if (instance[value] instanceof specifiedType)
                return true
    }

    return _find(instance)
}


//生成令牌
const generateToken = function (uid, scope) {
    const secretKey = global.config.security.secretKey
    const expiresIn = global.config.security.expiresIn
    const token = jwt.sign({
        uid,//用户id
        scope  //用户权限
    }, secretKey, {
        expiresIn: expiresIn
    })
    return token
}

module.exports = {
    findMembers,
    generateToken,
}
```

####  4.3.3 `http-exception.js`

```js
//统一封装的http错误类
class HttpException extends Error{
  constructor(errorCode=10000,code=400, msg='服务器异常'){
    super()
    this.errorCode = errorCode //项目自定义错误码
    this.code = code //http状态码
    this.msg = msg   //错误描述
  }
}

//参数错误类，一般在参数校验错误时使用
class ParameterException extends HttpException{
  constructor(msg, errorCode){
    super()
    this.code = 400
    this.msg = msg || '参数错误'
    this.errorCode = errorCode || 10000
  }
}

module.exports = {HttpException}
```



##  5 Sequelize+MySQL

> 关于MySQL的一些使用可以查看**本站分类：tools/MySQL文章**
>
> 使用**Sequelize** (一个基于 promise 的 Node.js  ORM）
>
> * [Sequelize官方文档](https://www.sequelize.com.cn/)

###  5.1 目录结构

> 调用关系：
>
> * api/v1/user.js中使用实例化的sequelize实现对数据库的操作，依赖app/models/user.js
> * app/models/user.js中定义数据表的字段，以及操作数据的业务流程，依赖core/db.js
> * core/db.js 中建立数据库连接，依赖config/config.js
> * config/config.js中配置了建立数据库连接相应的信息

* core
  * db.js   建立数据库连接
* config
  * config.js   配置信息-数据库基本信息
* app
  * models  数据模型
    * user.js  通过模型在数据库自动生成数据表
  * api
    * v1
      * user.js  对数据库中数据进行增删改查

###  5.2 使用前提

* 安装`sequelize`

  ```js
  npm i sequelize
  ```

* 安装驱动

  ```js
  # 选择以下之一:
  $ npm install --save pg pg-hstore # Postgres
  $ npm install --save mysql2    //使用mysql时安装
  $ npm install --save mariadb
  $ npm install --save sqlite3
  $ npm install --save tedious # Microsoft SQL Server
  ```


###  5.3 源码

####   5.3.1 `config.js`

```js
module.exports = {
  //数据库配置信息
  database : {
    dbName:'blink-backend',//数据库名称
    user: 'root',//数据库用户名
    password: '123456',//数据库用户密码
    host: 'localhost',//数据库主机名 
    port:3306,//数据库端口
  }
}
```

####  5.3.2 `db.js`

* 更多参数可以查看---->[官方API](https://sequelize.org/master/class/lib/sequelize.js~Sequelize.html#instance-constructor-constructor)
* **注意：`timezone: '+08:00'`**
  * 一定为'+08:00'，才能以北京的时间来记录所有的时间

```js
const { Model } = require('sequelize')
const Sequelize = require('sequelize')
const {unset, clone, isArray} = require('lodash')
const {database} = require('../config/config')//导入数据库配置信息

//参数1：数据库名称  参数2：数据库用户名  参数3：数据库用户密码  参数4：对象（包含多个参数）
const sequelize = new Sequelize(database.dbName, database.user, database.password, {
  host: database.host,//数据库主机名
  port: database.port,//数据库端口号
  dialect: 'mysql' ,/* 选择 'mysql' | 'mariadb' | 'postgres' | 'mssql' 其一 */   //数据库类型
  logging: true,//true终端显示数据库操作的sql语句，false则不显示
  timezone: '+08:00',//-----一定为'+08:00'，才能以北京的时间来记录所有的时间------
  define: {
    timestamps:true,//false:建表时不自动生成createAt,updateAt字段;true建表时自动生成，，，
    paranoid: true,//true：自动生成delete_time字段；false：不生成
    createdAt: 'created_at',//重命名自动生成的字段，命名成更符合数据标准的字符形式
    updatedAt: 'updated_at',
    deletedAt: 'deleted_at',
    underscored: true,//将所有的驼峰命名改成下划线命名
    scopes: {//自定义查询语句，
      bh : {//bh为自定义名称
        attributes: {//查询时调用scope('bh')，则可以在查询时排除以下字段，即查询结果不包含一下字段
          exclude : ['updated_at', 'deleted_at', 'created_at']
        }
      }
    }
  }
});

sequelize.sync({
  force:false //数据库字段更改后自动更新，原理是删除原来的表单（包括数据），再新建表单，开发阶段可使用，上线后不可使用
})

//JSON序列化
Model.prototype.toJSON = function(){
  //浅拷贝 this.dataValues可以拿到模型上所有属性
  let data = clone(this.dataValues)
  //lodash-unset,移除对象属性
  unset(data,'updated_at')
  unset(data,'created_at')
  unset(data,'deleted_at')

  // exclude 模型上要删除的字段 提供模型实例自定义需要删除的字段
  //lodash-isArray,判断是否为数组
  if(isArray(this.exclude)){
    //遍历数组
    this.exclude.forEach((value)=>{
      //移除需要自定义需要删除的字段
      unset(data,value)
    })
  }

  return data
}

module.exports = {
  sequelize
}
```

####  5.3.3 `app/models/user.js`

**模型功能：**

* 定义数据库中表单各字段
* 自定义相关数据库操作的函数（也包括一部分业务逻辑）

```js
const bcrypt = require('bcrypt')//密码加密库

const {
  sequelize
} = require('../../core/db') //导入实例化的sequelize

const {
  Sequelize,
  Model
} = require('sequelize') //导入原生类sequelize,模型Model

//在数据库中通过user模型新建表单
class User extends Model {
  //相关数据库操作的自定义函数
  //登录校验 判断用户名是否存在，密码是否正确
  static async verifyEmailPassword(email, plainPassword){
    const user = await User.findOne({
      where: {
        email
      }
    })
    if(!user){
      throw new global.errs.AuthFailed('用户名不存在')
    }
    //对比输入密码与数据库中密码是否一致
    const res = bcrypt.compareSync(plainPassword, user.password)
    if(!res){
      throw new global.errs.AuthFailed('密码错误')
    }
    return user
  }
}

//定义表单各字段
User.init({
  id: {
    type: Sequelize.INTEGER, //设置属性数据类型为字符串
    primaryKey: true, //设置该属性为主键
    autoIncrement: true //数据库自动的生成自增长的id编号，添加用户时不用传入该属性值
  },
  nickname: Sequelize.STRING, //设置属性数据类型为字符串
  email: {
    type: Sequelize.STRING(128), //设置属性数据类型为字符串，且最多64个字符
    unique: true //设置属性值唯一，不可重复
  },
  password: {
    type: Sequelize.STRING,
    //观察password的变化，每次有变化则调用下面的函数
    //对password进行加密操作
    set(val){
      const salt = bcrypt.genSaltSync(10)//传入数字越大则密码加密越难，成本消耗更大，一般设置为10
      const psw = bcrypt.hashSync(val,salt)
      this.setDataValue('password',psw)
    }
  },
  openid: {
    type: Sequelize.STRING(64), //设置属性数据类型为字符串，且最多64个字符
    unique: true //设置属性值唯一，不可重复
  }
}, {
  sequelize: sequelize,//传入实例化的sequelize,用于建立数据库连接
  tableName:'user'//定义表单的名称
}) 

module.exports = {
  User
}
```

####  5.3.4 `app/api/v1/user.js`

* 在路由中通过实例化的sequelize数据模型对数据进行增删改查，下面例子为插入数据库操作
* 增删改查语法请查看：[sequelize官方文档](https://www.sequelize.com.cn/core-concepts/model-querying-basics)

```js
const Router = require('koa-router')
const {RegisterValidater} = require('../../validators/validator')
const { User } = require('../../models/user')
const router = new Router({
  prefix: '/v1/user'//设置路由的基地址 
})

//用户注册路由   
//拼接后的路由地址为：/v1/user/register
router.post('/register', async (ctx) => {
  //1 参数校验
  const v = await new RegisterValidater().validate(ctx)
  //2 获取并整理参数
  const user = {
    nickname : v.get('body.nickname'),
    password : v.get('body.password2'),
    email : v.get('body.email')
  }
  //3 用sequelize实例化的模型将数据插入数据库
  await User.create(user)
  //4 返回前端操作成功
  throw new global.errs.Success() 
})

module.exports = router
```



##  6 注册及登录模块

> 此部分源码只列出了入口部分，依赖的其他源码可查看[本项目github上的源码](https://github.com/ChuckieWill/node-server-koa2)

###  6.1 目录结构

* app
  * api 
    * v1
      * user.js  用户注册（邮箱注册方式）
      * token.js 用户登录（包括小程序、web等多种登录方式，本质是身份验证并返回token）
  * validators
    * validator.js 参数校验（注册和登录参数的校验）
  * models
    * user.js 操作数据库的用户模型（操作数据库的相关业务逻辑写在这里）
  * lib
    * enum.js 枚举，定义一些常量（用户类型、用户权限）
  * services
    * wx.js   相关微信小程序登录的业务逻辑（app/models/user.js用户模型的上一层）
* core
  * utils.js 封装生成token的函数
* middlewares
  * auth.js   token令牌验证

###  6.2 用户注册

> 这里的注册只针对于邮箱注册

**注册流程：**

1. 参数校验 （校验器源码：app/validators/validator.js - RegisterValidater）
   * 参数要求：
     * nickname :用户昵称
     * password : 用户密码
     * email :用户邮箱，唯一标识，不可重复
2. 写入数据库 （调用User模型：app/models/user.js）
3. 返回前端操作结果

**源码：**

```js
//app/api/v1/user.js
const Router = require('koa-router')
const {RegisterValidater} = require('../../validators/validator')
const { User } = require('../../models/user')
const router = new Router({
  prefix: '/v1/user'//设置路由的基地址 
})

//用户注册路由   
//拼接后的路由地址为：/v1/user/register
router.post('/register', async (ctx) => {
  //1 参数校验
  const v = await new RegisterValidater().validate(ctx)
  //2 获取并整理参数
  const user = {
    nickname : v.get('body.nickname'),
    password : v.get('body.password2'),
    email : v.get('body.email')
  }
  //3 用sequelize实例化的模型将数据插入数据库
  await User.create(user)
  //4 返回前端操作成功
  throw new global.errs.Success() 
})

module.exports = router
```

###  6.3 用户登录

> 登录的本质是验证身份合法性并获取token
>
> 登录包括多种方式：
>
> * 小程序用户登录
> * web邮箱登录



**登录路由源码：**

```js
//app/api/v1/token.js
const Router = require('koa-router')
const {
  TokenValidator,
  NotEmptyValidator
} = require('../../validators/validator') //登录参数校验
const {
  LoginType
} = require('../../lib/enum') //登录方式枚举
const {
  User
} = require('../../models/user') //用户模型，操作数据库
const {
  generateToken
} = require('../../../core/utils') //生成token令牌
const {
  Auth
} = require('../../../middlewares/auth') //用户不同角色权限校验
const {
  WXManager
} = require('../../services/wx') //微信小程序登录相关业务逻辑
const router = new Router({
  prefix: '/v1/token' //设置路由的基地址 
})

//登录获取token接口
router.post('/', async (ctx) => {
  let token
  //1 参数校验
  const v = await new TokenValidator().validate(ctx)
  //2 根据不同的登录方式进行登录校验
  switch (v.get('body.type')) {
    //--------------------------web邮箱登录-------------------------
    case LoginType.USER_EMAIL:
      token = await emailLogin(v.get('body.account'), v.get('body.secret'))
      break;
      //-----------------------微信小程序登录--------------------------
    case LoginType.USER_MINI_PROGRAM:
      token = await WXManager.codeToToken(v.get('body.account'))
      break;
    default:
      throw new global.errs.ParameterException('该登录方式暂没有相应的处理处理函数')
  }
  //3 将数据返回前端
  ctx.body = {
    token
  }
})

//--------------------------token校验接口(验证该token是否有效)--------------------
router.post('/verify', async (ctx) => {
  //1 参数验证
  const v = await new NotEmptyValidator().validate(ctx)
  //2 验证token是否有效
  const result = Auth.verifyToken(v.get('body.token'))
  //3 返回前端验证结果
  ctx.body = {
    result
  }
})

//web邮箱登录获取token
async function emailLogin(account, password) {
  const user = await User.verifyEmailPassword(account, password)
  const token = generateToken(user.id, Auth.USER)
  return token
}

module.exports = router
```



####  6.3.1 web邮箱登录

**登录流程：**

*需要先调用注册接口注册*

1. 参数校验 （校验器源码：app/validators/validator.js - TokenValidator）
   * 参数要求：
     * account: 账户即为邮箱
     * secret: 用户密码
     * type： 登录方式（101）
2. 身份验证 （调用User模型的方法： app/models/user.js）
   * 数据库中查找该用户是否存在
   * 用户名与密码是否匹配    (调用加密库`bcryp`t对加密的密码比对)
3. 生成token并返回前端  (调用：core/utils.js - generateToken)
   * 在token中保存用户id：uid 和用户权限角色：scope

####  6.3.2 小程序登录

**登录流程：**

*不需要注册，直接在小程序端发来js_code即可*

1. 参数校验 （校验器源码：app/validators/validator.js - TokenValidator）
   * 参数要求：
     * account: 账户即为js_code
     * secret: 不用传
     * type： 登录方式（100）
2. 获取openid (源码：app/services/wx.js)
   * 向微信发送请求获取openid,相当于微信帮忙做了用户身份验证，若返回openid则用户身份合法
3. 写入数据库 (源码：app/models/user.js)
   * 通过openid在数据中查找用户，若没有查到则写入数据库（相当于注册）
4. 生成token并返回前端  (调用：core/utils.js - generateToken)

###  6.4 token校验

> [jsonwebtoken官网](https://jwt.io/)

**校验流程：**

1. 参数校验 （校验器源码：app/validators/validator.js -NotEmptyValidator）
   * 参数:token
2. 验证token是否有效 （源码：middlewares/auth.js）
3. 返回前端验证结果

**验证意义：**调用的`jsonwebtoken`库验证的token, 只有token有效时（没过期，正确）才返回正确，否则返回false

###  6.5 用户权限验证

> 使用`basic-auth`库，解析前端通过base64加密的token

**验证包括：**

* 是否有token，token是否过期、是否合法
  * **要求前端以`http-basic-auth`方式给后端传递token**
* 用户角色是否有权限（普通用户、管理员、超级管理员）

**源码：**

```js
//middlewares/auth.js
//令牌验证
const basicAuth = require('basic-auth')//解析HpptBasicAuth方式传递的令牌
const jwt = require('jsonwebtoken')//令牌处理库
class Auth {
  //实例化类时传入具体API的访问级别（可访问的角色）
  constructor(level){
    this.level = level || 1//设置实例化的api权限要求
    //类变量-scope角色定义
    Auth.USER = 8 //普通用户
    Auth.ADMIN = 16 //管理员
    Auth.SUPER_ADMIN = 32 //超级管理员
  }
  //属性
  get m(){
    return async (ctx, next) => {
      //获取前端传来的token
      const userToken = basicAuth(ctx.req)//用basicAuth解析前端通过base64加密的token
      //token不存在则阻止访问
      if(!userToken || !userToken.name){
        throw new global.errs.Forbbiden('未传token')
      }
      try {
        //验证token是否合法
        var decode = jwt.verify(userToken.name, global.config.security.secretKey)
      } catch (error) {
        if(error.name == 'TokenExpiredError'){
          throw new global.errs.Forbbiden('token已过期')
        }
        throw new global.errs.Forbbiden('token不合法')
      }
      //权限判断（用户角色判断），
      if(decode.scope < this.level){
        throw new global.errs.Forbbiden('权限不够')
      }
      //将保存在jwt中的用户id:uid和scop取出
      //并保存在上下文auth中，便于随时使用
      ctx.auth = {
        uid: decode.uid,
        scope: decode.scope
      }
      await next() //执行具体业务逻辑的中间件
    }
  }
}

module.exports = {
  Auth
}
```

**使用：**

* 在具体路由中加入这个中间件，先作一层校验，一定是加在业务逻辑中间件前面
* `new Auth(LoginAuth.ADMIN).m`
  * LoginAuth.ADMIN: 传入可以访问的权限限制  (更多等级可以查看源码：app/lib/enum.js)

```js
const Router = require('koa-router')
const {Auth} = require('../../../middlewares/auth')
const {LoginAuth} = require('../../lib/enum')//可以访问该接口的权限枚举
const router = new Router({
  prefix: '/v1/classic'//设置路由的基地址 
})

// 注册中间件
router.get('/latest', new Auth(LoginAuth.ADMIN).m, async (ctx, next) => {
	//路由具体业务逻辑
})

module.exports = router
```

####  6.5.1 前端以`basic-auth`方式给后端传递token

> 用户在访问接口时需要进行用户权限验证，其中就包括token的验证，因此需要前端每次在请求接口时携带上token,本项目统一规定以`basic-auth`的方式在header中传给后端。关于前端如何在header中以`basic-auth`的方式传递token给后端,下面有详细讲解

* 1 安装[`js-base64`](https://www.npmjs.com/package/js-base64)

  ```js
  npm i js-base64
  ```


* 2 对token进行base64加密

  ```js
  //给token进行base64加密
  encode(){
      const token = wx.getStorageSync('token')
      const base64 = Base64.encode(token+":")
      return 'Basic '+base64
  }
  ```
  
* 3 将base64加密的token通过`basic-auth`传递给后端

  ```js
  header:{
      Authorization: encode() //将加密的token传递在header中传递给前端
  },
  ```
  
* 案例

  ```js
  import {Base64} from 'js-base64'
  
  //小程序端以`basic-auth`方式携带token向后端发请求
  wx.request({
    url: 'http://localhost:3000/v1/classic/latest',
    method: "GET",
    header:{
      Authorization: encode() //将加密的token传递在header中传递给前端
    },
    success: (res) => {
      console.log(res)
    }
  })
  //web端以`basic-auth`方式携带token向后端发请求
  axios.get('http://localhost:3000/v1/classic/latest',{
     headers:{
      Authorization: encode() //将加密的token传递在header中传递给前端
    }, 
  }).then((res)=>{
      console.log(res)
  })
  
  //给token进行base64加密
  encode(){
      const token = wx.getStorageSync('token')
      const base64 = Base64.encode(token+":")
      return 'Basic '+base64
  }
  ```

  

#  三、辅助库

## 1 密码加密处理

1. 安装`bcrypt`

   ```js
   npm i bcrypt
   ```

2. 使用

   ```js
   const bcrypt = require('bcrypt')
   
   //加密
   const salt = bcrypt.genSaltSync(10)//传入数字越大则密码加密越难，成本消耗更大，一般设置为10
   const psw = bcrypt.hashSync('123456',salt)//psw即为‘123456’加密码后的密码
   
   //密码校验
   //plainPassword是原密码，user.password是加密后的密码
   //若原密码与加密密码是一致的则返回true
   const res = bcrypt.compareSync(plainPassword, user.password)
   
   ```

##  2 生成令牌

> [`jsonwebtoken`官网](https://www.npmjs.com/package/jsonwebtoken)

1. 安装`jsonwebtoken`

   ```js
   npm i jsonwebtoken
   ```

2. 使用

   ```js
   const jwt = require('jsonwebtoken')//第三方库 ，需要安装
   
   //生成令牌
   const generateToken = function (uid, scope) {
       const secretKey = global.config.security.secretKey
       const expiresIn = global.config.security.expiresIn
       const token = jwt.sign({
           uid,//用户id
           scope  //用户权限
       }, secretKey, {
           expiresIn: expiresIn
       })
       return token
   }
   ```


##  3 Sequelize 

> Sequelize使用的详细教程请查看：[Sequelize官方文档](https://www.sequelize.com.cn/)
>
> 以下部分主要总结了`sequelize`一些常用功能的使用方法

###  3.1 in查询

> 转入：数组（记录的属性）、   返回：数组（记录）

```js
//记录中有id属性， ids是一个数组，数组内容为多个id号
// Op.in  需要导入Op   ---- const {Op} = require(sequelize)
where: {
    id: {
      [Op.in]: ids// in查询，避免循环查询数据库  ids是要查找的数据id数组
    }
  }
```

###  3.2 查询结果排除特定属性

> 定义在Model上
>
> 使用场景：实例化的对象查询的数据都需要排除某些属性

* 定义排除的属性

  ```js
  //下面定义了所有查询结果都排除'updated_at', 'deleted_at', 'created_at'属性
  const sequelize = new Sequelize(....., {
      define: {
  		scopes: {//自定义查询语句，
            bh : {//bh为自定义名称
              attributes: {//查询时调用scope('bh')，则可以在查询时排除以下字段，即查询结果不包含一下字段
                exclude : ['updated_at', 'deleted_at', 'created_at']
              }
            }
          }
      }
  })
  ```

* 使用

  ```js
  //使用时在实例对象上加上`scope('bh')`  bh就是上面自定义的名称
  Movie.scope('bh').findAll(....)
  ```

###  3.3 不包括属性的某个值

```js
// type为属性名， 400是属性的某一个值， type允许的值可以有：100,200,300,400
// 查询结果是所有记录除了type属性值为400记录
where: {
        type: {
          [Op.not]: 400//不包括400
        }
      }
```

###  3.4 事务(数据一致性)

> 事务，确保数据的一致性（两个操作保持一致，要么都操作，要么都没有操作）

```js
//下面是确保了删除和-1操作能一致进行
   sequelize.transaction(async t => {
      //删除favor表单的点赞记录
      await favor.destroy({
        force: true,//false为物理删除，true为软删除
        transaction: t
      })
      //将相应期刊表单中点赞数量-1
      const art = await Art.getData(art_id, type)
      await art.decrement('fav_nums',{by:1, transaction: t})
    })
```

###  3.5 删除操作

> destory关键字

```js
//favor为查询返回的一条记录的实例化对象
favor.destroy({
        force: true,//false为物理删除，true为软删除
      })
```

###  3.6 属性+1-1

> decrement: -1
>
> increment: +1

```js
//art为查询返回的一条记录的实例化对象  fav_nums是记录的一个属性
//下面的操作是给fav_nums属性值-1
art.decrement('fav_nums',{by:1, transaction: t})
```

###  3.7 排序

```js
HotBook.findAll({
      order: [
        'id'//以id属性升序查询
      ]
    })
```





