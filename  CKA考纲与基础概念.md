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
- kubectl -> api-server -> etcd -> api-server -> controller-manager  创建 ReplicasSet,上报事件 ReplicasSet Created
- controller-manager -> api-server -> etcd -> api-server -> scheduler 创建 Pod,上报事件 Pod Created
- scheduler -> api-server -> etcd -> api-server -> kubelet 更新Pod，按调度结果绑定node;上报事件 Pod Bound(Updated)

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
- 给一组Pod设置反向代理，给Pod设置访问代理
### labels-selector
- 打标签做查询

## API对象基本构成
- typeMeta apiVersion、kind
- metaMeta name、labels
- spec     期望状态
- status   实际状态

## 常用命令
- kubectl create expose run set get edit delete ...
- kubectl -h 查询命令帮助文档
- kubectl explain 查询资源定义，查看资源有哪些字段
- kubectl describe 查询资源名称缩写
- kubectl run --image my-deploy -o yaml --dry-run -> my-deploy.yaml 参数`--dry-run`不会创建对象，快速生成yaml模板
- kubectl get statefulset/foo -o yaml --export > new.yaml get命令导出到yaml文件
- kubectl explain pod.spec.affinity.podAffinity 查看字段拼写
- kubectl get po --watch 观察Pod变化
- kubectl get po -owide 查看更多信息
- kubectl scale deployment nginx --replicas=3 水平扩容

## 课后作业
- 通过命令行，使用nginx镜像创建一个pod

```
kubectl run nginx --image=nginx
```
