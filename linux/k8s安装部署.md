# 一. ARM机器安装

## 1. 环境说明

```sh
[root@k8s-master ~]# uname -a
Linux slave1 4.11.0-22.el7a.aarch64 #1 SMP Sun Sep 3 13:39:10 CDT 2017 aarch64 aarch64 aarch64 GNU/Linux
[root@k8s-master ~]# cat /etc/redhat-release 
CentOS Linux release 7.4.1708 (AltArch)
```

## 2. 基础环境准备

1. 机器配置互信

2. 修改主机名

   ```sh
   hostnamectl set-hostname 主机名
   ```

3. 修改master和node的hosts文件

   ```sh
   [root@k8s-master ~]# vim /etc/hosts
   127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
   ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    
   10.2.152.78	k8s-master
   10.2.152.72	k8s-node1
   ```

4. 安装ntp实现所有服务器间的时间同步

   ```sh
   $：yum install ntp -y
   $：vim /etc/ntp.conf 
      21 server 10.2.152.72 iburst       #目标服务器网络位置
      22 #server 0.centos.pool.ntp.org iburst    #一下三个是CentOS官方的NTP服务器，我们注释掉
      23 #server 1.centos.pool.ntp.org iburst
      24 #server 2.centos.pool.ntp.org iburst
      25 #server 3.centos.pool.ntp.org iburst
   $：systemctl start ntpd.service
   $：systemctl enable ntpd.service
   $：systemctl status ntpd.service
   ● ntpd.service - Network Time Service
      Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
      Active: active (running) since Thu 2018-09-06 10:44:05 CST; 1 day 3h ago
    Main PID: 2334 (ntpd)
      CGroup: /system.slice/ntpd.service
              └─2334 /usr/sbin/ntpd -u ntp:ntp -g
    
   Sep 06 11:01:33 slave1 ntpd[2334]: new interface(s) found: wak...r
   Sep 06 11:06:54 slave1 ntpd[2334]: 0.0.0.0 0618 08 no_sys_peer
   Sep 07 09:26:34 slave1 ntpd[2334]: Listen normally on 8 flanne...3
   Sep 07 09:26:34 slave1 ntpd[2334]: Listen normally on 9 flanne...3
   Sep 07 09:26:34 slave1 ntpd[2334]: new interface(s) found: wak...r
   Sep 07 09:56:32 slave1 ntpd[2334]: Listen normally on 10 docke...3
   Sep 07 09:56:32 slave1 ntpd[2334]: Listen normally on 11 flann...3
   Sep 07 09:56:32 slave1 ntpd[2334]: Deleting interface #9 flann...s
   Sep 07 09:56:32 slave1 ntpd[2334]: Deleting interface #7 docke...s
   Sep 07 09:56:32 slave1 ntpd[2334]: new interface(s) found: wak...r
   Hint: Some lines were ellipsized, use -l to show in full.
   ```

5. 关闭master和node的防火墙和selinux

   ```sh
   sudo systemctl stop firewalld 
   sudo systemctl disable firewalld
   sudo vim /etc/selinux/config 
    
   # This file controls the state of SELinux on the system.
   # SELINUX= can take one of these three values:
   #     enforcing - SELinux security policy is enforced.
   #     permissive - SELinux prints warnings instead of enforcing.
   #     disabled - No SELinux policy is loaded.
   SELINUX=disabled
   # SELINUXTYPE= can take one of three two values:
   #     targeted - Targeted processes are protected,
   #     minimum - Modification of targeted policy. Only selected processes are protected. 
   #     mls - Multi Level Security protection.
   SELINUXTYPE=targeted
    
   reboot  //重启服务器
   
   
   ```

6. master和node上安装docker 

   ```sh
   # 安装完docker 以后
   mkdir /etc/docker
   cat > /etc/docker/daemon.json <<EOF
   {
     "exec-opts": ["native.cgroupdriver=systemd"],
     "log-driver": "json-file",
     "log-opts": {
       "max-size": "100m"
     },
     "storage-driver": "overlay2",
     "storage-opts": [
       "overlay2.override_kernel_check=true"
     ],
     "data-root": "/data/docker"
   }
   EOF
   ```

