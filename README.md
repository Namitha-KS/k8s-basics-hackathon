# Kubernetes hackathon

## Prerequisites
- Kubectl installation
	- https://kubernetes.io/docs/tasks/tools/
- Minikube installation
	- https://minikube.sigs.k8s.io/docs/start/
- Start the cluster using 
	 ```minikube start```
- Ensure minikube cluster context is set in **kubectl** command


## Task 1
List all kubernetes nodes using `kubectl get nodes` command and paste the output below
	    
```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   31m   v1.28.3

```

## Task 2
describe one kubernetes node using `kubectl describe node/<node-name>` command and paste the section from the terminal output below

**Number of cpu**:     
```
Capacity:
  cpu:                8
  ephemeral-storage:  1055762868Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3898188Ki
  pods:               110
```
**Hostname**: 
```
Addresses:
  InternalIP:  192.168.49.2
  Hostname:    minikube

```
**Cpu and Memory usage**:
```
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                750m (9%)   0 (0%)
  memory             170Mi (4%)  170Mi (4%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
```

## Task 3
List all kubernetes namespaces using `kubectl get namespace` command and paste the output below
	    
```
NAME              STATUS   AGE
default           Active   54m
kube-node-lease   Active   54m
kube-public       Active   54m
kube-system       Active   54m
```

## Task 4
List all the pods in '*kube-system*' namespace using `kubectl get pods -n <namespce command>`
	    
```
NAME                               READY   STATUS    RESTARTS      AGE
coredns-5dd5756b68-m67fl           1/1     Running   0             60m
etcd-minikube                      1/1     Running   0             60m
kube-apiserver-minikube            1/1     Running   0             60m
kube-controller-manager-minikube   1/1     Running   0             60m
kube-proxy-mcncz                   1/1     Running   0             60m
kube-scheduler-minikube            1/1     Running   0             60m
storage-provisioner                1/1     Running   1 (30m ago)   60m

```


## Task 5

Create a namespace with name `my-namespace` using `kubectl create namespace <namespace name>` command

List all namespaces like  in **Task 3**  and paste output below
```
NAME              STATUS   AGE
default           Active   61m
kube-node-lease   Active   61m
kube-public       Active   61m
kube-system       Active   61m
my-namespace      Active   28s

```
## Task 6

Create a Pod with name `nginx` image `nginx:latest` in `my-namespace` using yaml configurations.

1. Create yaml configuration to create a pod should run on the namespace you created in the last **Task 5**

	Refference: https://kubernetes.io/docs/concepts/workloads/pods/#using-pods
3. Use `kubectl apply -f <filename>` command to apply the configuration


Copy the yaml content below
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: my-namespace
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

List all the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace>` command
```
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          2m20s

```

Get extra details about the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace> -o wide` command
```
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          2m54s   10.244.0.3   minikube   <none>           <none>

```

## Task 7

Get details about `nginx` pod in `my-namespace`  using `kubectl describe pods/<pod-name> -n <namespace>` command


```
Name:             nginx
Namespace:        my-namespace
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 27 Mar 2024 14:37:45 +0530
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.3
IPs:
  IP:  10.244.0.3
Containers:
  nginx:
    Container ID:   docker://937130e30fec46f77d967597d3c2c38b35fd61618d078cc13ed47a29b85a0710
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:6db391d1c0cfb30588ba0bf72ea999404f2764febf0f1f196acd5867ac7efa7e
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Wed, 27 Mar 2024 14:39:50 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-7ts4f (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  kube-api-access-7ts4f:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  4m9s  default-scheduler  Successfully assigned my-namespace/nginx to minikube
  Normal  Pulling    4m7s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     2m5s  kubelet            Successfully pulled image "nginx:latest" in 2m2.189s (2m2.189s including waiting)
  Normal  Created    2m4s  kubelet            Created container nginx
  Normal  Started    2m4s  kubelet            Started container nginx

```

## Task 8

Port forward the `nginx` (running in namespace `my-namespace`) pods's port `80` to your local systems port `9090` using `kubectl port-forward` command 

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_port-forward/

```
Forwarding from 127.0.0.1:9090 -> 80
Forwarding from [::1]:9090 -> 80
Handling connection for 9090

```

Open another terminal  and do a curl to the port using given command
`curl http://localhost:9090`

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


## Task 9

List all the logs of pod `nginx` running in `my-namespace`  using `kubectl logs` command

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/03/27 09:09:50 [notice] 1#1: using the "epoll" event method
2024/03/27 09:09:50 [notice] 1#1: nginx/1.25.4
2024/03/27 09:09:50 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2024/03/27 09:09:50 [notice] 1#1: OS: Linux 5.15.146.1-microsoft-standard-WSL2
2024/03/27 09:09:50 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/03/27 09:09:50 [notice] 1#1: start worker processes
2024/03/27 09:09:50 [notice] 1#1: start worker process 29
2024/03/27 09:09:50 [notice] 1#1: start worker process 30
2024/03/27 09:09:50 [notice] 1#1: start worker process 31
2024/03/27 09:09:50 [notice] 1#1: start worker process 32
2024/03/27 09:09:50 [notice] 1#1: start worker process 33
2024/03/27 09:09:50 [notice] 1#1: start worker process 34
2024/03/27 09:09:50 [notice] 1#1: start worker process 35
2024/03/27 09:09:50 [notice] 1#1: start worker process 36
127.0.0.1 - - [27/Mar/2024:09:17:24 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/8.4.0" "-"
127.0.0.1 - - [27/Mar/2024:09:18:23 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36 Edg/122.0.0.0" "-"
127.0.0.1 - - [27/Mar/2024:09:18:23 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:9090/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36 Edg/122.0.0.0" "-"
2024/03/27 09:18:23 [error] 30#30: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:9090", referrer: "http://localhost:9090/"
```

## Task 10
Delete the pod `nginx` using the yaml configurations we created in the **Task 6**

`kubectl delete -f < yaml file name with path from task 6>`

Refference:
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_delete

```
D:\Kubernetes>kubectl delete -f nginx.yaml
pod "nginx" deleted

```

Delete the namespace `my-namespace` using `kubectl delete namespace <namespace>` command

```
D:\Kubernetes>kubectl delete namespace my-namespace
namespace "my-namespace" deleted

```
