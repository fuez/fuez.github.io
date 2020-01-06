---
title: "k8s"
date: 2017-10-30T14:04:16+08:00
lastmod: 2019-12-09T17:48:47+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Enable tls on ingress](https://kubernetes.github.io/ingress-nginx/examples/tls-termination/)

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nginx-test
spec:
  tls:
    - hosts:
      - foo.bar.com
      # This assumes tls-secret exists and the SSL 
      # certificate contains a CN for foo.bar.com
      secretName: tls-secret
  rules:
    - host: foo.bar.com
      http:
        paths:
        - path: /
          backend:
            # This assumes http-svc exists and routes to healthy endpoints
            serviceName: http-svc
            servicePort: 80
```

NOTE: [create tls secret](https://shocksolution.com/2018/12/14/creating-kubernetes-secrets-using-tls-ssl-as-an-example/): `kubectl create secret tls test-tls --key="tls.key" --cert="tls.crt"
`

## `kubectl get <> -o name` just output name for some type of resource, useful for shell programming

## `kubectl get apiservices` to get all apis including those provided by CRD

## Use `kubectl describe` to get human-readable information about some resouce such as secret

For secret, it will decode content from base64 actaully.

## [How to trigger a Kubernetes cronjob manually?](https://www.craftypenguins.net/how-to-trigger-a-kubernetes-cronjob-manually/)

Like this: `kubectl create job --from=cronjob/<name of cronjob> <name of job>`

## Quick way to create configmap based on files

This is from `charts/stable/mongodb`:

```
data:
{{ tpl (.Files.Glob "files/docker-entrypoint-initdb.d/*[sh|js|json]").AsConfig . | indent 2 }}
```

## k8s backup/migration tool: [velero](https://github.com/vmware-tanzu/velero)

## How to fix API Server down issue on k8s master node?

Looks like the easiest way is just restart the machine

## How to show all CRD resources?
`kubectl --all-namespaces=true get crd` to list all resources, and then `kubectl --all-namespaces=true delete crd <crd-resource-id>` to delete it.

## [Kubernetes ConfigMap Reload](https://github.com/jimmidyson/configmap-reload)

## [Difference between requirements.yaml and requirements.locks](https://docs.bitnami.com/kubernetes/how-to/create-your-first-helm-chart/)
> Much like a runtime language dependency file (such as Python’s requirements.txt), the requirements.yaml file allows you to manage your chart’s dependencies and their versions. When updating dependencies, a lockfile is generated so that subsequent fetching of dependencies use a known, working version.
> 
> The requirements.yaml file lists only the immediate dependencies that your chart needs. This makes it easier for you to focus on your chart.
> 
> The requirements.lock file lists the exact versions of immediate dependencies and their dependencies and their dependencies and so forth. This allows helm to precisely track the entire dependency tree and recreate it exactly as it last worked--even if some of the dependencies (or their dependencies) are updated later.
> 
> Here's roughly how it works:
> 
> You create the initial requirements.yaml file. You run helm install and helm creates the requirements.lock file as it builds the dependency tree.
> On the next helm install, helm will ensure that it uses the same versions identified in the requirements.lock file.
> At some later date, you update the requirements.yaml file. You run helm install (or helm upgrade) and helm will notice your changes and update the requirements.lock file to reflect them.
 

## About *Node affinity*
> Node affinity is conceptually similar to nodeSelector – it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node.
> There are currently two types of node affinity, called requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution. You can think of them as “hard” and “soft” respectively

```yaml
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/e2e-az-name
            operator: In
            values:
            - e2e-az1
            - e2e-az2
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: In
            values:
            - another-node-label-value

```

## What does Mi means? Simply means power-two
> Limits and requests for memory are measured in bytes. You can express memory as a plain integer or as a fixed-point integer using one of these suffixes: E, P, T, G, M, K. You can also use the power-of-two equivalents: Ei, Pi, Ti, Gi, Mi, Ki. 

## [kubectx and kubens](https://github.com/ahmetb/kubectx) command for easy cluster switch and namespace switch

Install on mac: `brew install kubectx` and add an alias for kubens: `alias kn="kubens"`

## [Set default storage class](https://kubernetes.io/docs/tasks/administer-cluster/change-default-storage-class/)

Simply patch the one you wish to set as default with annotation like this:

` kubectl patch storageclass ceph-rbd -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`


## Notes about PV/PVC
> After a PV has been bound to a PVC, however, that PV cannot then be bound to additional PVCs. This has the effect of scoping a bound PV to a single namespace (that of the binding project).
>
> PVs are defined by a PersistentVolume API object, which represents a piece of existing networked storage in the cluster that has been provisioned by an administrator.PV objects capture the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.
>
> PVCs are defined by a PersistentVolumeClaim API object, which represents a request for storage by a developer. It is similar to a pod in that pods consume node resources and PVCs consume PV resources. For example, pods can request specific levels of resources (e.g., CPU and memory), while PVCs can request specific storage capacity and access modes (e.g, they can be mounted once read/write or many times read-only).

## Get host IP in the POD;

```yaml
        env:
        - name: JAEGER_SERVICE_NAME
          value: myapp
        - name: JAEGER_AGENT_HOST   # NOTE: Point to the Agent daemon on the Node
          valueFrom:
            fieldRef:
              fieldPath: status.hostIP

```

## Add checksum to annotation to trigger deployment/sts to upgrade when configmap/secret changed

```yaml
      annotations:
        checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}
        checksum/dashboards-json-config: {{ include (print $.Template.BasePath "/dashboards-json-configmap.yaml") . | sha256sum }}
