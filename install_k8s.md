

### 安装kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos

### 安装dashboard 文档
https://www.cnblogs.com/RainingNight/p/deploying-k8s-dashboard-ui.html

### kubectl 命令使用
https://www.cnblogs.com/51wansheng/p/10249182.html

>1.安装dashboard
 kubectl create -f kubernetes-dashboard.yaml
 
>2.卸载dashboard
  kubectl delete -f kubernetes-dashboard.yaml
  
>3.获取token：
   kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token
  
>4.启动kubernetes-dashboard：
   kubectl proxy
   
>5.访问以下链接时，将获取的token粘贴到输入框中：
   http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/overview?namespace=default
   
>6.如果登陆一会儿后发现提示token过期，强制退出，那么可以修改token的过期时间：
   方式一：找到kubernetes-dashboard的配置文件，添加配置：
   "--token-ttl=43200"
   方式二：或者修改dashboard的yaml文件：
   ports:
   - containerPort: 8443
     protocol: TCP
   args:
     - --auto-generate-certificates
     - --token-ttl=43200

### 查看集群ip

```
kubectl get svc -n kube-system
NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
kube-dns                        ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP   23h
kubernetes-dashboard            ClusterIP   10.98.103.253   <none>        443/TCP                  98s
kubernetes-dashboard-external   NodePort    10.101.205.83   <none>        9090:30090/TCP           22h
```

```
 kubectl -n kube-system get svc -o wide
NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE   SELECTOR
kube-dns               ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP   27m   k8s-app=kube-dns
kubernetes-dashboard   ClusterIP   10.96.173.201   <none>        443/TCP                  20m   k8s-app=kubernetes-dashboard
```

```
 kubectl get service
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)     AGE
greeter-server   ClusterIP   None         <none>        50051/TCP   17h
kubernetes       ClusterIP   10.96.0.1    <none>        443/TCP     23h
```

```
kubectl get pods --namespace kube-system
NAME                                     READY   STATUS    RESTARTS   AGE
coredns-5c98db65d4-bk54f                 1/1     Running   0          27m
coredns-5c98db65d4-vl7d9                 1/1     Running   0          27m
etcd-docker-desktop                      1/1     Running   0          26m
kube-apiserver-docker-desktop            1/1     Running   0          26m
kube-controller-manager-docker-desktop   1/1     Running   0          26m
kube-proxy-9zpwf                         1/1     Running   0          27m
kube-scheduler-docker-desktop            1/1     Running   0          26m
kubernetes-dashboard-8f846679-pwvhc      1/1     Running   0          19m
```

```
kubectl get pods -n kube-system
NAME                                     READY   STATUS    RESTARTS   AGE
coredns-5c98db65d4-bk54f                 1/1     Running   0          26m
coredns-5c98db65d4-vl7d9                 1/1     Running   0          26m
etcd-docker-desktop                      1/1     Running   0          25m
kube-apiserver-docker-desktop            1/1     Running   0          25m
kube-controller-manager-docker-desktop   1/1     Running   0          25m
kube-proxy-9zpwf                         1/1     Running   0          26m
kube-scheduler-docker-desktop            1/1     Running   0          25m
kubernetes-dashboard-8f846679-pwvhc      1/1     Running   0          19m

```



```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.0/src/deploy/recommended/kubernetes-dashboard.yaml
secret/kubernetes-dashboard-certs created
serviceaccount/kubernetes-dashboard created
role.rbac.authorization.k8s.io/kubernetes-dashboard-minimal created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard-minimal created
deployment.apps/kubernetes-dashboard created
service/kubernetes-dashboard created

访问dashboard页面
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login

kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | awk '/^deployment-controller-token-/{print $1}') | awk '$1=="token:"{print $2}'
eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZXBsb3ltZW50LWNvbnRyb2xsZXItdG9rZW4tajlkaHMiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGVwbG95bWVudC1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiODQyZjk4NDgtMGU0Yy00NjA4LThhZWMtN2U0NTQ4ZTBlNTc0Iiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRlcGxveW1lbnQtY29udHJvbGxlciJ9.OHdc6qnMwOyneGo7C5shTjPSWS9HRJ1BzPTlfRZQaykEQnpE4GwePzG25UJS7J_Q8V9KUc7UIt3ZsLAohgTWj2wxF6WMHKMO8sC5m9eeFUU9w4xeLdpXMQxg8iOyVsuQmnbWgiHZw77Uuvx81F-6kYgXRmfw_j3emzTN0Bz6qnnB9GLOCeS2WrfJFKxsb9S8dcWIAsnURKh0nvtTqUkl60YMqPJc8Pn6D1SoB4QzWObLq4tku5SNqaN6nXSbh-QNQx40Z4MF08HVwgpVyp8uOaIGlaRaMk8I7KXq3MxLkBrPJ7YPtkVeeD5-9wgKdYlG6Y3l5hEgIzoUJz4mPWbu2A

```



```
kubectl get pods -n kube-system | grep -v Running
NAME                                     READY   STATUS    RESTARTS   AGE

kubectl -n kube-system describe pod kubernetes-dashboard
Name:           kubernetes-dashboard-8f846679-pwvhc
Namespace:      kube-system
Priority:       0
Node:           docker-desktop/192.168.65.3
Start Time:     Wed, 21 Oct 2020 19:15:55 +0800
Labels:         k8s-app=kubernetes-dashboard
                pod-template-hash=8f846679
Annotations:    <none>
Status:         Running
IP:             10.1.0.12
Controlled By:  ReplicaSet/kubernetes-dashboard-8f846679
Containers:
  kubernetes-dashboard:
    Container ID:  docker://73d40598e279a0718b57de317054fb8550f255d4e4bcad0833fc7cd9227b38ba
    Image:         k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.0
    Image ID:      docker-pullable://k8s.gcr.io/kubernetes-dashboard-amd64@sha256:1d2e1229a918f4bc38b5a3f9f5f11302b3e71f8397b492afac7f273a0008776a
    Port:          8443/TCP
    Host Port:     0/TCP
    Args:
      --auto-generate-certificates
    State:          Running
      Started:      Wed, 21 Oct 2020 19:15:56 +0800
    Ready:          True
    Restart Count:  0
    Liveness:       http-get https://:8443/ delay=30s timeout=30s period=10s #success=1 #failure=3
    Environment:    <none>
    Mounts:
      /certs from kubernetes-dashboard-certs (rw)
      /tmp from tmp-volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kubernetes-dashboard-token-c229c (ro)
```