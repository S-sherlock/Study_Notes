# MongoDB的更新
```python
db.inventory4.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );
```
---
**更新单个文档**

- `db.inventory4.updateOne({item: "paper"}, {$set: {"size.uom": "cm", status: "p"},$currentDate: {lastModified:true}})`把`item`=`paper`的文档更新, `$currentDate`代表使用`$currentDate`操作符将`lastModified`字段的值更新为当前日期。如果`lastModified`字段不存在，`$currentDate`将创建该字段.

**更新多个文档**

- `db.inventory4.updateMany({ "qty": { $lt: 50 } },{$set: { "size.uom": "in", status: "P" },$currentDate: { lastModified: true }})`更新所有`qty`小于50的文档.

**替换一个文档(replace)**

`db.inventory4.replaceOne({ item: "paper" }, { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] })`
要替换除`_id`字段之外的文档的整个内容，请将全新文档作为第二个参数传递给`replaceOne`, 第一个参数定位文档.


**更新集合中的字段名**
`db.collection.update_many({}, {'$rename': {'字段1老名字': '字段1新名字', '字段2老名字': '字段2新名字'}})`

**深入MongoDB的更新（update）操作:数组修改**

```python
> db.post.findOne({"id":1})   
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test1",    
        "test2"    
    ]    
}    
>
```
1. `$push`向数组末尾添加元素
```python
> db.post.update({"id":1},{$push:{"comments": "test3"}})   
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })    
> db.post.findOne({"id":1})    
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test1",    
        "test2",    
        "test3"    
    ]    
}    
>
```
使用`$each`一次性添加多个值：
```python
> db.post.update({"id":1},{$push:{"comments":{$each:["test4","test5","test6"]}}})   
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })    
> db.post.findOne({"id":1})    
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test1",    
        "test2",    
        "test3",    
        "test4",    
        "test5",    
        "test6"    
    ]    
}    
>
```
2. 用`$pop`删除数组中的元素

从数组末尾删除一个值：
```python
> db.post.update({"id":1},{$pop:{"comments":1}})   
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })    
> db.post.findOne({"id":1})    
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test1",    
        "test2",    
        "test3",    
        "test4",    
        "test5"    
    ]    
}
```
从数组开头删除一个值：
```python
> db.post.update({"id":1},{$pop:{"comments":-1}})    
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })    
> db.post.findOne({"id":1})    
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test2",    
        "test3",    
        "test4",    
        "test5"    
    ]    
}    
>
```
3. 删除数组中一个指定的值：
```python
> db.post.update({"id":1},{$pull:{"comments":"test3"}})   
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })    
> db.post.findOne({"id":1})    
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test2",    
        "test4",    
        "test5"    
    ]    
}    
>
```
4. 基于数组下标位置修改：
```python
> db.post.update({"id":1},{$set:{"comments.1":"test9"}})   
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })    
> db.post.findOne({"id":1})    
{    
    "_id" : ObjectId("54a530c3ff0df3732bac1680"),    
    "id" : 1,    
    "name" : "joe",    
    "age" : 21,    
    "comments" : [    
        "test2",    
        "test9",    
        "test5"    
    ]    
}    
>
```



---
**注意**

- 如果`updateOne()`， `updateMany()`或 `replaceOne()`包含且 没有与指定过滤器匹配的文档，则该操作将创建一个新文档并将其插入。如果存在匹配的文档，则操作修改或替换匹配的文档.
- 您无法更新`_id`字段的值，也无法使用具有不同`_id`字段值的替换文档替换现有文档。