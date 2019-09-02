# DRF的高级搜索功能
**需求**
1. 通过产品名字进行模糊匹配, 输入名字中的一个字符或者字, 自动匹配包含此字符或字的所有数据.
2. 根据所选时间范围进行查找数据.

**实现**

1. 在`filters.py`文件中加入如下代码:
```
name = django_filters.CharFilter(field_name="name", lookup_expr="icontains")
start_date = django_filters.DateTimeFilter(field_name="created_at", lookup_expr="gte")
end_date = django_filters.DateTimeFilter(field_name="created_at", lookup_expr="lte")
```
2. 我的路由是`http://127.0.0.1:8000/api/v1/products/search`, 在后面加query参数进行搜索`?name=xx&start_date=xx&end_date=xx`


`icontains`是模糊匹配, 包含输入字符的所有字段,`field_name`是指定搜索的字段.


**遇到的坑**
1. url是`api/v1/products/search`,django的机制是从上往下匹配, 所以他会先匹配到`products`就不匹配了.这时候需要把`search`的接口定位写到`products`路由定位上面就行了.
2. `django_filters.CharFilter`和`filters.CharFilter`两种一样, 用哪个都行
3. `DateTimeFilter`和`DateFilter`要分清楚
4. 外键的搜索比如`product_id`搜索的时候参数不能用`product_id`要用`product`