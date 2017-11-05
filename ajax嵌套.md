# ajax嵌套
在第一个ajax的callback方法中写第二个ajax。发现第二个ajax没有执行到，百度之后发现是ajax的异步问题导致的，因为都是异步，所以不能确定执行的顺序。在此，解决办法有一下集中：

1.在第一个ajax的success方法中写第二个ajax的方法。

2.在第一个ajax中加上async :false

 属性（设置异步为false）,原理是锁死当前浏览器的其他操作，等这个ajax请求完成之后才放行其它请求。
 
 3.在jQuery 1.5 的版本中，引入了deferred延迟对象(基于观察者模式)，具体见以下网址：
 http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html
```
  具体解决方式1：
var promise1 = $.ajax(url1);  
var promise2 = promise1.then(function(data){  
    return $.ajax(url2, { "data": data });  
});  
var promise3 = promise2.then(function(data){  
    return $.ajax(url3, { "data": data });  
});  
promise3.done(function(data){  
    // data retrieved from url3  
}); 
```


```
具体解决方式2：
$.ajax("your/url", {  
    dataType: "json"  
}).pipe(function(theOriginalData) {  
    return $.ajax("your/web/service/doSomethingWith", {  
        data: theOriginalData,  
        dataType: "json"  
    });  
}).done(function(theFinalData) {  
    $.each(theFinalData, function(key, value) {  
        $("#" + key).val(value);  
    });  
});  
```

## ajax异步原理
http://blog.csdn.net/wjw0130/article/details/47314079

