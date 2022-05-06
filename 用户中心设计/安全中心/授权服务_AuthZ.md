# 授权服务
``` plantuml
@startuml

package aa {

interface RuleManager 
{
  List<Object> findRuleAll();
  void createRule(String rule)
  void removeRule(String ruleID)
}
note left: 授权规则管理
abstract class AbstractRule implements RuleManager{
    + int count();
}

class AclRule extends AbstractRule{}

class AbacRule extends AbstractRule{}

class RbacRule extends AbstractRule{}

interface ModelMetadata{}

class AclModel implements ModelMetadata{}
class RbacModel implements ModelMetadata{}
class AbacModel implements ModelMetadata{}

interface AuthZ{
   + boolean authZ(String sub,String obj,String act) 
   + boolean authZ(String sub,String obj,String act,String... requestparam)
}


class AclAuthZ implements AuthZ{
    + AclAuthZ(AclModel model,AclRule rule)
}
class RbacAuthZ implements AuthZ{
    + RbacAuthZ(RbacModel model,RbacRule rule)
}
class AbacAuthZ implements AuthZ{
    + AbacAuthZ(AbacModel model,AbacRule rule)
}

 AuthZ o-- ModelMetadata
 AuthZ o-- RuleManager
}


@enduml
``` 