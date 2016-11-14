```

var hashMap = {  

    Set : function(key,value){this[key] = value},  

    Get : function(key){return this[key]},  

    Contains : function(key){return this.Get(key) == null?false:true},  

    Remove : function(key){delete this[key]}  

}  

```



```

hashMap.Set("name","John Smith");  

hashMap.Set("age",24);  

hashMap.Get("name");//John Smith  

hashMap.Contains("title");//false  

hashMap.Contains("name");//true  

hashMap.Remove("age"); 

```