## 3. 安装k8s

1. 更换yum源为阿里源

   ```sh
   vim   /etc/yum.repos.d/kubernetes.repo
   [kubernetes]
   name=Kubernetes
   baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-aarch64
   enabled=1
   gpgcheck=1
   repo_gpgcheck=1
   gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
   exclude=kube*
   ```

2. yum安装k8s

   ```sh
   yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
   ```

3. 启动k8s服务

   ```sh
    systemctl enable kubelet && systemctl start kubelet
   ```

   > 要确保kubelet启动, 如果启动报错
   >
   > - 参考教程https://www.cnblogs.com/hellxz/p/kubelet-cgroup-driver-different-from-docker.html
   >
   > ```sh
   > failed to run Kubelet: misconfiguration: kubelet cgroup driver: "cgroupfs" is different from docker cgroup driver: "systemd"
   > 
   > [kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp [::1]:10248: connect: connection refused.
   > ```
   >
   > 需要确保2.6 步骤中 "exec-opts": ["native.cgroupdriver=systemd"] 是否与kubelet一致

4. 查看版本号

   ```sh
   kubeadm version
   kubeadm version: &version.Info{Major:"1", Minor:"11", GitVersion:"v1.11.2", GitCommit:"bb9ffb1654d4a729bb4cec18ff088eacc153c239", GitTreeState:"clean", BuildDate:"2018-08-07T23:14:39Z", GoVersion:"go1.10.3", Compiler:"gc", Platform:"linux/arm64"}
   ```

5. 配置IPtables

   ```sh
   vim  /etc/sysctl.d/k8s.conf
   net.bridge.bridge-nf-call-ip6tables = 1
   net.bridge.bridge-nf-call-iptables = 1
   vm.swappiness=0
    
   sysctl --system
   ```

6. 关掉swap

   ```sh
   sudo swapoff -a
    #要永久禁掉swap分区，打开如下文件注释掉swap那一行
    # sudo vi /etc/stab
   ```

7. 安装etcd和flannel（master上安装etcd+flannel，node上只安装flannel）

   > 这一步安装后etcd先不要启动

   ```sh
   yum  -y  install  etcd
   # systemctl start etcd
   yum  -y  install  flannel
   ```

