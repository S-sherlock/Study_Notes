# 报表页面开发学到的知识
这两天在开发一个接口, 报表页面, 数据狠复杂, 也学习到了很多解决的方法, 在此记录下来
***
**条件过滤**, 也就是在url中添加参数去过滤, 除了drf自带的filter方法之外, 还可以用`request.query_params`去获取查询参数, 以便从数据库中查询.
```python
query_dict = {}
for key, value in request.query_params.items():
    query_dict[key] = value
```
这样就获得了查询的参数, 以字典的形式保存在`query_dict`中.

***
**mongodb的投影查询**

`db.collections.find({}, {"tables":1, "_id":0})`表示获取tables的所有字段, 并且不显示`_id`字段, 1是显示,0是不显示. 前面的花括号:空表示返回所有集合中的`tables`字段, 也可以在里面加条件, 比如获取某个特定的`tables`

`db.collections.find({"date":query_dict["date"]}, {"_id": 0})`表示返回前一个花括号指定的日期, 并且不返回`_id`字典
`db.collectionsfind({"date":query_dict["date"]}, {"navCalculationItems": 1, "_id": 0})`表示返回包含指定日期那个集合下`navCalculationItems`字段的内容.


***
**django的反向查询**
```python
product_obj = Product.objects.get(id=query_dict["product_id"])
futures_account_obj=product_obj.futuresaccount_set.all().values()
account_no_list = list(futures_account_obj)
```
`FutureAccount`表里有一个`product`字段是外键, 根据`product_id`查询`FutureAccount`表里对应的`account_no`字段可以像上面那样写, `主表对象.表_set.all()`是反向查询的意思, 但是这样获得的是一个`querySet`对象,  想让他变成一个`json`在后面加个`.values()`就可以了, 他会把`FutureAccount`中的所有字段以`Json`的形式存储,加个`list` 变成列表之后就可以取值遍历了.


***
**字典和列表的方法**

字典`Key`值的修改

如果`dict`是一个字典, name`dict["label"] = dict.pop("name")`的意思是把原来的`key`值`name`改成`label`.用`pop`方法

向列表的指定位置插入数据

假如`list`是一个列表, `list.insert(3, 'a')`, 意思是在列表索引=3处插入`a`.