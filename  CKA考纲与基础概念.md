# k8s架构
## Master
- API-server
- scheduler list & watch 集群中的pod，供调度时使用。watch未调度的Pod，进行多策略调度
- controller-manager watch各类set，处理声明周期时间。定期list做同步处理，保证最终一致性
- etcd key-value存储

## Node
- kubelet watch被调度到本节点的Pod，执行生命周期动作
- kube-proxy

# k8s工作原理

## Pod创建过程
- kubectl -> api-server 创建 ReplicasSet
- api-server -> etcd 创建 ReplicasSet
- etcd -> api-server 上报事件 ReplicasSet Created
- api-server -> controller-manager 上报事件 ReplicasSet Created
- controller-manager -> api-server 创建 Pod
- api-server -> etcd 创建 Pod
- etcd -> api-server 上报事件 Pod Created
- api-server -> scheduler 上报事件 Pod Created
- scheduler -> api-server 更新Pod，按调度结果绑定node
- api-server -> etcd 更新Pod
- etcd -> api-server 上报事件 Pod Bound(Updated)
- api-server -> kubelet 上报事件 Pod Bound(Updated)

## 基本概念
### Pod
- 一组功能相关的Container的封装
- 共享存储和Network Namespace
- K8S调度和作业运行的基本单位
- 容易丢失
### Workloads
- 一组功能相关的Pod封装，Deployment，StatefulSet，DaemonSet，Job
### Service
- Pod防丢失
- 给一组Pod设置反向代理
