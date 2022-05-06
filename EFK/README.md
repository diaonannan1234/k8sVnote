# README
发布顺序及命令
发布命名空间

kubectl apply -f ns.yml

查看命名空间

kubectl get ns

发布elasticsearch，需要等一段时间才能就绪。

kubectl apply -f elastic.yml

查看刚发布的pod，在命名空间logging里面

kubectl get all -n logging

发布成功，看到status running.

[ryan@lab3 efk_deploy]$ kubectl get all -n logging
NAME                                 READY   STATUS    RESTARTS   AGE
pod/elasticsearch-848b5b7585-dgh8l   1/1     Running   0          15m

NAME                    TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
service/elasticsearch   NodePort   10.99.78.88   <none>        9200:31200/TCP   15m

NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/elasticsearch   1/1     1            1           15m

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/elasticsearch-848b5b7585   1         1         1       15m


可以用下面命令查看minikube的service list。

minikube service list

发布Kibana，需要等一段时间才能就绪。

kubectl apply -f kibana.yml

查看成功：

[ryan@lab3 efk_deploy]$ kubectl get all -n logging
NAME                                 READY   STATUS    RESTARTS   AGE
pod/elasticsearch-848b5b7585-dgh8l   1/1     Running   0          36m
pod/kibana-5c7df47d47-dpjbc          1/1     Running   0          7m18s

NAME                    TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/elasticsearch   NodePort   10.99.78.88     <none>        9200:31200/TCP   36m
service/kibana          NodePort   10.99.146.170   <none>        5601:31601/TCP   7m18s

NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/elasticsearch   1/1     1            1           36m
deployment.apps/kibana          1/1     1            1           7m18s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/elasticsearch-848b5b7585   1         1         1       36m
replicaset.apps/kibana-5c7df47d47          1         1         1       7m18s

发布fluentd K8S角色权限绑定

kubectl apply -f fluentd-rbac.yml

继续发布fluentd-daemonset

kubectl apply -f fluentd-daemonset.yml

查看POD状态：

kubectl get po -n kube-system

[ryan@lab3 efk_deploy]$ kubectl get po -n kube-system
NAME                               READY   STATUS    RESTARTS   AGE
coredns-54d67798b7-2rdv2           1/1     Running   0          12h
etcd-minikube                      1/1     Running   0          12h
fluentd-6jrkj                      1/1     Running   0          2m19s
kube-apiserver-minikube            1/1     Running   0          12h
kube-controller-manager-minikube   1/1     Running   0          12h
kube-proxy-qwd7c                   1/1     Running   0          12h
kube-scheduler-minikube            1/1     Running   0          12h
storage-provisioner                1/1     Running   1          12h


minikube查看minikube node开启的服务端口：
可以发现，minikube node已经在监听服务端口了，kibana应该是可以访问了，但是centos没有装桌面gui怎么办。

minikube service list

[ryan@lab3 conf]$ minikube service list
|----------------------|---------------------------|--------------|---------------------------|
|      NAMESPACE       |           NAME            | TARGET PORT  |            URL            |
|----------------------|---------------------------|--------------|---------------------------|
| default              | kubernetes                | No node port |
| kube-system          | kube-dns                  | No node port |
| kubernetes-dashboard | dashboard-metrics-scraper | No node port |
| kubernetes-dashboard | kubernetes-dashboard      | No node port |
| logging              | elasticsearch             |         9200 | http://192.168.49.2:31200 |
| logging              | kibana                    |         5601 | http://192.168.49.2:31601 |


筛选日志：


结构化：

————————————————
版权声明：本文为CSDN博主「ryanlll3」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ryanlll3/article/details/112255486