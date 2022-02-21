# Pod生产运行时的情景分析
## 实例的创建、释放、监控应尽可能由机器自动完成，实现服务自治的目标

### Pod异常应如何保证高可用
### Pod扩容
### Pod缩容
``` plantuml
@startuml
   node "Pod:容器组" {
       
   }
   @enduml
```
``` plantuml
@startuml
!include <kubernetes/k8s-sprites-unlabeled-25pct>
package "Infrastructure" {
  component "<$master>\nmaster" as master
  component "<$etcd>\netcd" as etcd
  component "<$node>\nnode" as node
}
@enduml

```
``` plantuml
	
@startuml
!include <cloudogu/common>
!include <cloudogu/tools/etcd>
!include <cloudogu/dogus/cloudogu>
!include <cloudogu/dogus/scm>
!include <cloudogu/dogus/smeagol>
!include <cloudogu/dogus/nexus>
!include <cloudogu/tools/k8s>

node "Cloudogu Ecosystem" <<$cloudogu>> {
	
	DOGU_SCM(scm, SCM-Manager) #ffffff
	DOGU_SMEAGOL(smeagol, Smeagol) #ffffff
	DOGU_NEXUS(nexus,Nexus) #ffffff
}

TOOL_K8S(k8s, Kubernetes) #ffffff
TOOL_ETCD(etcd,etcd) #ffffff
actor developer

developer --> smeagol : "Edit Slides"
smeagol -> scm : Push
scm -> etcd : Trigger
etcd -> nexus : Deploy
etcd --> k8s : Deploy
@enduml
``` 