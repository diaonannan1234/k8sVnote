# 安装Helm
### 该指南展示了如何安装Helm CLI。
### Helm可以用源码或构建的二进制版本安装。

* 二进制安装
* [ ] 根据操作系统去获取最新二进制安装包https://github.com/helm/helm/releases       
```
wget https://get.helm.sh/helm-v3.3.1-linux-amd64.tar.gz       
```

* [ ] 由于helm包在国外,我通过ss拉到了腾讯云cos,国内可通过以下地址访问:
```
wget ttps://download.osichina.net/tools/k8s/helm/helm-v3.3.1-linux-amd64.tar.gz       
```
* [ ] 解压缩
```
tar -zxvf helm-v3.3.1-linux-amd64.tar.gz       
```
* [ ] copy到bin目录
```
cp linux-amd64/helm /usr/local/bin/
```
* [ ] 验证
```
helm help
执行结果
The Kubernetes package manager

Common actions for Helm:

- helm search:    search for charts
- helm pull:      download a chart to your local directory to view
- helm install:   upload the chart to Kubernetes
- helm list:      list releases of charts
```