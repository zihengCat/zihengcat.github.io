---
title: 快速搭建单主节点 Kubernetes 集群
subtitle: Setup and Run A Single-host Kubernetes Cluster
catalog: true
date: 2020-06-04
tags: ["Kubernetes", "Docker", "Cloud Native"]
---

# 前言

本文讲解如何在 x86_64 的 CentOS Linux 主机上快速搭建单主节点 Kubernetes 集群。

# 安装前配置

| 主机          | 角色         |
| :----------- | :----------- |
| `172.16.1.4` | master-node  |
| `172.16.1.5` | worker-node1 |
| `172.16.1.6` | worker-node2 |
| `172.16.1.7` | worker-node3 |

> 表：主机角色分配一览表

## 修改主机名

```bash
$ sudo hostnamectl set-hostname 'master-node'
```
> 代码清单：设置主机名

## 添加 Hosts 映射信息

```bash
$ sudo vi /etc/hosts
...
```
> 代码清单：设置 Hosts 映射信息

输入各节点 Host 信息：

```plain
172.16.1.4    master-node
172.16.1.5    worker-node1
172.16.1.6    worker-node2
172.16.1.7    worker-node3
```
> 代码清单：Hosts 映射信息

## 关闭 SELinux

```bash
$ sudo setenforce 0
$ sudo sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
```
> 代码清单：关闭 SELinux

## 关闭防火墙

```bash
$ sudo systemctl stop firewalld
$ sudo systemctl disable firewalld
```
> 代码清单：关闭防火墙

## 关闭 swap 虚拟内存

```bash
$ sudo swapoff -a
$ sudo sed -i '/swap/s/^\(.*\)$/#\1/g' /etc/fstab
```
> 代码清单：关闭 swap 虚拟内存

## 修改内核参数

```bash
# 制作配置文件
$ cat > /etc/sysctl.d/kubernetes.conf << EOF
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
vm.swappiness = 0
vm.overcommit_memory = 1
vm.panic_on_oom = 0
fs.inotify.max_user_watches = 89100
EOF
# 应用配置文件
$ sysctl -p /etc/sysctl.d/kubernetes.conf
```
> 代码清单：修改内核参数

# 安装 Docker

```bash
# 安装 Docker 依赖包
$ yum install -y yum-utils device-mapper-persistent-data lvm2
# 添加 Docker CE 仓库
$ yum-config-manager --add-repo \
  'https://download.docker.com/linux/centos/docker-ce.repo'
# 安装 Docker CE 
# yum search --showduplicates 查看历史版本
$ yum install -y docker-ce
# 创建 Docker CE 配置目录
$ mkdir /etc/docker
# 配置 Docker CE 服务
$ cat > /etc/docker/daemon.json << EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF
# 启动 Docker CE 服务
$ systemctl enable docker
$ systemctl restart docker
# 添加当前用户至 Docker 用户组
$ sudo usermod -aG docker $USER
```
> 代码清单：安装 Docker CE

# 安装 Kubernetes

```bash
$ cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
$ sudo yum install -y kubelet kubeadm kubectl
...
```
> 代码清单：安装 Kubernetes

# 主节点初始化

```bash
# 在 master 节点执行
# export 命令只在当前 Shell 会话中有效
# 开启新的 Shell 窗口后，如果要继续安装过程，请重新执行此处的 export 命令
# 替换 IP 为 master 节点实际 IP（请使用内网 IP）
$ export MASTER_IP='172.16.1.4'
# 替换 APISERVER_NAME 为实际 DNS Name
$ export APISERVER_NAME='master-node'
# Kubernetes 容器组所在的网段
# 该网段安装完成后由 kubernetes 主动创建，事先并不存在于物理网络中
$ export POD_SUBNET='10.100.0.1/16'
# 当前 Kubernetes 版本号
$ export K8S_VERSION='v1.18.3'
$ bash init_master.sh ${K8S_VERSION}
```
> 代码清单：主节点初始化

