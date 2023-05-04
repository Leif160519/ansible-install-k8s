# 说明
魔改自阿良老师的项目 https://github.com/lizhenliang/ansible-install-k8s
修改了一些镜像的地址为阿里云
完善了对Debian系列的支持
还有其他，等待你的发现。。。

# Kubernetes v1.26.4 企业级高可用集群自动部署（在线版）
>### 注：确保所有节点系统时间一致
>### 操作系统要求：CentOS7.x_x64 && > Ubuntu 18.04 LTS

### 1、找一台服务器安装Ansible
```
# yum install epel-release -y
# yum install ansible -y
或
# apt-get install ansible -y
```
### 2、下载所需文件

下载Ansible部署文件：

```
# git clone https://github.com/leif160519/ansible-install-k8s
# cd ansible-install-k8s
```

### 3、修改Ansible文件

修改hosts文件，根据规划修改对应IP和名称。

```
# vim inventory/hosts
...
```
修改group_vars/all.yml文件，修改软件包目录和证书可信任IP。

```
# vim group_vars/all.yml
k8s_version: 1.26.4
...
cert_hosts:
  k8s:
  etcd:
```
## 4、一键部署
### 4.1 架构图
单Master架构
![avatar](https://github.com/lizhenliang/ansible-install-k8s/blob/master/single-master.jpg)

多Master架构
![avatar](https://github.com/lizhenliang/ansible-install-k8s/blob/master/multi-master.jpg)
### 4.2 部署命令
单Master版：
```
# ansible-playbook playbooks/single-master-deploy.yml
```
多Master版：
```
# ansible-playbook playbooks/multi-master-deploy.yml
```

## 5、查看集群节点
```
# kubectl get node
NAME            STATUS   ROLES    AGE   VERSION
k8s-master-01   Ready    <none>   54m   v1.26.4
k8s-master-02   Ready    <none>   54m   v1.26.4
k8s-node-01     Ready    <none>   54m   v1.26.4
k8s-node-02     Ready    <none>   54m   v1.26.4
k8s-node-03     Ready    <none>   54m   v1.26.4
```

## 6、其他
### 6.1 部署控制
如果安装某个阶段失败，可针对性测试.

例如：只运行部署插件
```
# ansible-playbook playbooks/single-master-deploy.yml -t addons
```

### 6.2 节点扩容
1）修改hosts，添加新节点ip
```
# vi hosts
...
[newnode]
192.168.31.85 node_name=k8s-node-04
```
2）执行部署
```
# ansible-playbook  playbooks/add-node.yml
```
### 6.3 所有HTTPS证书存放路径
部署产生的证书都会存放到目录“ansible-install-k8s/playbooks/ssl”，一定要保存好，后面还会用到~

<br/>
<br/>