```

## Load all environment configurations from configmap

```yaml
          envFrom:
            - configMapRef:
                name: {{ include "emqx.fullname" . }}-env 
```

## [configmap reload](https://github.com/jimmidyson/configmap-reload) is quite useful to provide no downtime configuration reload if service provides http interface to trigger that

## Helm command to search chart repository: `helm search <keyword>` like `helm search stable`

## Trouble shooting deployment issue

When trying to deploy tiller to minikube, I found that tiller-deploy is stuck. To trouble shooting such issues, you can get detail log from controller manager POD like this: `kk logs -f kube-controller-manager-minikube --tail 100`

## Access docker inside minikube: `eval $(minikube docker-env)`

## Use [envFrom](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
>Use envFrom to define all of the ConfigMap’s data as container environment variables. The key from the ConfigMap becomes the environment variable name in the Pod

## It is good reading: [Herding pods: taints, tolerations and affinity in kubernetes](https://medium.com/@betz.mark/herding-pods-taints-tolerations-and-affinity-in-kubernetes-2279cef1f982)
> Taints are repulsion and are targeted at the pods we don’t want.

This article explains well how k8s node's taints and POD's toleration affect POD scheduling, and how to write podAntiAffinity affinity to avoid collocate on the same node.

## Learn more about *Designing Distributed Systems* based on k8s

Read more about [this book](https://azure.microsoft.com/en-us/resources/designing-distributed-systems/en-us/)

## Headless service

Headless service is needed to access individual POD in a statefulset, which is important to create replica set for mongodb clustering, and nats clustering.
The only difference between headless service and noraml k8s service is that it will set clusterIP to `None`

## [top 10 kubernetes tips and tricks](https://hackernoon.com/top-10-kubernetes-tips-and-tricks-27528c2d0222)

	- bash completion is buit-in when kubectl is installed via brew on mac
	- add default memory and cpu limits to namespace
	- Use PodDisruptionBudget to protect your service from drained
	- Label is your friend in k8s world

## It is possible to remove a value by setting its value to `null` per [this issue](https://github.com/helm/helm/pull/2648)

Example: `helm upgrade staging-0 --reuse-values gezhi/fabric --set msc-java.image.processTag=null`

## [semverCompare function](http://masterminds.github.io/sprig/semver.html) syntax

The first argument to this function is version assertion, not just a version, and this function will return bool instead of an integer. Example usage is like this (which is quite similar to syntax used in requirements file)

```yaml
{{- if semverCompare ">=1.4-0, <1.7-0" .Capabilities.KubeVersion.GitVersion -}}
```

or one I crafted but is not logically correct:

```yaml
{{/*
  decide which version should be used for process
*/}}
{{- define "tide-process.version" -}}
{{- if or (not .Values.image.processTag) ((semverCompare (join ">=" .Values.image.processTag) .Values.image.tag )) -}}
{{- printf "%s" .Values.image.tag -}}
{{- else -}}
{{- printf "%s" .Values.image.processTag -}}
{{- end -}}
{{- end -}}
```

## How to check upgrade diff by `helm`?

There is helm plugin called [helm-diff](https://github.com/databus23/helm-diff) to do that.
Install by: `helm plugin install https://github.com/databus23/helm-diff --version master`.
Example usage:
	- Show upgrade diff: `helm diff upgrade ar-0 gezhi/fabric`
	- Show rollback diff: `helm diff rollback ar-0 5`

