```
var msgDiv;

windows.onload = function{
    msgDiv = document.getElementById("msg")
}

function showObj(obj){
    var s = "";
    for(var  k in obj){
        s+=k+": "+obj[k]+"</br>"
    }
    msgDiv.innerHTML = s;
}
```
通过这段代码我们可以在网页中一个id为msg的Div中打印出对象的详细信息，用于调试。
