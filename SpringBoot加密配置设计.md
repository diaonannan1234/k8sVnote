# SpringBoot安全加密敏感配置设计

```
场景：在实际开发中一般会有三个环境（开发、测试、生产）。
     从安全的维度希望生产环境中的数据库用户名和密码越少人知道越好。
     现状是开发人员总可以从代码中得到生产环境的用户名和密码。
     集团要求每隔一段时间更换一下密码、在实际操作中基本依靠人工完成。
     
```

### 解决方案

```
在SpringBoot工程配置文件中引入SpringEL表达式
创建密码配置服务——Secret
```
```
Secret是一个健值对，可以把一些有加密要求的通过Http方式加入。
例如：dbUserName:XXXX;dbPassword:XXXXX
```



## 生产环境加密服务时序
``` plantuml 
@startuml
actor adminActor as adminUser

adminUser --> Secret : 1 动态维护加密策略、加密健值对
BussinessNode --> Secret : 2 业务服务启动时从Secret通过Key读取值
BussinessNode --> BussinessNode : 3 解密健值
BussinessNode --> BussinessNode : 3.1 健值存入本机节点环境变量
BussinessNode --> SpringBootApp : 4.业务应用开始起动
SpringBootApp --> SpringBootApp : 4.1 读取节点环境变量
SpringBootApp --> SpringBootApp : 4.1 替换应用配置文件的SpringEL 
@enduml
```
```
在不同环境中存在不同的Secret服务。Secret服务存储数据是加密的，只有相应环境的管理员可以维护。
应用-SpringBootApp对其是无感和透明的。
Secret服务职责单一，符合单一法则。具有不变性，可以只维护一套代码、多地部署。
提高了可重用性，降低了成本。
随着产品规模扩大、因其具有不变性，不会随着规模的扩大而大幅增加成本。
从而可支撑上层应用的快速变化，边际成本趋向零。
```
###  方案变形一
####  周期自动更新生产环境数据库密码
``` plantuml 
@startuml
actor adminActor as adminUser
SpringBootApp --> message : 1 业务应用起动订阅密钥更新事件
adminUser --> longSecretJob : 1 管理员维护更新策略
longSecretJob  --> longSecretJob : 2 存储策略
longSecretJob  --> longSecretJob : 2 解析策略
longSecretJob --> longSecretJob : 2 定时调度
longSecretJob --> longSecretJob : 2 生成密码
longSecretJob --> longSecretJob : 2 加密算法
longSecretJob --> Secret : 3 周期更新密钥
longSecretJob --> db : 3 周期更新用户名/密码 
longSecretJob --> message : 4 发布密钥更新事件
SpringBootApp <-- message : 5 业务应用消费密钥更新事件
SpringBootApp --> SpringBootApp : 6 动态更新Node环境变量
@enduml
```