## Upgrade local added helm chart repo by `helm repo update`

## securityContext can be configured on POD or container, not just container

## [kompose](https://github.com/kubernetes/kompose/blob/master/docs/user-guide.md) is a cool tool to convert docker-compose file to k8s manifest file, and could even be used to deploy directly to k8s cluster

## [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
>A network policy is a specification of how groups of pods are allowed to communicate with each other and other network endpoints.
NetworkPolicy resources use labels to select pods and define rules which specify what traffic is allowed to the selected pods.

An example from jenkins deployment to allow only its agent to communicate with it:

```yaml
kind: NetworkPolicy
apiVersion: {{ .Values.networkPolicy.apiVersion }}
metadata:
  name: "{{ .Release.Name }}-{{ .Values.master.componentName }}"
spec:
  podSelector:
    matchLabels:
      "app.kubernetes.io/component": "{{ .Values.master.componentName }}"
      "app.kubernetes.io/instance": "{{ .Release.Name }}"
  ingress:
    # Allow web access to the UI
    - ports:
      - port: 8080
    # Allow inbound connections from slave
    - from:
      - podSelector:
          matchLabels:
            "jenkins/{{ .Release.Name }}-{{ .Values.agent.componentName }}": "true"
      ports:
      - port: {{ .Values.master.slaveListenerPort }}
```

## [Adding entries to Pod /etc/hosts with HostAliases](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)

## Use podAffinity to schedule a new cronjob POD to run together with some named POD

The following affinity tries to put backup cron job POD together with jenkins master to match `topologyKey` as `hostname`; and labelSelector works as filter; limitation here is that only one host can be selected.

```yaml
        affinity:
          podAffinity:
            preferredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                - key: "app.kubernetes.io/name"
                  operator: In
                  values:
                  - "{{ template "jenkins.name" . }}"
                - key: "app.kubernetes.io/instance"
                  operator: In
                  values:
                  - "{{ .Release.Name }}"
              topologyKey: "kubernetes.io/hostname"
```

## It is possible to load `/etc/hosts` or directly add entry to [coreDNS corefile](https://github.com/coredns/coredns/tree/master/plugin/hosts)

```ini
hosts [FILE [ZONES...]] {
    [INLINE]
    ttl SECONDS
    no_reverse
    reload DURATION
    fallthrough [ZONES...]
}
```

Just load host `/etc/hosts` file:

```ini
. {
    hosts
}
```

An example of inlining some internal dns entries into *hosts* section after `kk edit cm coredns`:

```yaml
  Corefile: |
    .:53 {
        errors
        health
        kubernetes cluster.local in-addr.arpa ip6.arpa {
          pods insecure
          upstream /etc/resolv.conf
          fallthrough in-addr.arpa ip6.arpa
        }
        hosts {
            10.0.0.8 git.gezhitech.cn
            10.0.0.5 registry.gezhitech.cn
            fallthrough
        }
        prometheus :9153
        proxy . /etc/resolv.conf
        cache 30
        loop
        reload
        loadbalance
    }

```

