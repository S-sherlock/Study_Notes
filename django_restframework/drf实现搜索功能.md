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
2. 我的路由是`http://127.0.0.1:8000/api/v1/products`, 在后面加query参数进行搜索`?name=xx&start_date=xx&end_date=xx`
`icontains`是模糊匹配, 包含输入字符的所有字段,`field_name`是指定搜索的字段.

3. 搜索做完以后项目运行会产生警告, 大概是加了搜索以后分页排序出的问题. 在`views.py`中加入`queryset = Product.objects.all().order_by('id')`就好了.


**遇到的坑**
1. `django_filters.CharFilter`和`filters.CharFilter`两种一样, 用哪个都行
2. `DateTimeFilter`和`DateFilter`要分清楚
3. 外键的搜索比如`product_id`搜索的时候参数不能用`product_id`要用`product`