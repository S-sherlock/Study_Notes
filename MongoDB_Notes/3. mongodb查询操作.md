## 复杂文档查询
 `db.collections.find({"tables.name":"product_service_charge"},{"tables":{"$elemMatch":{"name":"valuation_accounting"}}})`
 
 
 
## mongodb的投影查询

```python
db.inventory_2.insertMany( [
  { item: "journal", status: "A", size: { h: 14, w: 21, uom: "cm" }, instock: [ { warehouse: "A", qty: 5 } ] },
  { item: "notebook", status: "A",  size: { h: 8.5, w: 11, uom: "in" }, instock: [ { warehouse: "C", qty: 5 } ] },
  { item: "paper", status: "D", size: { h: 8.5, w: 11, uom: "in" }, instock: [ { warehouse: "A", qty: 60 } ] },
  { item: "planner", status: "D", size: { h: 22.85, w: 30, uom: "cm" }, instock: [ { warehouse: "A", qty: 40 } ] },
  { item: "postcard", status: "A", size: { h: 10, w: 15.25, uom: "cm" }, instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```

1. `db.collections.find({}, {"tables":1, "_id":0})`表示获取tables的所有字段, 并且不显示`_id`字段, 1是显示,0是不显示. 前面的花括号:空表示返回所有集合中的`tables`字段, 也可以在里面加条件, 比如获取某个特定的`tables`

2. `db.collections.find({"date":query_dict["date"]}, {"_id": 0})`表示返回前一个花括号指定的日期, 并且不返回`_id`字段

3. `db.collectionsfind({"date":query_dict["date"]}, {"navCalculationItems": 1, "_id": 0})`表示返回包含指定日期那个集合下`navCalculationItems`字段的内容.

**除了_id字段外，不能在投影文档中组合包含和排除语句。要么都返回,要么都排除**
4. `db.inventory_2.find( { status: "A" }, { status: 0, instock: 0 } )`匹配`status=A`的所有文档, 返回文档中除了`status`和`instock`所有的字段.


**嵌入式查询中禁用特定字段**

5. `db.inventory_2.find({ status: "A" },{ "size.uom": 0 })`隐藏`size`数组中`uom`字段.
 
**嵌入式查询中显示特定字段**
6. `db.inventory_2.find( { status: "A" }, { item: 1, status: 1, "instock.qty": 1 } )`显示`instock`中`qty`字段.


**查询空字段或缺少字段**

```python
db.inventory3.insertMany([
   { _id: 1, item: null },
   { _id: 2 }
])
```
7. `db.inventory3.find( { item: null } )`查询`item`为空的字段或者不包含`item`的字段.
8. `db.inventory.find( { item : { $type: 10 } } )`查询只匹配包含值为null的item字段的文档;即项目字段的值为BSON类型Null(类型号为10)
9. `db.inventory3.find( { item : { $exists: false } } )`查询不含包此字段的文档.