## It is possible to install [k8s node problem detector](https://kubernetes.io/docs/tasks/debug-application-cluster/monitor-node-health/) to check node health. But so far, seems there is no good upstream app to consume data emitted from detector.

Simply apply like this `kubectl apply -f https://k8s.io/examples/debug/node-problem-detector.yaml`, or put into kubernetes addon pods directory: `/etc/kubernetes/addons/node-problem-detector`

## Manually install `helm`

First create sa, rolebinding for tiller service account by apply the following manifests:

```yml
---
# rbac-config.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Then init helm with `helm init --service-account tiller --stable-repo-url http://mirror.azure.cn/kubernetes/charts/ --tiller-image=docker.io/googlecontainer/tiller:v2.13.1`.


## Fix CrashLoopBackOff kube-apiserver/kube-controller-manager POD

When reboot master server, sometimes, I guess due to race condition in starting sequence of kubelet and other core components containers. Those core containers might fail to start due to port is already being used ( such as 6443 for api server, 10251 for controller manager), the fix is also to first stop kubelet, kill process that is using conflict port, and restart kubelet again.

## Fix problem of failing to start *nginx-proxy* on k8s node

Error might like this: failed to create listener: failed to listen on 0.0.0.0:6443: listen tcp 0.0.0.0:6443: bind: address already in use. 

To fix this:

```sh
netstat -lutpn | grep 6443
kill <pid>
systemctl restart kubelet
```

## How to configure ingress nginx controller?

