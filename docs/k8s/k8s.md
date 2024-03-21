# debian12 安装k8s集群


## 准备工作

更新系统到最新，然后移除不再需要的软件，清理无用的安装包。
```
sudo apt update && sudo apt full-upgrade -y
sudo apt autoremove
sudo apt autoclean
```

如果更新了内核，重启。

Kubernetes 的机器不能有 swap 分区，所以要卸载 swap 分区。
```
swapoff -a
```
然后编辑 /etc/fstab 文件，注释或删除 swap 那行（如有）。


开启转发等：
```
cat <<EOF | tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sysctl --system
```

* 设置主机名
```
hostnamectl set-hostname k8s-master1 && hostname
```

##  安装containerd

* 安装dependencies：
```
apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
```

* 添加docker repo：
```
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/debian.gpg
add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```

* 现在安装containerd
```
apt update
apt install -y containerd.io
```

* 配置容器：
```
# 初始化配置
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml

# sandbox_image = "registry.k8s.io/pause:3.6" 更新配置如下
sandbox_image = "registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.6"

# SystemdCgroup = false 更新配置如下
SystemdCgroup = true
```

* 重新启动服务：
```
systemctl restart containerd
systemctl enable containerd
```

* 验证它是否正在运行：
```
root@k8s-master1:~# systemctl status containerd
* containerd.service - containerd container runtime
     Loaded: loaded (/lib/systemd/system/containerd.service; enabled; preset: enabled)
     Active: active (running) since Sun 2023-12-10 09:34:35 CST; 11s ago
       Docs: https://containerd.io
   Main PID: 2858 (containerd)
      Tasks: 8
     Memory: 17.5M
        CPU: 42ms
     CGroup: /system.slice/containerd.service
             `-2858 /usr/bin/containerd

```

```
{
 "registry-mirrors": ["https://p19s5cct.mirror.aliyuncs.com"],
 "exec-opts": ["native.cgroupdriver=systemd"]
}
```

## 安装CNI插件

注意，http://containerd.io包括了runc, 但是不包括CNI插件，我们需要手动安装CNI插件：



访问：[Releases · containernetworking/plugins (github.com)](https://github.com/containernetworking/plugins/releases)获取最新版本的插件，然后将其安装到/opt/cni/bin中：

```
$ wget https://github.com/containernetworking/plugins/releases/download/v1.4.0/cni-plugins-linux-amd64-v1.4.0.tgz
$ mkdir -p /opt/cni/bin
$ tar Cxzvf /opt/cni/bin cni-plugins-linux-amd64-v1.4.0.tgz
./
./macvlan
./static
./vlan
./portmap
./host-local
./vrf
./bridge
./tuning
./firewall
./host-device
./sbr
./loopback
./dhcp
./ptp
./ipvlan
./bandwidth
```


## 安装 kubeadm、kubelet 和 kubectl

1. 更新apt包索引并安装使用 Kubernetesapt仓库所需要的包：
```
apt-get update
apt-get install -y apt-transport-https ca-certificates curl
```

2. 下载 Google Cloud 公开签名秘钥：
```
curl -fsSL https://dl.k8s.io/apt/doc/apt-key.gpg | gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
```

3. 添加 Kubernetesapt仓库：
```
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list
```

4. 更新 apt 包索引，安装 kubelet、kubeadm 和 kubectl，并锁定其版本：
```
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```

说明：
在低于 Debian 12 和 Ubuntu 22.04 的发行版本中，/etc/apt/keyrings 默认不存在。 如有需要，你可以创建此目录，并将其设置为对所有人可读，但仅对管理员可写。


## (可选) 开启ipvs
在kubernetes中service有两种代理模型，一种是基于iptables（链表），另一种是基于ipvs（hash表）。ipvs的性能要高于iptables的，但是如果要使用它，需要手动载入ipvs模块，默认iptables
中文官网参考： https://kubernetes.io/zh-cn/docs/reference/networking/virtual-ips/

#安装ipset和ipvsadm：
apt install -y ipset ipvsadm

### 开启ipvs 转发
modprobe br_netfilter 

# 配置加载模块

```shell
cat > /etc/modules-load.d/ipvs.conf << EOF
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack
EOF
```

# 临时加载
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack

# 开机加载配置，将ipvs相关模块加入配置文件中
```shell
cat >> /etc/modules <<EOF
ip_vs_sh
ip_vs_wrr
ip_vs_rr
ip_vs
nf_conntrack
EOF

## 查看 ip_vs 是否加载进入内核中
lsmod | grep -e ip_vs -e nf_conntrack
```


## 初始化主节点
```
kubeadm init \
  --apiserver-advertise-address=192.168.16.81 \
  --image-repository registry.aliyuncs.com/google_containers \
  --kubernetes-version v1.28.4 \
  --service-cidr=10.96.0.0/12 \
  --pod-network-cidr=10.244.0.0/16

kubeadm init \
  --apiserver-advertise-address=192.168.16.81 \
  --service-cidr=10.96.0.0/12 \
  --pod-network-cidr=10.244.0.0/16

```

**重新创建token**
```
kubeadm token create --print-join-command
```

## 安装网络插件
```
curl https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```