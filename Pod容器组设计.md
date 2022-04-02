# Pod容器组设计

### Pod容器组边斗设计一
```
Sidecar-边斗和日志收集器在同一个域下由平台为每个POD自动创建，对开发人员透明。
Sidecat对业务应用容器进行代理，可以在Sidecat中实现限流、白名单等服务治理功能。
业务应用日志、容器日志、Pod日志均写入“共享文件目录”中。
“日志收集器”从目录中读取并转发出去。
日志随着时间会慢慢变大，需要在设计中考虑。
```
``` plantuml
@startuml
!include <kubernetes/k8s-sprites-unlabeled-25pct>
title pod设计


package "合同Pod服务"{
   node "合同Pod服务_历史版本1" {
        Pod_Interface - [Sidecar:边斗] 
        [Sidecar:边斗] --> [共享文件目录]
        [Sidecar:边斗] <--> [容器:业务应用] 
        [容器:业务应用] --> [共享文件目录]
        [日志收集器] -> [共享文件目录]
   }

@enduml
```



###  Pod容器组边斗设计二
```
为应用加入监控
```
``` plantuml
@startuml
!include <kubernetes/k8s-sprites-unlabeled-25pct>
title pod设计


node "合同Pod服务_历史版本2" {
        Pod_Interface - [Sidecar:边斗] 
        [Sidecar:边斗] <--> [容器:业务应用] 
        [容器:业务应用] --> [共享文件目录]
        [日志收集器] -> [共享文件目录]
        [监控] -> [容器:业务应用]
   }

@enduml
```

###  Pod容器组边斗设计三
```
为Sidecar加入缓存与（事件、发布订阅、池）队列
```
``` plantuml
@startuml
!include <kubernetes/k8s-sprites-unlabeled-25pct>
title pod设计


node "合同Pod服务_当前版本" {
        Pod_1_Interface - [Sidecar:边斗]
        [Sidecar:边斗] -> [缓存:ES]
        [Sidecar:边斗] --> [队列:MQ] 
        [Sidecar:边斗] <--> [容器:业务应用] 
        [容器:业务应用] --> [共享文件存储]
        [日志收集器] -> [共享文件存储]
        [监控] -> [容器:业务应用]
   }

@enduml
```



``` plantuml
@startuml
!include <kubernetes/k8s-sprites-unlabeled-25pct>
title pod设计


package "合同Pod服务"{
   node "合同Pod服务_历史版本1" {
        Pod_Interface - [Sidecar:边斗] 
        [Sidecar:边斗] --> [共享文件存储]
        [Sidecar:边斗] <--> [容器:业务应用] 
        [容器:业务应用] --> [共享文件存储]
        [日志收集器] -> [共享文件存储]
   }
   
node "合同Pod服务_历史版本2" {
        Pod_Interface_N - [Sidecar:边斗_N] 
        [Sidecar:边斗_N] <--> [容器:业务应用_N] 
        [容器:业务应用_N] --> [共享文件存储_N]
        [日志收集器_N] -> [共享文件存储_N]
        [监控] -> [容器:业务应用_N]
   }
   
node "合同Pod服务_当前版本" {
        Pod_1_Interface - [Sidecar:边斗_1]
        [Sidecar:边斗_1] -> [缓存:ES]
        [Sidecar:边斗_1] --> [队列:MQ] 
        [Sidecar:边斗_1] <--> [容器:业务应用_1] 
        [容器:业务应用_1] --> [共享文件存储_1]
        [日志收集器_1] -> [共享文件存储_1]
        [监控_1] -> [容器:业务应用_1]
   }
   }

  package "日志中心:基础设施命名空间" {
  
    component "<$pod>\npod" as kfaka
    
    node "pod日志" {
        LogInterface -> [日志API服务]
        [日志API服务] --> LogOutInterface
    }
    
    node "业务日志"{
        LogOutInterface --> BLogInterface
         BLogInterface -> [业务日志代理]
         [业务日志代理] --> BLogOutInterface
    }
    
    node "服务器日志" {
        LogOutInterface --> MLogInterface
        MLogInterface -> [服务器日志代理]
        [服务器日志代理] --> MLogOutInterface
    }
  }
  package "大数据存储中心"{
   database "大数据存储" {
      DataInterface --> [HBase]
      DataInterface --> [Spark]
      DataInterface --> [云存储]
      DataInterface --> [ES]
      DataInterface --> [自定义存储]
      [HBase] --> DataOutInterface
      [Spark] --> DataOutInterface
      [云存储] --> DataOutInterface
      [ES] --> DataOutInterface
      [自定义存储] --> DataOutInterface
   }
   }
   
package "数据分析中心"{
node "AI日志分析"{
   
  }
  node "用户画像分析"{
    
  }
  node "业务BI分析"{
    
  }
  node "运维架构地图"{
    
  }
  node "自定义分析"{
    
  }
  node "ApiDataServer"{
    
  }
  }
  
   [日志API服务] --> [DataInterface]
   [MLogOutInterface] --> [DataInterface]
   [BLogOutInterface] --> [DataInterface]
  [日志收集器] --> [LogInterface]
    [日志收集器_N] --> [LogInterface]
        [日志收集器_1] --> [LogInterface]
        component "<$user>\nuser" as user
        user --> ApiDataServer
        DataOutInterface -->AI日志分析
         DataOutInterface -->用户画像分析
          DataOutInterface -->业务BI分析
           DataOutInterface -->运维架构地图
            DataOutInterface -->自定义分析
             DataOutInterface -->ApiDataServer
@enduml
```