It is possible to pass a configmap name for additional nginx configuration for controller as mentioned [here](https://kubernetes.github.io/ingress-nginx/user-guide/cli-arguments/).
And [supported options](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/) is quite comprehensive.

## Is it possible to create secret accross multiple namespaces?
>Secret API objects reside in a namespace. They can only be referenced by pods in that same namespace. Basically, you will have to create the secrete for every namespace. Referecen [k8s secret](https://kubernetes.io/docs/concepts/configuration/secret/) for more detail.

## [Attach Handlers to Container Lifecycle Events](https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/)
>Kubernetes supports the postStart and preStop events. Kubernetes sends the postStart event immediately after a Container is started, and it sends the preStop event immediately before the Container is terminated.

## An easy way to check logs from pods: check logs on sts or deployment like this: `kubectl -n staging get deploy/bass`

## Syntax to set env variable value from meta, configmap, and secret

To configure an env from configmap:

```yaml
   - name: LOG_LEVEL
     valueFrom:
       configMapKeyRef:
         name: env-config
         key: log_level

```

To configure an env from secret:

```yaml
    - name: SECRET_PASSWORD
      valueFrom:
        secretKeyRef:
          name: test-secret
          key: password
```

To configure an env from meta:

```yaml
    - name: POD_NAMESPACE
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: metadata.namespace
```

## Supported functions in _helm_ chart template:

Some are defined in [golang template package](https://godoc.org/text/template),  and others are defined in [sprig package](https://godoc.org/github.com/Masterminds/sprig)
And a quick reference for _sprig_ functions is [here](https://github.com/Masterminds/sprig/blob/master/functions.go)

## [Naming convention for k8s resources](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/)

>By convention, the names of Kubernetes resources should be up to maximum length of 253 characters and consist of lower case alphanumeric characters, -, and ., but certain resources have more specific restrictions.

## How to serve a local helm chart repo?

First, package local charts by `helm package` command, and then create *index.yaml* file by calling `helm repo index`, and finally run `helm serve --repo-path .` in the chart root directory.

## How to create secret by a POD?

```yaml
containers:
  - name: create-block-secret
    image: {{ .Values.image.kubectl }}
    volumeMounts:
      - name: shared
        mountPath: /shared
      - mountPath: /orderer-data
        name: orderer-home
    env:
      - name: CHANNEL_ID
        value: composer-channel
      - name: NAMESPACE
        value: {{ .Release.Namespace }}
      - name: CHANNEL_BLOCK_SECRET_NAME
        value: {{ .Release.Name }}-channel-block
    workingDir: /shared
    command:
      - sh
      - -c
      - >
        kubectl --namespace $NAMESPACE get secret $CHANNEL_BLOCK_SECRET_NAME &>/dev/null || kubectl --namespace $NAMESPACE create secret generic $CHANNEL_BLOCK_SECRET_NAME --from-file "block=/orderer-data/configtx/${CHANNEL_ID}.block"

```

## How to delete resource by pod?

```yaml
   - name: clean-up-k8s-resources
     image: {{ .Values.image.kubectl }}
     env: 
       - name: NAMESPACE
         value: {{ .Release.Namespace }}
       - name: CHANNEL_BLOCK_SECRET_NAME
         value: {{ .Release.Name }}-channel-block
     workingDir: /shared
     command:
       - sh
       - -c
       - >
         kubectl --namespace $NAMESPACE delete secret $CHANNEL_BLOCK_SECRET_NAME &>/dev/null || true
```

## How to mount secret as file?

```yaml
   - name: channel-block
     secret:
       secretName: {{ .Release.Name }}-channel-block
       items:
         - key: block
           path: channel.block
```

## How to mount configmap as file?

```yaml
   - name: fabric-ops-scripts
     configMap:
       name: fabric-ops-scripts
       items:
       - key: create-channel.sh
        # path: create-channel.sh
       - key: join-channel.sh
        # path: join-channel.sh

```

## [Install a sample application on k8s](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/)

```sh
kubectl create namespace sock-shop
kubectl apply -n sock-shop -f "https://github.com/microservices-demo/microservices-demo/blob/master/deploy/kubernetes/complete-demo.yaml?raw=true
```

## [How to call rest API by curl](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/#without-kubectl-proxy-post-v13x)

The basic idea here is to first get token from secret for some service account with proper access to API corresponding resource. With this token as Bearer in *Authorization* HTTP header, you can access REST resource on k8s API server

```sh
$ APISERVER=$(kubectl config view | grep server | cut -f 2- -d ":" | tr -d " ")
$ TOKEN=$(kubectl describe secret $(kubectl get secrets | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t')
$ curl $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
```

 - [How to re-create Bootstrap token?](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.8.md#behavioral-changes)

> The default Bootstrap Token created with kubeadm init v1.8 expires and is deleted after 24 hours by default to limit the exposure of the valuable credential. You can create a new Bootstrap Token with kubeadm token create or make the default token permanently valid by specifying --token-ttl 0 to kubeadm init. The default token can later be deleted with kubeadm token delete.

 Be sure to add token to groups for node anthorization like this: `kubeadm token create --groups system:bootstrappers:kubeadm:deafult-node-token`

## Guides

### Workings steps for v1.8.0

## Install docker and enable/start docker
## Add a kubernets yum repo:
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

```
## On master node, install kube components (locked to v1.8.0)
```
#yum install -y kubelet kubeadm kubectl kubernetes-cni
yum install -y kubelet-1.8.0-0 kubeadm-1.8.0-0 kubectl-1.8.0-0 kubernetes-cni-1.8.0-0
```
## On master node, enable kubelet system service
```
systemctl enable kubelet

```
## On master node, init the cluster:
```
kubeadm init  --pod-network-cidr 10.244.0.0/16
```
Write down join token from output like this:
```
kubeadm join --token 7e4e01.b13d9c3559730fa1 10.112.80.131:6443 --discovery-token-ca-cert-hash sha256:c66d6fae56d5347a018751079f18855dd65b73d965aeaa025d5151d159398c49

```

## Copy admin.conf file to $HOME for `kubectl` command to work:
```
sudo cp /etc/kubernetes/admin.conf $HOME/
sudo chown $(id -u):$(id -g) $HOME/admin.conf
export KUBECONFIG=$HOME/admin.conf
```

## Install flannel network
```
kubectl create -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

### set up kube on aliyun
## [create cluster by kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/)
First [install kubeadm](https://kubernetes.io/docs/setup/independent/install-kubeadm/)

## [Pull and tag images](https://mritd.me/2016/11/30/set-up-kubernetes-cluster-by-flannel/):
```
# load 镜像
images=(kube-proxy-amd64:v1.4.6 kube-discovery-amd64:1.0 kubedns-amd64:1.7 kube-scheduler-amd64:v1.4.6 kube-controller-manager-amd64:v1.4.6 kube-apiserver-amd64:v1.4.6 etcd-amd64:2.2.5 kube-dnsmasq-amd64:1.3 exechealthz-amd64:1.1 pause-amd64:3.0 kubernetes-dashboard-amd64:v1.4.1)
for imageName in ${images[@]} ; do
  docker pull mritd/$imageName
  docker tag mritd/$imageName gcr.io/google_containers/$imageName
  docker rmi mritd/$imageName
done

# 其他的什么 dns、etcd 搞完了直接初始化
kubeadm init --api-advertise-addresses 192.168.1.101 --external-etcd-endpoints http://192.168.1.102:2379 --use-kubernetes-version v1.4.6 --pod-network-cidr 10.244.0.0/16

```

## On the master node, need the following container images:
```
    gcr.io/google_containers/kube-apiserver-amd64:v1.8.0
    gcr.io/google_containers/kube-controller-manager-amd64:v1.8.0
    gcr.io/google_containers/kube-scheduler-amd64:v1.8.0
    gcr.io/google_containers/kube-proxy-amd64:v1.8.0
    gcr.io/google_containers/etcd-amd64:3.0.17
    gcr.io/google_containers/pause-amd64:3.0
```
## On the slave node, need the following container images:
```
```

## Trouble shooting kubelet fails to start
```
local cluster fails to start Error: failed to run Kubelet: Running with swap on is not supported 
```
Fix:
[Adding KUBELET_FLAGS=${KUBELET_FLAGS:-"--fail-swap-on=false"} to hack/local-up-cluster.sh](https://github.com/kubernetes/kubernetes/issues/50373)
Or running [sudo swapoff -a](http://blog.csdn.net/csdn_duomaomao/article/details/75142769)

```
kubelet cgroup driver: "systemd" is different from docker cgroup driver: "cgroupfs"
```

[Fix](https://github.com/kubernetes/kubernetes/issues/43805):
```
kubelet's cgroup driver is not same with docker's cgroup driver, so I update systemd -> cgroupfs.

vi /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
update KUBELET_CGROUP_ARGS=--cgroup-driver=systemd to KUBELET_CGROUP_ARGS=--cgroup-driver=cgroupfs

restart kubelet
run 'service kubelet restart'

everyting is ok
```

with the above fixes, now master is inited successfully:
```
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run (as a regular user):

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  http://kubernetes.io/docs/admin/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join --token bda1d2.e97a506e04ab3a33 10.112.80.131:6443 --discovery-token-ca-cert-hash sha256:cbc107ceecd25f5dc9e31b50ca98a614d1463ca5a2c401f654f306bb07569c56
```

###  Try out [minikube](https://github.com/kubernetes/minikube)
## Install minikube  and kubectl on linux:
```
curl -Lo minikube https://stora9ge.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true
mkdir $HOME/.kube || true
touch $HOME/.kube/config

export KUBECONFIG=$HOME/.kube/config
sudo -E ./minikube start --vm-driver=none

# this for loop waits until kubectl can access the api server that minikube has created
for i in {1..150} # timeout for 5 minutes
do
   ./kubectl get po &> /dev/null
   if [ $? -ne 1 ]; then
      break
  fi
  sleep 2
done

# kubectl commands are now able to interact with minikube cluster

```
## Trouble shooting minikube failed to start due to  *AnonymousAuth is not allowed with the AllowAll authorizer*
```
On the proposed solution: as a workaround the additional line Environment='GODEBUG=netdns=go' can be applied directly to the localkube.service on the system followed by a systemctl daemon-reload and systemctl restart localkube.
```
It is said on [godoc](https://golang.org/pkg/net/):
```
export GODEBUG=netdns=go    # force pure Go resolver
export GODEBUG=netdns=cgo   # force cgo resolver
```

## Links

## [Add preStart and postStart handler to pod](https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/)
