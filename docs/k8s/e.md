# 我的k8s环境

```
192.168.16.81 k8s-master1
192.168.16.82 k8s-node1
192.168.16.83 k8s-node2
192.168.16.84 k8s-node3


kubeadm init \
  --apiserver-advertise-address=192.168.16.81
  
  
重新创建token  
kubeadm token create --print-join-command
```


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.16.81:6443 --token yf66nx.g69igjummk9ai83u \
	--discovery-token-ca-cert-hash sha256:4626fb381aba4cfb9d016279150793b7e08dc1d47d2ed99403d4d0f9b058f8a6



```
kubeadm reset

kubeadm init --apiserver-advertise-address=192.168.16.81 --image-repository registry.aliyuncs.com/google_containers

mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

curl https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml -O
kubectl apply -f calico.yaml


kubectl create deploy nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get svc,po


kubectl create deploy tomcat --image=tomcat
kubectl expose deployment tomcat --port=8080 --type=NodePort
kubectl get svc,po

```