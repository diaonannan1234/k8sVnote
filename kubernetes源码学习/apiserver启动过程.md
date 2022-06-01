# apiserver启动过程
``` plantuml
@startmindmap
* cmd.kube-apiserver.app.server_go
**  NewAPIServerCommand
***  CreateServerChain
****  createAPIExtensionsConfig
***** createAPIExtensionsServer
****  CreateKubeAPIServerConfig
***** CreateKubeAPIServer
****  createAggregatorConfig
*****  createAggregatorServer

@enddmap
``` plantuml