8. master上初始化镜像

   > 如果一致停在 Pulling images required for setting up a Kubernetes cluster
   >
   > 手动拉取镜像
   >
   > ```sh
   > # 查看所需镜像
   > kubeadm config images list
   > # 拉取镜像, 注意版本
   > kubeadm config images pull --image-repository=registry.aliyuncs.com/google_containers
   > dockr images
   > # 修改名字
   > docker tag 90d15e1187ca k8s.gcr.io/kube-apiserver:v1.23.0
   > docker rmi registry.aliyuncs.com/google_containers/kube-apiserver:v1.23.0
   > ...
   > kubeadm reset
   > ```
   >
   > -- 再重新初始化

   ```sh
   kubeadm init --kubernetes-version=v1.11.2 --pod-network-cidr=10.2.0.0/16 --apiserver-advertise-address=10.2.152.78
   #这里是之前所安装K8S的版本号；这里填写集群所在网段
   输出：
   [init] using Kubernetes version: v1.11.2
   [preflight] running pre-flight checks
   I0909 11:13:01.251094   31919 kernel_validator.go:81] Validating kernel version
   I0909 11:13:01.252496   31919 kernel_validator.go:96] Validating kernel config
   [preflight/images] Pulling images required for setting up a Kubernetes cluster
   [preflight/images] This might take a minute or two, depending on the speed of your internet connection
   [preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
   [kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
   [kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
   [preflight] Activating the kubelet service
   [certificates] Generated ca certificate and key.
   [certificates] Generated apiserver certificate and key.
   [certificates] apiserver serving cert is signed for DNS names [localhost.localdomain kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.2.152.78]
   [certificates] Generated apiserver-kubelet-client certificate and key.
   [certificates] Generated sa key and public key.
   [certificates] Generated front-proxy-ca certificate and key.
   [certificates] Generated front-proxy-client certificate and key.
   [certificates] Generated etcd/ca certificate and key.
   [certificates] Generated etcd/server certificate and key.
   [certificates] etcd/server serving cert is signed for DNS names [localhost.localdomain localhost] and IPs [127.0.0.1 ::1]
   [certificates] Generated etcd/peer certificate and key.
   [certificates] etcd/peer serving cert is signed for DNS names [localhost.localdomain localhost] and IPs [10.2.152.78 127.0.0.1 ::1]
   [certificates] Generated etcd/healthcheck-client certificate and key.
   [certificates] Generated apiserver-etcd-client certificate and key.
   [certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
   [controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
   [controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
   [controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
   [etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
   [init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests" 
   [init] this might take a minute or longer if the control plane images have to be pulled
   [apiclient] All control plane components are healthy after 75.007389 seconds
   [uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
   [kubelet] Creating a ConfigMap "kubelet-config-1.11" in namespace kube-system with the configuration for the kubelets in the cluster
   [markmaster] Marking the node localhost.localdomain as master by adding the label "node-role.kubernetes.io/master=''"
   [markmaster] Marking the node localhost.localdomain as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
   [patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "localhost.localdomain" as an annotation
   [bootstraptoken] using token: dlo2ec.ynlr9uyocy9vdnvr
   [bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
   [bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
   [bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
   [bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
   [addons] Applied essential addon: CoreDNS
   [addons] Applied essential addon: kube-proxy
    
   Your Kubernetes master has initialized successfully!
    
   To start using your cluster, you need to run the following as a regular user:
    
     mkdir -p $HOME/.kube
     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     sudo chown $(id -u):$(id -g) $HOME/.kube/config
    
   You should now deploy a pod network to the cluster.
   Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
     https://kubernetes.io/docs/concepts/cluster-administration/addons/
    
   You can now join any number of machines by running the following on each node
   as root:
    
     kubeadm join 10.2.152.78:6443 --token dlo2ec.ynlr9uyocy9vdnvr --discovery-token-ca-cert-hash sha256:0457cd2a8ffcf91707a71c4ef6d8717e2a8a6a2c13ad01fa1fc3f15575e28534
   
   
   ```

9. 注意保存添加节点语句

   ```sh
   kubeadm join 10.2.152.78:6443 --token dlo2ec.ynlr9uyocy9vdnvr --discovery-token-ca-cert-hash sha256:0457cd2a8ffcf91707a71c4ef6d8717e2a8a6a2c13ad01fa1fc3f15575e28534
   ```

10. 设置pod网络

    `kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"`

11. 在主节点查看node list `kubectl get nodes`

12. 添加node节点到mastersh上, 在node上执行master初始化保存下来的输出：

    ```sh
    kubeadm join 10.2.152.78:6443 --token dlo2ec.ynlr9uyocy9vdnvr --discovery-token-ca-cert-hash sha256:0457cd2a8ffcf91707a71c4ef6d8717e2a8a6a2c13ad01fa1fc3f15575e28534
    ```

13. 在主节点查看node list `kubectl get nodes`

14. 重新获取ttl语句

    ```sh
    kubeadm token create --ttl 0 --print-join-command
    ```

    

## 4. 部署Dashboard插件

> mkdir ~/k8s
>
> cd ~/k8s

