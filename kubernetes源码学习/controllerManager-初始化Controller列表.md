# 初始化Controller列表
``` plantuml
@startmindmap
* 初始化Controller列表
** 1 controllers["endpoint"] = startEndpointController
** 2	controllers["replicationcontroller"] = startReplicationController
** 3	controllers["podgc"] = startPodGCController
** 4	controllers["resourcequota"] = startResourceQuotaController
** 5	controllers["namespace"] = startNamespaceController
** 6	controllers["serviceaccount"] = startServiceAccountController
** 7	controllers["garbagecollector"] = startGarbageCollectorController
** 8	controllers["daemonset"] = startDaemonSetController
** 9	controllers["job"] = startJobController
** 10	controllers["deployment"] = startDeploymentController
** 11	controllers["replicaset"] = startReplicaSetController
** 12	controllers["horizontalpodautoscaling"] = startHPAController
** 13	controllers["disruption"] = startDisruptionController
** 14	controllers["statefulset"] = startStatefulSetController
** 15	controllers["cronjob"] = startCronJobController
** 16	controllers["csrsigning"] = startCSRSigningController
** 17	controllers["csrapproving"] = startCSRApprovingController
** 18	controllers["csrcleaner"] = startCSRCleanerController
** 19	controllers["ttl"] = startTTLController
** 20	controllers["bootstrapsigner"] = startBootstrapSignerController
** 21	controllers["tokencleaner"] = startTokenCleanerController
** 22	controllers["nodeipam"] = startNodeIpamController
** 23	controllers["nodelifecycle"] = startNodeLifecycleController
** 24	controllers["persistentvolume-binder"] = startPersistentVolumeBinderController
** 25	controllers["attachdetach"] = startAttachDetachController
** 26	controllers["persistentvolume-expander"] = startVolumeExpandController
** 27	controllers["clusterrole-aggregation"] = startClusterRoleAggregrationController
** 28	controllers["pvc-protection"] = startPVCProtectionController
** 28	controllers["pv-protection"] = startPVProtectionController
** 29	controllers["ttl-after-finished"] = startTTLAfterFinishedController
** 30	controllers["root-ca--publisher"] = startRootCACertPublisher
** 	if loopMode == IncludeCloudLoops {
*** 1		controllers["service"] = startServiceController
*** 2		controllers["route"] = startRouteController
*** 3		controllers["cloud-node-lifecycle"] = startCloudNodeLifecycleController
***		// TODO: volume controller into the IncludeCloudLoops only set.

@endmindmap
``` plantuml