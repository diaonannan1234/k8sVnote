# 用户组织设计

``` plantuml


@startuml
<style>
artifact {
  BackGroundColor #ee1100
  LineThickness 1
  LineColor black
}
card {
  BackGroundColor #ff3311
  LineThickness 1
  LineColor black
}
cloud {
  BackGroundColor #ff4422
  LineThickness 1
  LineColor black
}
component {
  BackGroundColor #ff6644
  LineThickness 1
  LineColor black
}
database {
  BackGroundColor #ff9933
  LineThickness 1
  LineColor black
}
file {
  BackGroundColor #feae2d
  LineThickness 1
  LineColor black
}
folder {
  BackGroundColor #ccbb33
  LineThickness 1
  LineColor black
}
frame {
  BackGroundColor #d0c310
  LineThickness 1
  LineColor black
}
hexagon {
  BackGroundColor #aacc22
  LineThickness 1
  LineColor black
}
node {
  BackGroundColor #22ccaa
  LineThickness 1
  LineColor black
}
package {
  BackGroundColor #12bdb9
  LineThickness 1
  LineColor black
}
queue {
  BackGroundColor #11aabb
  LineThickness 1
  LineColor black
}
rectangle {
  BackGroundColor #4444dd
  LineThickness 1
  LineColor black
}
stack {
  BackGroundColor #3311bb
  LineThickness 1
  LineColor black
}
storage {
  BackGroundColor #3b0cbd
  LineThickness 1
  LineColor black
}
</style>
actor user as "集群内部服务N"
card userSpace as “组织机构域1-N”{

node  orgServer as "组织机构API组合服务"{
       component deptApiServer1 as "API解析"
       component deptApiServer as "API查询结果缓存"
       component deptApiServer2 as "API聚合查询"
    
}
user -> orgServer : [调用]
node userServer as "用户API内部服务"

node companyServer as "公司API内部服务" 

node deptmentServer  as "部门API内部服务"

node database as "组织存储内部服务"

 node pvcserver  as "存储PVC"

orgServer  -> userServer
orgServer  -> companyServer
orgServer  -> deptmentServer

 database --(0  pvcserver
  userServer --(0  database
  companyServer  -(0  database
  deptmentServer -(0  database
  
companyServer  --> userServer
deptmentServer --> userServer
companyServer --> deptmentServer

}

card pv as "持久PV"{
    database   store  as "组织机构存储" {

     

      database companyDatabaseMysql as "公司数据库Mysql"{
        
      }
     database deptDatabaseMysql as "部门数据库Mysql"{
        
     }
     database  userDatabaseOpenLdap as "用户持久OpenLDAP"{
        
     }
}
}

pvcserver -[#green,dashed,thickness=4]--> pv : [持久PV一对多存储PVC]


@enduml


``` plantuml