1. 查看pod运行情况

   ```sh
   [root@binghe101 ~]# kubectl get pods -A  -o wide
   NAMESPACE     NAME                                       READY   STATUS    RESTARTS   AGE    IP                NODE        NOMINATED NODE   READINESS GATES
   kube-system   calico-kube-controllers-5b8b769fcd-l2tmm   1/1     Running   2          15h    172.18.203.71     binghe101   <none>           <none>
   kube-system   calico-node-7b7fx                          1/1     Running   2          15h    192.168.175.102   binghe102   <none>           <none>
   kube-system   calico-node-8krsl                          1/1     Running   2          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system   coredns-546565776c-rd2zr                   1/1     Running   2          15h    172.18.203.72     binghe101   <none>           <none>
   kube-system   coredns-546565776c-x8r7l                   1/1     Running   2          15h    172.18.203.73     binghe101   <none>           <none>
   kube-system   etcd-binghe101                             1/1     Running   2          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system   kube-apiserver-binghe101                   1/1     Running   3          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system   kube-controller-manager-binghe101          1/1     Running   3          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system   kube-proxy-cgq5n                           1/1     Running   2          15h    192.168.175.102   binghe102   <none>           <none>
   kube-system   kube-proxy-qnffb                           1/1     Running   2          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system   kube-scheduler-binghe101                   1/1     Running   3          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system   metrics-server-57bc7f4584-cwsn8            1/1     Running   0          109m   172.18.229.68     binghe102   <none>           <none>
   
   
   ```

2. 下载recommended.yaml文件

   ```sh
   wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
   # 如果下载不了用迅雷
   ```

3. 修改recommended.yaml文件

   ```sh
   vim recommended.yaml
   
   ---
   kind: Service
   apiVersion: v1
   metadata:
     labels:
       k8s-app: kubernetes-dashboard
     name: kubernetes-dashboard
     namespace: kubernetes-dashboard
   spec:
     type: NodePort #增加
     ports:
       - port: 443
         targetPort: 8443
         nodePort: 30000 #增加
     selector:
       k8s-app: kubernetes-dashboard
   ---
   ```

4. 创建证书

   ```sh
   mkdir dashboard-certs
   
   cd dashboard-certs/
   
   #创建命名空间
   kubectl create namespace kubernetes-dashboard
   
   # 创建key文件
   openssl genrsa -out dashboard.key 2048
   
   #证书请求
   openssl req -days 36000 -new -out dashboard.csr -key dashboard.key -subj '/CN=dashboard-cert'
   
   #自签证书
   openssl x509 -req -in dashboard.csr -signkey dashboard.key -out dashboard.crt
   
   #创建kubernetes-dashboard-certs对象
   kubectl create secret generic kubernetes-dashboard-certs --from-file=dashboard.key --from-file=dashboard.crt -n kubernetes-dashboard
   
   
   ```

5. 安装dashboard

   ```sh
   kubectl create -f ~/recommended.yaml 
   ```

   >注意：这里可能会报如下所示。
   >
   >Error from server (AlreadyExists): error when creating "./recommended.yaml": namespaces "kubernetes-dashboard" already exists
   >1
   >这是因为我们在创建证书时，已经创建了kubernetes-dashboard命名空间，所以，直接忽略此错误信息即可。

6. 查看安装结果

   ```sh
   [root@binghe101 ~]# kubectl get pods -A  -o wide
   NAMESPACE              NAME                                         READY   STATUS    RESTARTS   AGE    IP                NODE        NOMINATED NODE   READINESS GATES
   kube-system            calico-kube-controllers-5b8b769fcd-l2tmm     1/1     Running   2          15h    172.18.203.71     binghe101   <none>           <none>
   kube-system            calico-node-7b7fx                            1/1     Running   2          15h    192.168.175.102   binghe102   <none>           <none>
   kube-system            calico-node-8krsl                            1/1     Running   2          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system            coredns-546565776c-rd2zr                     1/1     Running   2          15h    172.18.203.72     binghe101   <none>           <none>
   kube-system            coredns-546565776c-x8r7l                     1/1     Running   2          15h    172.18.203.73     binghe101   <none>           <none>
   kube-system            etcd-binghe101                               1/1     Running   2          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system            kube-apiserver-binghe101                     1/1     Running   3          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system            kube-controller-manager-binghe101            1/1     Running   3          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system            kube-proxy-cgq5n                             1/1     Running   2          15h    192.168.175.102   binghe102   <none>           <none>
   kube-system            kube-proxy-qnffb                             1/1     Running   2          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system            kube-scheduler-binghe101                     1/1     Running   3          15h    192.168.175.101   binghe101   <none>           <none>
   kube-system            metrics-server-57bc7f4584-cwsn8              1/1     Running   0          133m   172.18.229.68     binghe102   <none>           <none>
   kubernetes-dashboard   dashboard-metrics-scraper-6b4884c9d5-qccwt   1/1     Running   0          102s   172.18.229.75     binghe102   <none>           <none>
   kubernetes-dashboard   kubernetes-dashboard-7b544877d5-s8cgd        1/1     Running   0          102s   172.18.229.74     binghe102   <none>           <none>
   
   
   
   [root@binghe101 ~]# kubectl get service -n kubernetes-dashboard  -o wide
   NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE     SELECTOR
   dashboard-metrics-scraper   ClusterIP   10.96.249.138   <none>        8000/TCP        2m21s   k8s-app=dashboard-metrics-scraper
   kubernetes-dashboard        NodePort    10.96.219.128   <none>        443:30000/TCP   2m21s   k8s-app=kubernetes-dashboard
   
   
   ```

