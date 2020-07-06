# k3s

```shell
$ curl -sfL https://get.k3s.io | sh -s - server --datastore-endpoint="mysql://username:password@tcp(hostname:3306)/database"

$ systemctl status k3s
$ k3s kubectl get nodes
$ k3s kubectl get pods -A
$ k3s kubectl get svc -A
$ crictl ps
$ cd /var/lib/rancher/k3s/agent/etc/containerd
$ cp config.toml config.toml.tmpl
$ >>
[plugins.cri.registry.mirrors]
  [plugins.cri.registry.mirrors."docker.io"]
    endpoint = ["https://docker.mirrors.ustc.edu.cn"]
$ systemctl restart k3s
$ crictl info
```

# helm3

```shell
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
$ helm version
```

- gitlab

```
$ k3s kubectl create namespace dev
$ helm repo add gitlab https://charts.gitlab.io
$ helm install gitlab gitlab/gitlab \
  --namespace dev \
  --set global.edition=ce \
  --set global.hosts.domain=git.com \
  --set global.hosts.https=false \
  --set global.hosts.registry.name=reg.git.com
```

# rancher

```shell
$ helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
$ helm repo add rancher-stable http://rancher-mirror.oss-cn-beijing.aliyuncs.com/server-charts/stable
$ export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
$ k3s kubectl create namespace cattle-system
$ k3s kubectl -n cattle-system create secret generic tls-ca --from-file=cacerts.pem
$ helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set replicas=1 \
  --set hostname=rancher.dev.run \
  --set ingress.tls.source=secret \
  --set tls=external \
  --set privateCA=true
$ helm ls --namespace cattle-system
$ kubectl -n cattle-system rollout status deploy/rancher
$ kubectl -n cattle-system get deploy rancher
```

# FIX

```
$ swapoff -a
$ env | grep -i kub
$ systemctl status firewalld.service
$ systemctl status docker.service
$ systemctl status kubelet
$ netstat -pnlt | grep 6443
```

```
$ k3s kubectl get pods -n kube-system | grep -v Running
$ k3s kubectl describe pod -n kube-system metrics-server-7566d596c8-krsf4
$ k3s kubectl logs -n kube-system metrics-server-7566d596c8-krsf4
$ k3s kubectl delete pod -n kube-system metrics-server-7566d596c8-krsf4
$ k3s kubectl delete namespace cattle-system
$ k3s kubectl get namespace "cattle-system" -o json \
            | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/" \
            | kubectl replace --raw /api/v1/namespaces/cattle-system/finalize -f -
```

# [SKIP]kubectl

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
$ kubectl version
$ kubectl cluster-info
```
