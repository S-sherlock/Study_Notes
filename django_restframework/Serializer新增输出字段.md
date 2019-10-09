## SerializerMethodField()的一种用法

drf模型序列化器默认仅返回数据库中已存在字段,如果想新增输出字段,该如何操作?
```
class ProductShareChange(models.Model):
    """
    基金产品份额变动表
    """
    product = models.ForeignKey()
    date = models.DateField()
    direction = models.SmallIntegerField()
    share = models.DecimalField()
    created_at = models.DateTimeField()
    updated_at = models.DateTimeField()
```
如果此时想增加一个字段,那么序列化该这样写
```
class ProductShareChangeSerializer(serializers.ModelSerializer):
    """
    基金产品份额变动的基础序列化器
    """
    product_id = CustomProductIdField()
    type = serializers.SerializerMethodField()

    def get_type(self, obj):
        return "ProductShareChange"

    class Meta:
        model = ProductShareChange
        fields = ['id', 'type', 'product_id', 'date', 'direction', 'share', 'updated_at', 'created_at']
```
type字段的作用是返回这个表的名字, 这个字段在数据库中不能存在.
下面写方法的时候前面加 get_  就可以，这样就得到我们需要的数据了.
然后把它添加到fields中.

还有一种情况，需要获取关联表里面的一些字段（正向查询）

下面代码表示`product`作为外键存在于`StockAccount`中，需要用到`product`中的`alias`字段和`account_no`字段，可以使用正向查询。
```python
class StockAccountSerializer(serializers.ModelSerializer):
    product_account_no = serializers.SerializerMethodField()

    def get_product_account_no(self, obj):
        if obj.product:
            return obj.product.alias + obj.account_no
            
    class Meta:
        model = StockAccount
        fields = ['product_account_no']
```


在此方法里需要调用序列化器，我们直接调用即可，
```
 ad_goods = serializers.SerializerMethodField()  #位于中间部分goods商品的img大图片显示

    def get_ad_goods(self, obj):
        goods_json = {}
        ad_goods = IndexAd.objects.filter(category_id=obj.id,)  #过滤goods在广告表中的数据
        if ad_goods:
            good_ins = ad_goods[0].goods  #取一条
            goods_json = IndexGoodsSerializer(good_ins, many=False, context={‘request‘: self.context[‘request‘]}).data
        return goods_json
```