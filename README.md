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
$ helm version
$ helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
$ helm repo add rancher-stable http://rancher-mirror.oss-cn-beijing.aliyuncs.com/server-charts/stable
$ kubectl create namespace bees
```

- BUG
```
$ kubectl delete namespace  bees
$ kubectl get namespace "bees" -o json \
            | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/" \
            | kubectl replace --raw /api/v1/namespaces/bees/finalize -f -
```

# rancher

```shell
$ export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
$ helm install rancher rancher-stable/rancher \
  --namespace bees \
  --set replicas=1 \
  --set hostname=rancher.dev.run \
  --set ingress.tls.source=secret \
  --set privateCA=true
$ helm ls --namespace bees
$ kubectl -n bees rollout status deploy/rancher
$ kubectl -n bees get deploy rancher
```
