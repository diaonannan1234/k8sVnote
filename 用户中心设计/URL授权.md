# URL授权
``` plantuml
@startuml

package k8sVnote/用户中心设计/安全中心/授权服务_AuthZ.md {

actor 项目管理员 as projectAdmin
actor 用户 as user
node AuthZ{
   }

node urlServer {
    component 菜单授权 as menuAuth
    component 页面元素授权 as pageAuth
    component 数据授权 as dataAuth
    component URL授权 as urlAuth
    
  menuAuth --> urlAuth : 依赖
  pageAuth --> urlAuth : 依赖
  dataAuth --> urlAuth : 依赖
 
  
}

node 业务服务一 as server1{
     component 拦截器 as interceptor 
}
node 业务服务二 as server2{
    
}
node 业务服务三 as server3{
    
}
urlServer <-- server1 : 业务服务启动时URL注册
urlServer <-- server2 
urlServer <-- server3 

projectAdmin -> urlServer : 动态配置规则
urlServer -> AuthZ

user --> interceptor : A-1用户请求业务服务资源 
interceptor -> urlServer :A-2对业务服务资源授权认证
@enduml
``` 

