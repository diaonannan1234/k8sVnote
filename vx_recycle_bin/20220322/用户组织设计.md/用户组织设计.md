# 用户组织设计

``` plantuml
@startmindmap
* 用户API-调度服务
**  公司服务-总公司
***  部门
***  岗位
***  员工
** 公司服务-二级子公司
** 公司服务-三级子公司
** 公司服务-N级子公司
@endmindmap

@startmindmap
* 公司服务数据来源
**  数据库
**  LDAP活动目录
**  AD域
**  云存储
**  HR系统
@endmindmap


``` plantuml