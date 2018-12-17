# Lowdb Chinese README

[![](http://img.shields.io/npm/dm/lowdb.svg?style=flat)](https://www.npmjs.org/package/lowdb) [![Build Status](https://travis-ci.org/typicode/lowdb.svg?branch=master)](https://travis-ci.org/typicode/lowdb) [![npm](https://img.shields.io/npm/v/lowdb.svg)](https://www.npmjs.org/package/lowdb)

> Small JSON database for Node, Electron and the browser. Powered by Lodash. :zap:
> 
> 用于Node,Electron和浏览器的小型JSON数据库. 由Lodash提供技术支持. 


```js
db.get('posts')
  .push({ id: 1, title: 'lowdb is awesome'})
  .write()
```

_To all the amazing people who have answered the lowdb survey, thanks so much!_

_对于所有回答了lowdb调查的人来说,非常感谢！_

<a href="https://www.patreon.com/typicode">
  <img src="https://c5.patreon.com/external/logo/become_a_patron_button@2x.png" width="160">
</a>

## Usage

```sh
npm install lowdb
```

```js
const low = require('lowdb')
const FileSync = require('lowdb/adapters/FileSync')

const adapter = new FileSync('db.json')
const db = low(adapter)

// Set some defaults (required if your JSON file is empty)
// 设置一些默认值(如果您的JSON文件为空,则需要)
db.defaults({ posts: [], user: {}, count: 0 })
  .write()

// Add a post
// 添加一个 post
db.get('posts')
  .push({ id: 1, title: 'lowdb is awesome'})
  .write()

// Set a user using Lodash shorthand syntax
// 使用 Lodash 简写语法设置 user
db.set('user.name', 'typicode')
  .write()
  
// Increment count
// 增量计数
db.update('count', n => n + 1)
  .write()
```

Data is saved to `db.json`

数据保存到了`db.json`

```json
{
  "posts": [
    { "id": 1, "title": "lowdb is awesome"}
  ],
  "user": {
    "name": "typicode"
  },
  "count": 1
}
```

You can use any of the powerful [lodash](https://lodash.com/docs) functions, like [`_.get`](https://lodash.com/docs#get) and [`_.find`](https://lodash.com/docs#find) with shorthand syntax.

您可以使用任何强大的 [lodash](https://lodash.com/docs) 函数,如 [`_.get`](https://lodash.com/docs#get) 和 [`_.find`](https://lodash.com/docs#find) 速记语法.

```js
// For performance, use .value() instead of .write() if you're only reading from db
// 对于性能,如果您只是从 db 读取,请使用 .value() 而不是 .write()
db.get('posts')
  .find({ id: 1 })
  .value()
```

Lowdb is perfect for CLIs, small servers, Electron apps and npm packages in general.

It supports __Node__, the __browser__ and uses __lodash API__, so it's very simple to learn. Actually, if you know Lodash, you already know how to use lowdb :wink:

Lowdb非常适合 CLIs ,小型服务器,Electron应用程序和npm软件包.

它支持 __Node__ ,__browow__ 并使用 __lodash API__,因此学习起来非常简单. 实际上,如果你知道Lodash,你已经知道如何使用lowdb.

* [Usage examples](https://github.com/typicode/lowdb/tree/master/examples)
  * [CLI](https://github.com/typicode/lowdb/tree/master/examples#cli)
  * [Browser](https://github.com/typicode/lowdb/tree/master/examples#browser)
  * [Server](https://github.com/typicode/lowdb/tree/master/examples#server)
  * [In-memory](https://github.com/typicode/lowdb/tree/master/examples#in-memory)
* [JSFiddle live example](https://jsfiddle.net/typicode/4kd7xxbu/)

__Important__ lowdb doesn't support Cluster and may have issues with very large JSON files (~200MB).

__Important__ lowdb不支持群集,可能存在非常大的JSON文件(~200MB)的问题.

## Install

```sh
npm install lowdb
```

Alternatively, if you're using [yarn](https://yarnpkg.com/)

或者,如果您使用  [yarn](https://yarnpkg.com/)

```sh
yarn add lowdb
```

A UMD build is also available on [unpkg](https://unpkg.com/) for testing and quick prototyping:

在 [unpkg](https://unpkg.com/) 上也可以使用UMD版本进行测试和快速原型设计：

```html
<script src="https://unpkg.com/lodash@4/lodash.min.js"></script>
<script src="https://unpkg.com/lowdb@0.17/dist/low.min.js"></script>
<script src="https://unpkg.com/lowdb@0.17/dist/LocalStorage.min.js"></script>
<script>
  var adapter = new LocalStorage('db')
  var db = low(adapter)
</script>
```

## API

__low(adapter)__

Returns a lodash [chain](https://lodash.com/docs/4.17.4#chain) with additional properties and functions described below.

返回lodash [chain](https://lodash.com/docs/4.17.4#chain),其中包含下面描述的其他属性和函数.

__db.[...].write()__ and __db.[...].value()__

`write()` writes database to state.

On the other hand, `value()` is just [\_.prototype.value()](https://lodash.com/docs/4.17.4#prototype-value) and should be used to execute a chain that doesn't change database state.

另一方面,`value()`只是 [\_.prototype.value()](https://lodash.com/docs/4.17.4#prototype-value) ,应该用来执行一个链,不会更改数据库状态.


```js
db.set('user.name', 'typicode')
  .write()
```

Please note that `db.[...].write()` is syntactic sugar and equivalent to

请注意 `db.[...].write()` 是语法糖,相当于

```js
db.set('user.name', 'typicode')
  .value()

db.write()
```

__db.___

Database lodash instance. Use it to add your own utility functions or third-party mixins like [underscore-contrib](https://github.com/documentcloud/underscore-contrib) or [lodash-id](https://github.com/typicode/lodash-id).

数据库lodash实例. 使用它来添加自己的实用程序功能或第三方mixins,如[underscore-contrib](https://github.com/documentcloud/underscore-contrib) or [lodash-id](https://github.com/typicode/lodash-id).

```js
db._.mixin({
  second: function(array) {
    return array[1]
  }
})

db.get('posts')
  .second()
  .value()
```

__db.getState()__

Returns database state.

返回数据库状态.

```js
db.getState() // { posts: [ ... ] }
```

__db.setState(newState)__

Replaces database state.

替换数据库状态.

```js
const newState = {}
db.setState(newState)
```

__db.write()__

Persists database using `adapter.write` (depending on the adapter, may return a promise).

使用 `adapter.write` 持久化数据库(取决于适配器,可能会返回一个promise).

```js
// With lowdb/adapters/FileSync
db.write()
console.log('State has been saved')

// With lowdb/adapters/FileAsync
db.write()
  .then(() => console.log('State has been saved'))
```

__db.read()__

Reads source using `storage.read` option (depending on the adapter, may return a promise).

使用 `storage.read` 选项读取源代码(取决于适配器,可能会返回一个promise).

```js
// With lowdb/FileSync
db.read()
console.log('State has been updated')

// With lowdb/FileAsync
db.read()
  .then(() => console.log('State has been updated'))
```

## Adapters API

Please note this only applies to adapters bundled with Lowdb. Third-party adapters may have different options.

For convenience, `FileSync`, `FileAsync` and `LocalBrowser` accept the following options:



* __defaultValue__ if file doesn't exist, this value will be used to set the initial state (default: `{}`)
* __serialize/deserialize__ functions used before writing and after reading (default: `JSON.stringify` and `JSON.parse`)

请注意,这仅适用于与Lowdb捆绑在一起的适配器. 第三方适配器可能有不同的选项.

为方便起见,`FileSync`,`FileAsync`和`LocalBrowser`接受以下选项：
* __defaultValue__  如果文件不存在,该值将用于设置初始状态 (default: `{}`)
* __serialize/deserialize__ 在写入之前和读取之后使用的函数 (default: `JSON.stringify` and `JSON.parse`)

```js
const adapter = new FileSync('array.yaml', {
  defaultValue: [],
  serialize: (array) => toYamlString(array),
  deserialize: (string) => fromYamlString(string)
})
```

## Guide

### How to query

With lowdb, you get access to the entire [lodash API](http://lodash.com/), so there are many ways to query and manipulate data. Here are a few examples to get you started.

使用lowdb,您可以访问整个 [lodash API](http://lodash.com/) ,因此有许多方法可以查询和操作数据. 以下是一些可以帮助您入门的示例.

Please note that data is returned by reference, this means that modifications to returned objects may change the database. To avoid such behaviour, you need to use `.cloneDeep()`.

请注意,数据是通过引用返回的,这意味着对返回对象的修改可能会更改数据库. 要避免这种行为,您需要使用 `.cloneDeep()`.

Also, the execution of methods is lazy, that is, execution is deferred until `.value()` or `.write()` is called.

此外,方法的执行是惰性的,也就是说,执行被推迟到调用 `.value()` 或 `.write()` .

### Reading from existing JSON file
> 从现有的JSON文件中读取

If you are reading from a file adapter, the path is relative to execution path (CWD) and not to your code.

如果从文件适配器读取,则路径相对于执行路径 (CWD) 而不是代码.

```sh
my_project/
  src/
    my_example.js
  db.json 
```
So then you read it like this:

那你就这样 read 了：

```js
// file src/my_example.js
const adapter = new FileSync('db.json')

// With lowdb/FileAsync
db.read()
  .then(() => console.log('Content of my_project/db.json is loaded'))
```


#### Examples

Check if posts exists.

检查是否存在 posts .

```js
db.has('posts')
  .value()
```

Set posts.

设置 posts.

```js
db.set('posts', [])
  .write()
```


Sort the top five posts.

排名前五位的 posts.

```js
db.get('posts')
  .filter({published: true})
  .sortBy('views')
  .take(5)
  .value()
```

Get post titles.

获得 post 的 titles.

```js
db.get('posts')
  .map('title')
  .value()
```

Get the number of posts.

获取 posts 的 number.

```js
db.get('posts')
  .size()
  .value()
```

Get the title of first post using a path.

使用 path 获取第一个 post 的 title.

```js
db.get('posts[0].title')
  .value()
```

Update a post.

更新 post.

```js
db.get('posts')
  .find({ title: 'low!' })
  .assign({ title: 'hi!'})
  .write()
```

Remove posts.

删除 posts.

```js
db.get('posts')
  .remove({ title: 'low!' })
  .write()
```

Remove a property.

删除属性.

```js
db.unset('user.name')
  .write()
```

Make a deep clone of posts.

对 posts 进行深度克隆.

```js
db.get('posts')
  .cloneDeep()
  .value()
```

### How to use id based resources
> 如何使用基于id的资源

Being able to get data using an id can be quite useful, particularly in servers. To add id-based resources support to lowdb, you have 2 options.

能够使用id获取数据非常有用,尤其是在服务器中.要向lowdb添加基于id的资源支持,您有2个选项.

[shortid](https://github.com/dylang/shortid) is more minimalist and returns a unique id that you can use when creating resources.

[shortid](https://github.com/dylang/shortid) 更简约,并返回一个可在创建资源时使用的唯一ID.

```js
const shortid = require('shortid')

const postId = db
  .get('posts')
  .push({ id: shortid.generate(), title: 'low!' })
  .write()
  .id

const post = db
  .get('posts')
  .find({ id: postId })
  .value()
```

[lodash-id](https://github.com/typicode/lodash-id) provides a set of helpers for creating and manipulating id-based resources.

[lodash-id](https://github.com/typicode/lodash-id) 提供了一组用于创建和操作基于id的资源的帮助程序.

```js
const lodashId = require('lodash-id')
const FileSync = require('lowdb/adapters/FileSync')

const adapter = new FileSync('db.json')
const db = low(adapter)

db._.mixin(lodashId)

// We need to set some default values, if the collection does not exist yet
// We also can store our collection
// 如果 collection 尚不存在,我们需要设置一些默认值
// 我们也可以存储我们的 collection
const collection = db
  .defaults({ posts: [] })
  .get('posts')

// Insert a new post...
// 插入新 post ...
const newPost = collection
  .insert({ title: 'low!' })
  .write()

// ...and retrieve it using its id
// ...并使用其 id 检索它
const post = collection
  .getById(newPost.id)
  .value()
```

### How to create custom adapters
> 如何创建自定义适配器

`low()` accepts custom Adapter, so you can virtually save your data to any storage using any format.

`low()` 接受自定义适配器,因此您可以使用任何格式将数据虚拟保存到任何存储.

```js
class MyStorage {
  constructor() {
    // ...
  }

  read() {
    // Should return data (object or array) or a Promise
    // 应返回数据(对象或数组)或Promise
  }

  write(data) {
    // Should return nothing or a Promise
    // 不应该返回任何内容或Promise
  }
}

const adapter = new MyStorage(args)
const db = low()
```

See [src/adapters](src/adapters) for examples.

有关示例,请参见 [src/adapters](src/adapters) for examples.

### How to encrypt data
> 如何加密数据

`FileSync`, `FileAsync` and `LocalStorage` accept custom `serialize` and `deserialize` functions. You can use them to add encryption logic.

`FileSync`,`FileAsync`和`LocalStorage`接受自定义`serialize`和`deserialize`函数.您可以使用它们来添加加密逻辑.

```js
const adapter = new FileSync('db.json', {
  serialize: (data) => encrypt(JSON.stringify(data)),
  deserialize: (data) => JSON.parse(decrypt(data))
})
```

## Changelog

See changes for each version in the [release notes](https://github.com/typicode/lowdb/releases).

请参阅 [release notes](https://github.com/typicode/lowdb/releases). 中每个版本的更改.

## Limits

Lowdb is a convenient method for storing data without setting up a database server. It is fast enough and safe to be used as an embedded database.

However, if you seek high performance and scalability more than simplicity, you should probably stick to traditional databases like MongoDB.

Lowdb是一种在不设置数据库服务器的情况下存储数据的便捷方法.它足够快且可以安全地用作嵌入式数据库.

但是,如果您寻求高性能和可伸缩性而不是简单性,那么您应该坚持像MongoDB这样的传统数据库.

## License

MIT - [Typicode :cactus:](https://github.com/typicode)