```bash
#!/bin/bash

# 只在 master 节点执行

# 脚本出错时终止执行
set -e

if [ ${#POD_SUBNET} -eq 0 ] || [ ${#APISERVER_NAME} -eq 0 ]
then
    echo -e "\033[31;1m请确保您已经设置了环境变量 POD_SUBNET 和 APISERVER_NAME \033[0m"
    echo "当前POD_SUBNET=${POD_SUBNET}"
    echo "当前APISERVER_NAME=${APISERVER_NAME}"
    exit 1
fi

# 查看完整配置选项
# https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta2
rm -f ./kubeadm-config.yaml
cat << EOF > ./kubeadm-config.yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: ${1}
#imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers
controlPlaneEndpoint: "${APISERVER_NAME}:6443"
networking:
  serviceSubnet: "10.96.0.0/16"
  podSubnet: "${POD_SUBNET}"
  dnsDomain: "cluster.local"
EOF

# kubeadm init
# 根据服务器网速情况，需要 3 - 10 分钟
kubeadm init \
    --ignore-preflight-errors=NumCPU \
    --config=kubeadm-config.yaml \
    --upload-certs \
    --token-ttl 0

# 配置 kubectl
rm -rf /root/.kube/
mkdir /root/.kube/
cp -i /etc/kubernetes/admin.conf /root/.kube/config

# 容器网络插件：Flannel, Calico, Canal, Weave

# 安装 Flannel 容器网络插件
# 参考：https://coreos.com/flannel/docs/latest/kubernetes.html
echo "安装 Flannel 容器网络插件..."
kubectl apply -f 'https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml'

# 安装 Calico 容器网络插件
# 参考：https://docs.projectcalico.org/getting-started/kubernetes/quickstart
#echo "安装 Calico CNI 网络插件..."
#kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

# 完成
echo "All done!"
```
> 代码清单：主节点初始化 `init_master.sh` 脚本

```bash
$ kubeadm join master-node:6443 --token dm82sq.jzghsst2gbvjh8ol \
    --discovery-token-ca-cert-hash sha256:9184e7612dfeb126c5d0de2452804a9be9b0e66998c449daaf4eaafedaffe534
```
> 代码清单：加入工作节点至 Kubernetes 集群

```bash
$ kubectl get node -owide
NAME           STATUS   ROLES    AGE    VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
master-node    Ready    master   1h     v1.18.3   172.16.1.4    <none>        CentOS Linux 8 (Core)   4.18.0-147.8.1.el8_1.x86_64   docker://18.9.1
worker-node1   Ready    <none>   9m10s  v1.18.3   172.16.1.5    <none>        CentOS Linux 8 (Core)   4.18.0-147.8.1.el8_1.x86_64   docker://18.9.1
worker-node2   Ready    <none>   6m2s   v1.18.3   172.16.1.6    <none>        CentOS Linux 8 (Core)   4.18.0-147.8.1.el8_1.x86_64   docker://18.9.1
worker-node3   Ready    <none>   3m6s   v1.18.3   172.16.1.7    <none>        CentOS Linux 8 (Core)   4.18.0-147.8.1.el8_1.x86_64   docker://18.9.1
```
> 代码清单：验证 Kubernetes 集群

# 完全重置 Kubernetes 集群

```bash
# 使用 kubeadm 重置 Kubernetes 集群
$ kubeadm reset --force
# 停止 kubelet
$ systemctl stop kubelet
# 重置 CNI 容器网络
$ ip route flush proto bird
$ modprobe -r ipip
$ rm -rf /var/lib/cni
$ rm -rf /var/lib/kubelet
$ rm -rf /run/flannel
$ rm -rf /etc/cni
$ ip link list | grep cali | awk '{print $2}' | cut -c 1-15 \
    | xargs -I {} ip link delete {}
$ ip link list | grep flan | awk '{print $2}' | cut -c 1-15 \
    | xargs -I {} ip link delete {}
$ ip link delete cni0
# 重启 Docker 容器运行时
$ docker system prune -a -f
$ systemctl restart docker
```
> 代码清单：完全重置 Kubernetes 集群

# 参考资料

- Kubernetes Documentation: https://kubernetes.io/docs/home/

- Quickstart for Calico on Kubernetes: https://docs.projectcalico.org/getting-started/kubernetes/quickstart

<!-- EOF -->