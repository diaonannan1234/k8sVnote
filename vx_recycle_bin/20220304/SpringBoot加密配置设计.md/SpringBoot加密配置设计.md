# 聚合查询设计
# 
# ```
# 订单与用户为多对多，通过聚合服务向外部提供服务
# 订单服务与用户服务是有状态服务，通过聚合设计使其无耦合
# 聚合服务是一个无状态服务，可水平扩展 
# 缓存服务-cacheServer:应如何设计Key（健值）。当规模分别为巨大和中小时要如何设计缓存 
# ```
# ``` plantuml
# @startuml
## node "aggregation" {
# interface - [orderListA] 
# }
# 
# node "cacheServer" {
# [orderListA] ---> [redis] : 只读
# }
# 
# node "idGeneratr"{
# [orderListA] -> [idGeneratrs]    
# }
# 
# node "User" {
# [orderListA] --> [userList]
# 
# }
# node "Order" {
# [orderListA] --> [orderList]  
# } 
# 
# 
# 
# node "MQ" {
# [orderList] ---|> [rocketMQ] : 订单创建事件
# [rocketMQ] -|> [redis] : 消费订单创建事件
# }
# 
# 
# 
# database "orderdbs" {
# orderList -- orderdb     
# }
# 
# database "userdbs" {
# userList -- userdb   
# }
# 
# 
# 
# @enduml
# ```
# 
# 创建订单
# ``` plantuml 
# @startuml
# actor Actor as user
# user -> aggregation : 1 用户创建订单
# aggregation --> User : 2 传入用户属性
# User --> User : 2.1 验证用户是否存在
# User --> aggregation : 2.1.1 用户存在返回用户对象
# User --> aggregation : 2.1.1 用户不存在按回空用户对象
# aggregation --> idGeneratr : 3 创建订单ID
# aggregation --> aggregation : 3.1 创建订单对象
# aggregation --> order : 3.2 传入订单对象和用户对象
# order --> order : 创建订单
# order --> aggregation : 返回订单
# aggregation -> aggregation : 异步缓存订单对象
# aggregation -> user : 返回创建订单成功消息
# @enduml
# ```
# 
# 创建订单事件
# ``` plantuml 
# @startuml
# aggregation --|> mq : 1 订阅创建订单事件 
# order --> order : 2 创建订单
# order --> mq : 2.1 创建订单成功生成创建订单事件
# mq --> aggregation : 3 消费创建订单事件
# 
# @enduml
# ```
# 
# 查询订单清单
# ``` plantuml 
# @startuml
# actor Actor as user
# user --> aggregation : 1 查询订单请求
# aggregation --> cacheDB : 1.1 查询缓存记录
# aggregation --> user : 1.2 存在缓存记录，直接返回缓存记录
# user --> aggregation : 2 查询订单请求
# aggregation --> order : 2.1 不存在缓存，创建订单查询请求
# order --> aggregation : 2.2 按查询条件返回订单清单
# aggregation --> aggregation : 2.3 找到订单清单中用户ID列表
# aggregation --> cacheDB : 2.3.1 按用户ID询问缓存中用户信息
# aggregation --> userServer : 2.3.2 用户缓存不存在,创建用户ID查询请求
# userServer --> aggregation : 2.3.3 按用户ID查询请求返回用户对象列表
# aggregation  --> aggregation : 2.3.4 订单清单加入用户对象 
# aggregation --> cacheDB : 2.4 订单清单存入缓存
# aggregation  --> user : 2.4 返回订单清单
# @enduml
# ```
# 
# 
# 
# 