# Pod异常用户友好响应
``` plantuml
@startuml
   title Pod在出现断网，应用错误等异常
   [Service] --> [Pod:实例一]
      [Service] --> [Pod:实例二]
         [Service] --> [Pod:实例三]
@enduml
```
```
为每个Service增加一个缓存容器，把每个Pod中业务容器的响应按时间、查询条件缓存
```
```
缓存容器应是一个基于内存的文档类型数据库。
```

```
  当Pod:实例出现异常时，Pod在缓存容器中搜索到上一条正常的搜索记录，
  返回客户端。用户端不会出现错误提示，而是正常页面。用户不会感知后端服务出现异常。
```
``` plantuml
@startuml
    
    title Pod:实例一出现异常，从缓存容器中获得结果返回。

   component "Service" as service
   component "Pod:实例一" as pod1 #red
    component "Pod:实例二" as pod2 
     component "Pod:实例三" as pod3
         component "缓存服务" as cache  
   
      
    service --> pod1
    service --> pod2
    service --> pod3
    pod1 --> cache
    cache -> service

@enduml
```



