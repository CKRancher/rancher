We will need a VM (can be the same with the one hosting our Rancher instance) with `Google Cloud SDK` and `kubelet` installed. Make sure that gcloud has access to the Cloud Platform with Google user credentials (`gcloud init` and `gcloud auth login`).
As soon as cluster is deployed, we can check basic kubectl commands.

`gcloud container clusters get-credentials` updates a kubeconfig file with appropriate credentials and endpoint information to point kubectl at a specific cluster in Google Kubernetes Engine. 

```bash
$ gcloud container clusters get-credentials c-hg4tm
Fetching cluster endpoint and auth data.
kubeconfig entry generated for c-hg4tm.
```

```bash
$ helm version
Client: &version.Version{SemVer:"v2.15.2", GitCommit:"8dce272473e5f2a7bf58ce79bb5c3691db54c96b", GitTreeState:"clean"}
Error: could not find tiller
```

```
$ helm init --wait
$HELM_HOME has been configured at /home/rustudorcalin/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
```

```bash
$ helm version
Client: &version.Version{SemVer:"v2.15.2", GitCommit:"8dce272473e5f2a7bf58ce79bb5c3691db54c96b", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.15.2", GitCommit:"8dce272473e5f2a7bf58ce79bb5c3691db54c96b", GitTreeState:"clean"}
```

```bash
helm list
Error: configmaps is forbidden: User "system:serviceaccount:kube-system:default" cannot list resource "configmaps" in API group "" in the namespace "kube-system"
```

```bash
$ kubectl --namespace kube-system create serviceaccount tiller
serviceaccount/tiller created

$ kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
clusterrolebinding.rbac.authorization.k8s.io/tiller-cluster-rule created

$ kubectl --namespace kube-system patch deploy tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
deployment.extensions/tiller-deploy patched
```

```bash
$ helm install --namespace monitoring --name demo stable/prometheus-operator
```

<details><summary>Click for full command output</summary>

