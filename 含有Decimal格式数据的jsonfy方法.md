# 含有Decimal格式数据的jsonfy方法

标签（空格分隔）： Python

---

```
class DecimalEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, decimal.Decimal):
            return float(o)
        return super(DecimalEncoder, self).default(o)

json.dumps(Decimal('37.1'), cls=DecimalEncoder)
```
或者
```
import simplejson as json
```
```
json.dumps(Decimal('3.9'), use_decimal=True)
'3.9'
```
