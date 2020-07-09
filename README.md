
# helm3

```shell
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
$ helm version
```

# rancher

```shell
1$ helm repo add rancher-stable https://releases.rancher.com/server-charts/stable

2$ helm repo add rancher-stable http://rancher-mirror.oss-cn-beijing.aliyuncs.com/server-charts/stable

helm repo update
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
k3s kubectl create ns cattle-system
k3s kubectl -n cattle-system create secret generic tls-ca --from-file=/etc/rancher/cacerts.pem

helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=rancher.dev.run \
  --set ingress.tls.source=secret \
  --set tls=external \
  --set privateCA=true

k3s kubectl -n cattle-system rollout status deploy/rancher
k3s kubectl -n cattle-system get deploy rancher
```

- reset-password
```
k3s kubectl --kubeconfig $KUBECONFIG -n cattle-system exec $(k3s kubectl --kubeconfig $KUBECONFIG -n cattle-system get pods -l app=rancher | grep '1/1' | head -1 | awk '{ print $1 }') -- reset-password
```

# traefik

```shell
$ k3s kubectl create ns traefik
$ helm repo add traefik https://containous.github.io/traefik-helm-chart
$ helm repo update
$ helm install traefik traefik/traefik \
  --namespace traefik
```

# gitlab

```
$ k3s kubectl create ns dev
$ helm repo add gitlab https://charts.gitlab.io
$ helm repo update
$ helm install gitlab gitlab/gitlab \
  --namespace dev \
  --set global.edition=ce \
  --set certmanager.install=false \
  --set global.ingress.configureCertmanager=false \
  --set global.hosts.domain=git.com \
  --set global.hosts.https=false \
  --set global.hosts.registry.name=reg.git.com \
  --set gitlab-runner.install=false
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
$ cat << EOF > /etc/yum.repos.d/kubernetes.repo
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