```bash  
NAME:   demo
LAST DEPLOYED: Mon Mar  9 20:56:20 2020
NAMESPACE: monitoring
STATUS: DEPLOYED

RESOURCES:
==> v1/Alertmanager
NAME                                   AGE
demo-prometheus-operator-alertmanager  35s

==> v1/ClusterRole
NAME                                     AGE
demo-grafana-clusterrole                 35s
demo-prometheus-operator-operator        35s
demo-prometheus-operator-operator-psp    35s
demo-prometheus-operator-prometheus      35s
demo-prometheus-operator-prometheus-psp  35s
psp-demo-kube-state-metrics              35s
psp-demo-prometheus-node-exporter        35s

==> v1/ClusterRoleBinding
NAME                                     AGE
demo-grafana-clusterrolebinding          35s
demo-prometheus-operator-operator        35s
demo-prometheus-operator-operator-psp    35s
demo-prometheus-operator-prometheus      35s
demo-prometheus-operator-prometheus-psp  35s
psp-demo-kube-state-metrics              35s
psp-demo-prometheus-node-exporter        35s

==> v1/ConfigMap
NAME                                                        DATA  AGE
demo-grafana                                                1     35s
demo-grafana-config-dashboards                              1     35s
demo-grafana-test                                           1     35s
demo-prometheus-operator-apiserver                          1     35s
demo-prometheus-operator-cluster-total                      1     35s
demo-prometheus-operator-controller-manager                 1     35s
demo-prometheus-operator-etcd                               1     35s
demo-prometheus-operator-grafana-datasource                 1     35s
demo-prometheus-operator-k8s-resources-cluster              1     35s
demo-prometheus-operator-k8s-resources-namespace            1     35s
demo-prometheus-operator-k8s-resources-node                 1     35s
demo-prometheus-operator-k8s-resources-pod                  1     35s
demo-prometheus-operator-k8s-resources-workload             1     35s
demo-prometheus-operator-k8s-resources-workloads-namespace  1     35s
demo-prometheus-operator-kubelet                            1     35s
demo-prometheus-operator-namespace-by-pod                   1     35s
demo-prometheus-operator-namespace-by-workload              1     35s
demo-prometheus-operator-node-cluster-rsrc-use              1     35s
demo-prometheus-operator-node-rsrc-use                      1     35s
demo-prometheus-operator-nodes                              1     35s
demo-prometheus-operator-persistentvolumesusage             1     35s
demo-prometheus-operator-pod-total                          1     35s
demo-prometheus-operator-pods                               1     35s
demo-prometheus-operator-prometheus                         1     35s
demo-prometheus-operator-proxy                              1     35s
demo-prometheus-operator-scheduler                          1     35s
demo-prometheus-operator-statefulset                        1     35s
demo-prometheus-operator-workload-total                     1     35s

==> v1/DaemonSet
NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
demo-prometheus-node-exporter  3        3        3      3           3          <none>         35s

==> v1/Deployment
NAME                               READY  UP-TO-DATE  AVAILABLE  AGE
demo-grafana                       1/1    1           1          35s
demo-kube-state-metrics            1/1    1           1          35s
demo-prometheus-operator-operator  1/1    1           1          35s

==> v1/Pod(related)
NAME                                                READY  STATUS   RESTARTS  AGE
demo-grafana-d767d786-kjvtw                         3/3    Running  0         35s
demo-kube-state-metrics-67bf64b7f4-9z96l            1/1    Running  0         35s
demo-prometheus-node-exporter-fnq56                 1/1    Running  0         35s
demo-prometheus-node-exporter-hpmfq                 1/1    Running  0         35s
demo-prometheus-node-exporter-sq2bh                 1/1    Running  0         35s
demo-prometheus-operator-operator-7d5776c444-pt844  2/2    Running  0         35s

==> v1/Prometheus
NAME                                 AGE
demo-prometheus-operator-prometheus  35s

==> v1/PrometheusRule
NAME                                                           AGE
demo-prometheus-operator-alertmanager.rules                    35s
demo-prometheus-operator-etcd                                  34s
demo-prometheus-operator-general.rules                         34s
demo-prometheus-operator-k8s.rules                             34s
demo-prometheus-operator-kube-apiserver-error                  34s
demo-prometheus-operator-kube-apiserver.rules                  34s
demo-prometheus-operator-kube-prometheus-node-recording.rules  34s
demo-prometheus-operator-kube-scheduler.rules                  34s
demo-prometheus-operator-kubernetes-absent                     35s
demo-prometheus-operator-kubernetes-apps                       34s
demo-prometheus-operator-kubernetes-resources                  34s
demo-prometheus-operator-kubernetes-storage                    35s
demo-prometheus-operator-kubernetes-system                     34s
demo-prometheus-operator-kubernetes-system-apiserver           35s
demo-prometheus-operator-kubernetes-system-controller-manager  34s
demo-prometheus-operator-kubernetes-system-kubelet             34s
demo-prometheus-operator-kubernetes-system-scheduler           34s
demo-prometheus-operator-node-exporter                         34s
demo-prometheus-operator-node-exporter.rules                   34s
demo-prometheus-operator-node-network                          34s
demo-prometheus-operator-node-time                             34s
demo-prometheus-operator-node.rules                            34s
demo-prometheus-operator-prometheus                            35s
demo-prometheus-operator-prometheus-operator                   34s

==> v1/Role
NAME                                   AGE
demo-grafana-test                      35s
demo-prometheus-operator-alertmanager  35s

==> v1/RoleBinding
NAME                                   AGE
demo-grafana-test                      35s
demo-prometheus-operator-alertmanager  35s

==> v1/Secret
NAME                                                TYPE    DATA  AGE
alertmanager-demo-prometheus-operator-alertmanager  Opaque  1     35s
demo-grafana                                        Opaque  3     35s

==> v1/Service
NAME                                              TYPE       CLUSTER-IP     EXTERNAL-IP  PORT(S)           AGE
demo-grafana                                      ClusterIP  10.27.244.232  <none>       80/TCP            35s
demo-kube-state-metrics                           ClusterIP  10.27.247.38   <none>       8080/TCP          35s
demo-prometheus-node-exporter                     ClusterIP  10.27.247.205  <none>       9100/TCP          35s
demo-prometheus-operator-alertmanager             ClusterIP  10.27.240.11   <none>       9093/TCP          35s
demo-prometheus-operator-coredns                  ClusterIP  None           <none>       9153/TCP          35s
demo-prometheus-operator-kube-controller-manager  ClusterIP  None           <none>       10252/TCP         35s
demo-prometheus-operator-kube-etcd                ClusterIP  None           <none>       2379/TCP          35s
demo-prometheus-operator-kube-proxy               ClusterIP  None           <none>       10249/TCP         35s
demo-prometheus-operator-kube-scheduler           ClusterIP  None           <none>       10251/TCP         35s
demo-prometheus-operator-operator                 ClusterIP  10.27.248.153  <none>       8080/TCP,443/TCP  35s
demo-prometheus-operator-prometheus               ClusterIP  10.27.246.9    <none>       9090/TCP          35s

==> v1/ServiceAccount
NAME                                   SECRETS  AGE
demo-grafana                           1        35s
demo-grafana-test                      1        35s
demo-kube-state-metrics                1        35s
demo-prometheus-node-exporter          1        35s
demo-prometheus-operator-alertmanager  1        35s
demo-prometheus-operator-operator      1        35s
demo-prometheus-operator-prometheus    1        35s

==> v1/ServiceMonitor
NAME                                              AGE
demo-prometheus-operator-alertmanager             34s
demo-prometheus-operator-apiserver                34s
demo-prometheus-operator-coredns                  34s
demo-prometheus-operator-grafana                  34s
demo-prometheus-operator-kube-controller-manager  34s
demo-prometheus-operator-kube-etcd                34s
demo-prometheus-operator-kube-proxy               34s
demo-prometheus-operator-kube-scheduler           34s
demo-prometheus-operator-kube-state-metrics       34s
demo-prometheus-operator-kubelet                  34s
demo-prometheus-operator-node-exporter            34s
demo-prometheus-operator-operator                 34s
demo-prometheus-operator-prometheus               34s

==> v1beta1/ClusterRole
NAME                     AGE
demo-kube-state-metrics  35s

==> v1beta1/ClusterRoleBinding
NAME                     AGE
demo-kube-state-metrics  35s

==> v1beta1/MutatingWebhookConfiguration
NAME                                AGE
demo-prometheus-operator-admission  35s

==> v1beta1/PodSecurityPolicy
NAME                                   PRIV   CAPS      SELINUX           RUNASUSER  FSGROUP    SUPGROUP  READONLYROOTFS  VOLUMES
demo-grafana                           false  RunAsAny  RunAsAny          RunAsAny   RunAsAny   false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
demo-grafana-test                      false  RunAsAny  RunAsAny          RunAsAny   RunAsAny   false     configMap,downwardAPI,emptyDir,projected,secret
demo-kube-state-metrics                false  RunAsAny  MustRunAsNonRoot  MustRunAs  MustRunAs  false     secret
demo-prometheus-node-exporter          false  RunAsAny  RunAsAny          MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim,hostPath
demo-prometheus-operator-alertmanager  false  RunAsAny  RunAsAny          MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
demo-prometheus-operator-operator      false  RunAsAny  RunAsAny          MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
demo-prometheus-operator-prometheus    false  RunAsAny  RunAsAny          MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim

==> v1beta1/Role
NAME          AGE
demo-grafana  35s

==> v1beta1/RoleBinding
NAME          AGE
demo-grafana  35s

==> v1beta1/ValidatingWebhookConfiguration
NAME                                AGE
demo-prometheus-operator-admission  34s


NOTES:
The Prometheus Operator has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=demo"

Visit https://github.com/coreos/prometheus-operator for instructions on how
to create & configure Alertmanager and Prometheus instances using the Operator.
```
</details></br>

