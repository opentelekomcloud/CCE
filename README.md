# Toolset for Cloud Container Engine (CCE)
## Kubernetes-Dashboard (./dashboard)
In ./dashboard folder there will be placed the current recommended Kubernetes dashboard files with additional notes to get quickly started.
### Prerequisits
* CCE cluster created with at least one cluster node
* kubectl installed and running

### Getting started

* Rollout Kubernetes Dash

```
kubctl create -f kubernetes-dashboard.yaml
```

* Create (Administrator) user account and add the cluster role binding for accessing Kubernetes dashboard.

```
kubctl create -f admin-user.yaml
kubctl create -f admin-user-role.yaml
```


