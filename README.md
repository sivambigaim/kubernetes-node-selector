# kubernetes-node-selector
**nodeSelector — This is a simple Pod scheduling feature that allows scheduling a Pod onto a node whose labels match the nodeSelector labels specified by the user.**

# kubectl get nodes --show-labels
NAME               STATUS   ROLES                  AGE   VERSION        LABELS
gratitude-laptop   Ready    <none>                 17d   v1.20.4+k3s1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=k3s,beta.kubernetes.io/os=linux,k3s.io/hostname=gratitude-laptop,k3s.io/internal-ip=10.200.46.112,kubernetes.io/arch=amd64,kubernetes.io/hostname=gratitude-laptop,kubernetes.io/os=linux,node.kubernetes.io/instance-type=k3s
master-laptop      Ready    control-plane,master   18d   v1.20.4+k3s1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=k3s,beta.kubernetes.io/os=linux,k3s.io/hostname=master-laptop,k3s.io/internal-ip=10.200.3.69,kubernetes.io/arch=amd64,kubernetes.io/hostname=master-laptop,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=true,node-role.kubernetes.io/master=true,node.kubernetes.io/instance-type=k3s
  
Adding label to a node
 kubectl label nodes gratitude-laptop disktype=ssd
 
# kubectl get nodes --show-labels
NAME               STATUS   ROLES                  AGE   VERSION        LABELS
master-laptop      Ready    control-plane,master   18d   v1.20.4+k3s1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=k3s,beta.kubernetes.io/os=linux,k3s.io/hostname=master-laptop,k3s.io/internal-ip=10.200.3.69,kubernetes.io/arch=amd64,kubernetes.io/hostname=master-laptop,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=true,node-role.kubernetes.io/master=true,node.kubernetes.io/instance-type=k3s
gratitude-laptop   Ready    <none>                 17d   v1.20.4+k3s1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=k3s,beta.kubernetes.io/os=linux,disktype=ssd,k3s.io/hostname=gratitude-laptop,k3s.io/internal-ip=10.200.46.112,kubernetes.io/arch=amd64,kubernetes.io/hostname=gratitude-laptop,kubernetes.io/os=linux,node.kubernetes.io/instance-type=k3s

create a pod with node-selector flag
# kubectl create -f node_selector.yaml 

# kubectl get po -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP           NODE               NOMINATED NODE   READINESS GATES
my-nginx-5d9cf45bfd-gp4qt   1/1     Running   1          4d23h   10.42.0.23   master-laptop      <none>           <none>
nginx-7848d4b86f-29zkn      1/1     Running   0          33m     10.42.0.30   master-laptop      <none>           <none>
nginx-7848d4b86f-zv47k      1/1     Running   0          33m     10.42.1.24   gratitude-laptop   <none>           <none>
httpd                       1/1     Running   0          25s     10.42.1.25   gratitude-laptop   <none>           <none>

