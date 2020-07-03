# k3S

```shell
$ curl -sfL https://get.k3s.io | sh -s - server \
  --datastore-endpoint="mysql://username:password@tcp(hostname:3306)/database-name"

$ curl -sfL https://docs.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -s - server \
  --datastore-endpoint="mysql://username:password@tcp(hostname:3306)/database-name"
  
$ k3s kubectl get nodes
$ k3s kubectl get pods --all-namespaces
```

# kubectl

```shell
$ cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
$ yum install -y kubectl
$ kubectl version --client
```

# helm3

```shell
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```
