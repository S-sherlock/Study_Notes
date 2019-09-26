在django开发中，用到mongodb的时候，会有一些操作mongodb的接口。
源代码如下。。


```python
class TradingDayView(APIView):
   
    permission_classes = (IsAuthenticated,)
    authentication_classes = (JWTAuthentication,)
    # 把mongo的连接配置视图类上，避免每次执行都需要连接一遍，有时间可以研究一下通过全局的配置解决这个问题
    trading_days_collection = get_trading_days_collection()

    def __init__(self):

        super(TradingDayView, self).__init__()

    def get(self, request):
        """
        暂时是不带任何过滤的get
        """
        trading_day_data = list(self.trading_days_collection.find({},{"_id": 0}))
        return Response(trading_day_data)

    def post(self, request):
        insert_data = self.trading_days_collection.insert_one(request.data)
        return Response("插入成功") if insert_data.acknowledged else Response("插入失败")

    def patch(self, request):
        # restful风格是在url后面加上要修改的id，
        _id = request.stream.path.split('/')[-1]
        x = self.trading_days_collection.update_one({"_id": ObjectId(_id)}, {"$set": request.data})
        if x.acknowledged:
            updated_data = self.trading_days_collection.find_one({"_id": ObjectId(_id)})
            updated_data['_id'] = str(updated_data['_id'])
            return Response(updated_data)
        else:
            return Response("更新失败")

    def delete(self, request):
        _id = request.stream.path.split('/')[-1]
        delete_data = self.trading_days_collection.delete_one({"_id": ObjectId(_id)})
        return Response("删除成功") if delete_data.acknowledged else Response("删除失败")
```