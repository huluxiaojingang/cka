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
- kubectl-> api-server create ReplicasSet
- api-server-> etcd create ReplicasSet
- etcd-> api-server 
