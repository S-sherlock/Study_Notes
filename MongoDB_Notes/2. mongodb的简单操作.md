## MongoDB的操作(CRUD)
## 插入文档
如果集合当前不存在，则插入操作将创建集合。

```python
db.inventory.insert_one(
    {"item": "mongodb",
     "name": "数据库",
     "size": {"B": 64, "S": 32, "L": 18}
    }
)
```
查询刚刚插入的文档
`db.inventory.find({"item": "mongodb"})`

插入多条文档
`db.inventory.insert_many()`

插入单个或多个
`db.inventory.insert()`

_id可以自己指定,但必须保证其唯一性 .

## 查询文档
```python
db.study.insert([
    {"name": "nike",
     "qty": 25,
     "size": {"h": 14, "w": 21, "uom": "cm"},
     "status": "A"},
     {"name": "adidas",
     "qty": 50,
     "size": {"h": 8.5, "w": 11, "uom": "in"},
     "status": "A"},
     {"name": "kappa",
     "qty": 100,
     "size": {"h": 8.5, "w": 11, "uom": "in"},
     "status": "D"},
     {"name": "anta",
     "qty": 75, "size": {"h": 22.85, "w": 30, "uom": "cm"},
     "status": "D"},
     {"name": "lining",
     "qty": 45,
     "size": {"h": 10, "w": 15.25, "uom": "cm"},
     "status": "A"}])
```
1. db.study.find();
2. db.study.find({"status":"A"}), 条件查询, 查询状态为A的数据
3. db.study.find({"status": {"$in": ["A", "D"]}}), 条件查询,状态包括AD的数据
4. db.study.find({"status": "A", "qty": {"$lt": 30}}) 状态为A,且qty小于30的数据
5. db.study.find({"$or": [{"status": "A"}, {"qty": {"$lt": 30}}]}), 查找状态为A, 或者qty小于30的数据
6. db.study.find({
    "status": "A",
    "$or": [{"qty": {"$lt": 30}}, {"name": {"$regex": "^n"}}]})
查找状态为A, 或者qty小于30/且name以n开头的
## 查询嵌入式文档
1. db.study.find( { size: { h: 14, w: 21, uom: "cm" } } ),
2. db.inventory.find(  { size: { w: 21, h: 14, uom: "cm" } }  )
3. 如果查询条件字段的顺序错了,查不出集合中的文档.例子2会匹配不到文档
4. db.study.find({"size.uom": "in"}) ,嵌套字段查询
5. db.inventory.find( { "size.h": { $lt: 15 } } )
6. db.study.find( { "size.h": { $lt: 15 }, "size.uom": "in", status: "D" } )
## 查询数组
```
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

1. `db.inventory.find( { tags: ["red", "blank"] } )` 查询仅包含red,blank数组的文档.
2. `db.inventory.find( { tags: { $all: ["red", "blank"] } } )`,查询包含red, blank数组的所有文档(不考虑其他元素或者顺序)
3. `db.inventory.find( { tags: "red" } )`查询所有文档，其中标记是一个数组，其中包含字符串“red”作为其元素之一
4. `db.inventory.find( { dim_cm: { $gt: 25 } } )`查询数组dim_cm中包含至少一个值大于25的元素的所有文档
5. `db.inventory.find( { dim_cm: { $gt: 15, $lt: 20 } } )`下面的示例查询文档，其中`dim_cm`数组包含在某些组合中满足查询条件的元素;例如，一个元素可以满足大于15的条件，另一个元素可以满足
****

6. `db.inventory.find( { dim_cm: { $elemMatch: { $gt: 16, $lt: 30 } } } )`
7. `b.inventory.find( { "dim_cm.1": { $gt: 25 } } )` 查询数组`dim_cm`中的第二个元素大于25的所有文档
8. `b.inventory.find( { "tags": { $size: 3 } } )`使用`$size`操作符按元素数量查询数组。例如，下面选择数组标记有3个元素的文档

## 查询嵌套在数组中的文档
```
db.inventory.insertMany( [
   { item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ] },
   { item: "notebook", instock: [ { warehouse: "C", qty: 5 } ] },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 15 } ] },
   { item: "planner", instock: [ { warehouse: "A", qty: 40 }, { warehouse: "B", qty: 5 } ] },
   { item: "postcard", instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```
1. `db.inventory.find( { "instock": { warehouse: "A", qty: 5 } } )`
2. `db.inventory.find( { "instock": { qty: 5, warehouse: "A" } } )`顺序错了, 查询不到文档,嵌入文档要求精确匹配

### 在文档数组中的字段上指定查询条件
1. `db.inventory.find( { 'instock.qty': { $lte: 20 } } )`
2. `db.inventory.find( { 'instock.0.qty': { $lte: 20 } } )`使用索引查询
3. `db.inventory.find( { "instock": { $elemMatch: { qty: { $gt: 10, $lte: 20 } } } } )`如果用`$elemMatch`说明需要满足大于10且小于20, 在10-20区间内的数据
4. `db.inventory.find( { "instock.qty": { $gt: 10,  $lte: 20 } } )`如果不用`$elemMatch`说明只要满足大于10和小于20的所有数据.都可以, 包括5