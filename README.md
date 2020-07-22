
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
