# kubernetes-mastery
These projects are a complete guide to study Kubernetes based on Udemy course [Kubernetes Mastery: Hands-On Lessons From A Docker Captain](https://www.udemy.com/course/kubernetesmastery/) created by [Bret Fisher](https://www.udemy.com/user/bretfisher/).

### Slides

https://slides.kubernetesmastery.com/

### Kubernetes Documentation

- [Kubernetes Distributions](https://kubernetes.io/partners/#conformance)
- [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)
- [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) 
- [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)


### Kubernetes Architecture

![](./images/k8s-arch.png)
![](./images/k8s-arch2.png)

### MicroK8s

[MicroK8s](https://microk8s.io/) is a low-ops, minimal production Kubernetes.

To install MicroK8s
```
sudo snap install microk8s --classic
```

To run **microk8s** without needing sudo, execute the following commands. After this it is necessary to logout/login to update your user groups.
```
sudo usermod -a -G microk8s $USER
sudo chown -R $USER ~/.kube
```

To enable CoreDNS
```
microk8s enable dns
```

To check status
```
microk8s status
```

### shpod

Provides a shell running in a pod on the cluster.

To run **shpod**
```
kubectl apply -f https://k8smastery.com/shpod.yaml
```

To attach shell
```
kubectl attach --namespace=shpod -ti shpod
```

To delete **shpod**
```
kubectl delete -f https://k8smastery.com/shpod.yaml
```

### CLI

**kubectl** is a rich CLI tool arround the Kubernetes API. It controls the Kubernetes cluster manager.

The **kubectl** config file is located in **~/.kube/config** with the follwing data:

- the Kubernetes API address
- the path to TLS certificates used to authenticate

You can set another config file to execute the **kubectl** commands using the **--kubeconfig** flag. If the flag is not present, the config file that will be used is the one located in **~/.kube/config**. Another option to set the config file is setting an environment variable called **KUBECONFIG**.

You can see the configuration data using
```
kubectl config view
```
or
```
cat ~/.kube/config
```

The **kubectl** CLI is composed by a command following by the Kubernetes resource where this command will be executed.
```
kubectl <command> <resource>
```

**Resource types**

| name                      | shortname | apiversion            | namespaced | kind                     |
|---------------------------|-----------|-----------------------|------------|--------------------------|
| configmaps                | cm        | v1                    | true       | ConfigMap                |
| endpoints                 | ep        | v1                    | true       | Endpoints                |
| events                    | ev        | v1                    | true       | Event                    |
| namespaces                | ns        | v1                    | false      | Namespace                |
| nodes                     | no        | v1                    | false      | Node                     |
| persistentvolumeclaims    | pvc       | v1                    | true       | PersistentVolumeClaim    |
| persistentvolumes         | pv        | v1                    | false      | PersistentVolume         |
| pods                      | po        | v1                    | true       | Pod                      |
| replicationcontrollers    | rc        | v1                    | true       | ReplicationController    |
| secrets                   |           | v1                    | true       | Secret                   |
| serviceaccounts           | sa        | v1                    | true       | ServiceAccount           |
| services                  | svc       | v1                    | true       | Service                  |
| daemonsets                | ds        | apps/v1               | true       | DaemonSet                |
| deployments               | deploy    | apps/v1               | true       | Deployment               |
| replicasets               | rc        | apps/v1               | true       | ReplicaSet               |
| statefulsets              | sts       | apps/v1               | true       | StatefulSet              |
| horizontalpodautoscalers  | hpa       | autoscaling/v2        | true       | HorizontalPodAutoscaler  |
| cronjobs                  | cj        | batch/v1              | true       | CronJob                  |
| ingresses                 | ing       | networking.k8s.io/v1  | true       | Ingress                  |
| storageclasses            | sc        | storage.k8s.io/v1     | false      | StorageClass             |

### API

To create a proxy in a localhost port to access the Kubernetes API:
```
kubectl proxy --port=8080 &
```

To check if API proxy is working try to call the endpoint **/version**:
```
curl --location 'http://localhost:8080/version'
```
or **/api**
```
curl --location 'http://localhost:8080/api'
```

**Resource endpoints**

| name                      | endpoint                                      | kind                          |
|---------------------------|-----------------------------------------------|-------------------------------|
| configmaps                | /api/v1/configmaps                            | ConfigMapList                 |
| endpoints                 | /api/v1/endpoints                             | EndpointsList                 |
| events                    | /api/v1/events                                | EventList                     |
| namespaces                | /api/v1/namespaces                            | NamespaceList                 |
| nodes                     | /api/v1/nodes                                 | NodeList                      |
| persistentvolumeclaims    | /api/v1/persistentvolumeclaims                | PersistentVolumeClaimList     |
| persistentvolumes         | /api/v1/persistentvolumes                     | PersistentVolumeList          |
| pods                      | /api/v1/pods                                  | PodList                       |
| replicationcontrollers    | /api/v1/replicationcontrollers                | ReplicationControllerList     |
| secrets                   | /api/v1/secrets                               | SecretList                    |
| serviceaccounts           | /api/v1/serviceaccounts                       | ServiceAccountList            |
| services                  | /api/v1/services                              | ServiceList                   |
| daemonsets                | /apis/apps/v1/daemonsets                      | DaemonSetList                 |
| deployments               | /apis/apps/v1/deployments                     | DeploymentList                |
| replicasets               | /apis/apps/v1/replicasets                     | ReplicaSetList                |
| statefulsets              | /apis/apps/v1/statefulsets                    | StatefulSetList               |
| horizontalpodautoscalers  | /apis/autoscaling/v2/horizontalpodautoscalers | HorizontalPodAutoscalerList   |
| cronjobs                  | /apis/batch/v1/cronjobs                       | CronJobList                   |
| ingresses                 | /apis/networking.k8s.io/v1/ingresses          | IngressList                   |
| storageclasses            | /apis/storage.k8s.io/v1/storageclasses        | StorageClassList              |


You can access all these endpoints using Postman [collection](./postman/kubernetes.postman_collection.json) running in this [environament](./postman/kubernetes.postman_environment.json).

### describe

Retrieves some extra human-readable informations about a kind of resource. You need to specify the name of resource you wanna describe.

CLI
```
kubectl describe node/<node>
kubectl describe node <node>
```

### get

List a kind of resources inside the Kubernetes cluster.

CLI
```
kubectl get nodes
kubectl get node
kubectl get no
```  

You can change the output using **-o** flag:

| output | description              |
|--------|--------------------------|
| wide   | Display more information |
| json   | Display in JSON format   |
| yaml   | Display in YAML format   |

To extract a specific information from the output in JSON format, you can use **jq** utility. For instance:

```
kubectl get nodes -o json | jq ".items[] | {name:.metadata.name} + .status.capacity"
```

To install **jq** utility:
```
sudo snap install jq
```

API
```
curl --location $API_HOST'/api/v1/nodes'
```
