# Python 中一些时间操作技巧


标签（空格分隔）： Python


---


### 获得今日日期
```
import time
today = time.strftime("%Y-%m-%d", time.localtime())
```

### 获得当前年份
```
 def get_this_year(self):
    """
    获得当前年份
    :return:
    """
    return str(time.localtime(time.time())[0])
```
### 日期加减操作
```
import datetime
today = datetime.date.today()
oneday = datetime.timedelta(days=1)
yesterday = today - oneday
tomorrow = today + oneday
```

### 时间字符串转时间戳
```
>>> import time
>>> import datetime
>>> s = "01/12/2011"
>>> time.mktime(datetime.datetime.strptime(s, "%d/%m/%Y").timetuple())
1322697600.0
```