kubectl get pods
kubectl port-forward --address 0.0.0.0 -n monitoring alertmanager-demo-prometheus-operator-alertmanager-0 9093  >/dev/null 2>&1 &
kubectl port-forward --address 0.0.0.0 -n monitoring prometheus-demo-prometheus-operator-prometheus-0 9090  >/dev/null 2>&1 &

kubectl -n monitoring delete prometheusrules $(kubectl -n monitoring get prometheusrules | grep -v alert)
kubectl -n monitoring get prometheusrules
kubectl -n monitoring delete prometheusrules $(kubectl -n monitoring get prometheusrules | grep -v alert)
kubectl -n monitoring edit prometheusrules demo-prometheus-operator-alertmanager.rules
kubectl -n monitoring describe prometheusrules demo-prometheus-operator-alertmanager.rules

cat alertmanager.yaml
cat alertmanager-secret-k8s.yaml
sed "s/ALERTMANAGER_CONFIG/$(cat alertmanager.yaml | base64 -w0)/g" alertmanager-secret-k8s.yaml | kubectl apply -f -

cat create_deploy.yml
kubectl apply -f create_deploy.yml

kubectl get pods
kubectl exec -it nginx-deployment-5754944d6c-dj42p -- /bin/bash



![01](images/01-rancher-redis-cluster-architecture.png)
