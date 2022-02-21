# Pod异常应如何保证高可用
``` plantuml
@startuml
   title Pod在出现断网，应用错误，如何保证高可用
   [Deployment] --> [Pod:实例一]
      [Deployment] --> [Pod:实例二]
         [Deployment] --> [Pod:实例三]
@enduml
```
```
通过Deployment创建Pod ，可以指定Pod副本集的个数。
上图为3，因此有三个的Pod容器实例启动，
这三个Pod实例均来同一个容器镜像。
```
```
  当Pod:实例出现异常时，Deployment启动一个新的实例，当新实例启动完成后，把流量切到新的实例。
```
``` plantuml
@startuml
    
    title Pod:实例一出现异常，实例报警

   component "Deployment" as deploy
   component "Pod:实例一" as pod1 #red
    component "Pod:实例二" as pod2 
     component "Pod:实例三" as pod3 
   
      
    deploy --> pod1
    deploy --> pod2
    deploy --> pod3


@enduml
```

``` plantuml
@startuml
   title 启动一个新的容器实例Pod4
   component "Deployment" as deploy
   component "Pod:实例一" as pod1 #red
    component "Pod:实例二" as pod2 
     component "Pod:实例三" as pod3 
      component "Pod:实例四" as pod4 #yellow 
      
    deploy --> pod1
    deploy --> pod2
    deploy --> pod3


@enduml
```


``` plantuml
@startuml
   title 新实例Pod4启动完成，流量接入。Pod1停止，流量断开。实例释放资源
   component "Deployment" as deploy
   component "Pod:实例一" as pod1 #red
    component "Pod:实例二" as pod2 
     component "Pod:实例三" as pod3 
      component "Pod:实例四" as pod4 #green 
      

    deploy --> pod2
    deploy --> pod3
    deploy --> pod4

@enduml
> ```