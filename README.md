# Toolset for Cloud Container Engine (CCE)

## Kubernetes-Dashboard (./dashboard)

The `./dashboard` folder contains all recommended files to run Kubernetes dashboard on Cloud Container Engine. The following advices guide your through all necessary steps to get started quickly.

### Prerequisits

* CCE cluster created with at least one cluster node
* EIP configured to at least one node or LoadBalancer setup
* kubectl installed

### Getting started

* Create Kubernetes Dashboard

```
kubctl create -f kubernetes-dashboard.yaml
```

* Create (Administrator) user account and add the cluster role binding for accessing Kubernetes dashboard.

```
kubctl create -f admin-user.yaml
kubctl create -f admin-user-role.yaml
```

* Expose the dashboard (e.g. NodePort) and make it available from outside. Change the `type: ClusterIP` to `type: NodePort` 


```
kubectl edit  svc  kubernetes-dashboard -n kube-system
...
spec:
  clusterIP: <ClusterIP>
  ports:
  - port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: NodePort
...
```

* Obtain the correct NodePort from

```
kubectl get svc  kubernetes-dashboard -n kube-system
```  
```
NAME                   TYPE       CLUSTER-IP	EXTERNAL-IP		PORT(S)				AGE
kubernetes-dashboard   NodePort   <Cluster-IP>	<none>			443:<NodePort>/TCP	37m
```

* Accessing the kubernetes-dashboard is now available from `https://<CCE-node-EIP>:<NodePort>`.
* For generating a login-Token for admin user account via kubectl, use the following commands:

```
token=$(kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}') | awk '$1=="token:"{print $2}')
echo $token
```