7. 创建dashboard管理员

   ```bash
   vim dashboard-admin.yaml
   
   apiVersion: v1
   kind: ServiceAccount
   metadata:
     labels:
       k8s-app: kubernetes-dashboard
     name: dashboard-admin
     namespace: kubernetes-dashboard
   
   # 保存
   kubectl create -f ./dashboard-admin.yaml
   
   ```

8. 为用户分配权限

   ```sh
   vim dashboard-admin-bind-cluster-role.yaml
   
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: dashboard-admin-bind-cluster-role
     labels:
       k8s-app: kubernetes-dashboard
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   subjects:
   - kind: ServiceAccount
     name: dashboard-admin
     namespace: kubernetes-dashboard
   
   # 保存退出后执行如下命令为用户分配权限。
   kubectl create -f ./dashboard-admin-bind-cluster-role.yaml
   
   ```

9. 查看并复制用户Token

   ```sh
   kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep dashboard-admin | awk '{print $1}')
   ```

10. 使用https访问 https://192.168.175.101:30000

    ![image-20211213172754874](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20211213172754874.png)

## 5. kubectl使用

```sh
$:kubectl version
Client Version: version.Info{Major:"1", Minor:"11", GitVersion:"v1.11.2", GitCommit:"bb9ffb1654d4a729bb4cec18ff088eacc153c239", GitTreeState:"clean", BuildDate:"2018-08-07T23:17:28Z", GoVersion:"go1.10.3", Compiler:"gc", Platform:"linux/arm64"}
Server Version: version.Info{Major:"1", Minor:"11", GitVersion:"v1.11.2", GitCommit:"bb9ffb1654d4a729bb4cec18ff088eacc153c239", GitTreeState:"clean", BuildDate:"2018-08-07T23:08:19Z", GoVersion:"go1.10.3", Compiler:"gc", Platform:"linux/arm64"}
$:kubectl get nodes
NAME         STATUS    ROLES     AGE       VERSION
k8s-master   Ready     master    1d        v1.11.2
k8s-node1    Ready     <none>    1d        v1.11.2
[root@k8s-master ~]# kubectl run kubernetes-bootcamp --image=jocatalin/kubernetes-bootcamp
deployment.apps/kubernetes-bootcamp created
[root@k8s-master ~]# kubectl get deployments
NAME                  DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   1         1         1            0           3s
[root@k8s-master ~]# kubectl get pods
NAME                                   READY     STATUS             RESTARTS   AGE
kubernetes-bootcamp-589d48ddb4-qkn5s   0/1       ImagePullBackOff   0          54s
[root@k8s-master ~]# kubectl get pods -o wide
NAME                                   READY     STATUS             RESTARTS   AGE       IP          NODE        NOMINATED NODE
kubernetes-bootcamp-589d48ddb4-qkn5s   0/1       ImagePullBackOff   0          1m        10.2.1.12   k8s-node1   <none>
```

