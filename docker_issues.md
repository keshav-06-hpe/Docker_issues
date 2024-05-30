## This file is made to address the issues faced while cetting up the Kubernetes v1.23 on minikube

### Docker Issue 1:
https://docs.docker.com/engine/install/sles/ <br>
From this above official docker docs we are not able to get a paerticular version ov docker

* Issue is: https://download.docker.com/linux/sles/docker-ce.repo this repo is not working properly
* Even if we try using the packges the packages are not stable.<br>
`Error building the cache: [docker-ce-stable|https://download.docker.com/linux/sles/15/x86_64/stable] Valid metadata not found at specified URL`

### Docker Issue 2:
* If we use the latest version of docker then it is not compatible with k8s v1.23 if it is used as the container-runtime.
`[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 24.0.7-ce. Latest validated version: 20.10`
* k8s v1.23 requires docker v20.10 which is giving errors while downloading.

### Docker Issue 3:

### Installation issue 4:
* RUNTIME_ENABLE: Failed to start container runtime: which crictl: exit status 1
* Even installing crictl gives errors:
 * Exiting due to K8S_INSTALL_FAILED: Failed to update cluster: update primary control-plane node: generating kubeadm cfg: getting cgroup driver: get cri info: sudo crictl info: exit status 1
 ```sh
 stderr:
 sudo: crictl: command not found
```

### Installation Issue 5:
Unable to pull Images:
```sh
 error: exit status 1
        [ERROR ImagePull]: failed to pull image registry.k8s.io/kube-controller-manager:v1.23.17: output: Error response from daemon: Get "https://registry.k8s.io/v2/": dial tcp 34.96.108.209:443: i/o timeout
, error: exit status 1
        [ERROR ImagePull]: failed to pull image registry.k8s.io/kube-scheduler:v1.23.17: output: Error response from daemon: Get "https://registry.k8s.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
, error: exit status 1
        [ERROR ImagePull]: failed to pull image registry.k8s.io/kube-proxy:v1.23.17: output: Error response from daemon: Get "https://registry.k8s.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
, error: exit status 1
        [ERROR ImagePull]: failed to pull image registry.k8s.io/pause:3.6: output: Error response from daemon: Get "https://registry.k8s.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
, error: exit status 1
        [ERROR ImagePull]: failed to pull image registry.k8s.io/etcd:3.5.6-0: output: Error response from daemon: Get "https://registry.k8s.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
, error: exit status 1
        [ERROR ImagePull]: failed to pull image registry.k8s.io/coredns/coredns:v1.8.6: output: Error response from daemon: Get "https://registry.k8s.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

### Installation issue 6:
* kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
 * Kubectl was not installed on its own
* `Unable to connect to the server: net/http: TLS handshake timeout` issue also came up.

### Installation issue 7:
```sh
     Exiting due to NOT_FOUND_CNI_PLUGINS: 


    💡  Suggestion: 

        The none driver with Kubernetes v1.24+ requires containernetworking-plugins.
        
        Please install containernetworking-plugins using these instructions:
        
        https://minikube.sigs.k8s.io/docs/faq/#how-do-i-install-containernetworking-plugins-for-none-driver
```
* CNI issue also came up for which minikube suggested this https://github.com/containernetworking/plugins/releases to refer.
* https://minikube.sigs.k8s.io/docs/faq/#how-do-i-install-containernetworking-plugins-for-none-driver
* Proxy issues also came up:
 * You appear to be using a proxy, but your NO_PROXY environment does not include the minikube IP (10.103.16.194).

### issue 8:
https://kubesphere.io/docs/v3.4/quick-start/all-in-one-on-linux/
<br>
using above link was also a failiure

### issue 9:
initialization failed, will try again: wait: /bin/bash -c "sudo env PATH="/var/lib/minikube/binaries/v1.23.17:$PATH" kubeadm init --config /var/tmp/minikube/kubeadm.yaml --ignore-preflight-errors=DirAvailable--etc-kubernetes-manifests,DirAvailable--var-lib-minikube,DirAvailable--var-lib-minikube-etcd,FileAvailable--etc-kubernetes-manifests-kube-scheduler.yaml,FileAvailable--etc-kubernetes-manifests-kube-apiserver.yaml,FileAvailable--etc-kubernetes-manifests-kube-controller-manager.yaml,FileAvailable--etc-kubernetes-manifests-etcd.yaml,Port-10250,Swap,NumCPU,Mem": exit status 1