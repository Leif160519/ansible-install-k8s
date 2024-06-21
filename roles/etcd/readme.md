## etcd备份及恢复
### 备份
在每台etcd节点上执行备份脚本
```
/opt/etcd/etcd_backup.sh
```

## 恢复
在每台etcd节点上执行以下操作
- 1.停止kube-apiserver(master节点)和etcd服务
```
systemctl stop kube-api-server
systemctl stop etcd
```

- 2.修改`/opt/etcd/etcd_restore.sh`中备份文件的路径，接着执行恢复脚本
```
/opt/etcd/etcd_restore.sh
```

- 3.启动kube-apiserver和etcd服务
```
systemctl start kube-apiserver
systemctl start etcd
```

- 4.查看kube-apiserver和etcd服务状态
```
systemctl status kube-apiserver
systemctl status etcd
/opt/etcd/etcd.sh
```

## 参考
- [k8s集群（三）- etcd备份恢复][1]

[1]: https://www.yuque.com/u26829067/ggkgmt/ni39qcy4mcwxph0v?singleDoc#
