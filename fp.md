# lowdb/lib/fp

__:warning: Experimental__
__:警告: 实验__

`lowdb/lib/fp` lets you use [`lodash/fp`](https://github.com/lodash/lodash/wiki/FP-Guide), [`Ramda`](https://github.com/ramda/ramda) or simple JavaScript functions with lowdb.

It can help reducing the size of your `bundle.js`

它可以帮助减少`bundle.js`的大小

## Usage

```js
import low from 'lowdb/lib/fp'
import { concat, find, sortBy, take, random } from 'lodash/fp'
import FileSync from 'lowdb/adapters/FileSync'

const adapter = new FileSync('db.json')
const db = low(adapter)

// Get posts
const defaultValue = []
const posts = db('posts', defaultValue)

// replace posts with a new array resulting from concat
// and persist database
// 用 concat 生成的新数组替换帖子
// 并保留数据库
posts.write(
  concat({ title: 'lowdb is awesome', views: random(0, 5) })
)

// Find post by id
// 按 id 查找 posts
const post = posts(
  find({ id: 1 })
)

// Find top 5 posts
// 找到前 5 个 posts
const popular = posts([
  sortBy('views'),
  take(5)
])
```

## API

`lowdb/lib/fp` shares the same API as `lowdb` except for the two following methods.

除了以下两种方法之外,`lowdb/lib/fp` 与 `lowdb` 共享相同的API.

__db(path, [defaultValue])([funcs])__

Returns a new array or object without modifying the database state.

返回新数组或对象而不修改数据库状态.

```js
db('posts')(filter({ published: true }))
```

__db(path, [defaultValue]).write([funcs])__

Add `.write` when you want the result to be written back to `path`

如果希望将结果写回 `path`,请添加 `.write`

```js
db('posts').write(concat(newPost))
db('user.name').write(set('typicode'))
```
