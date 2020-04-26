# Install HokStack using Hok Operator

## Adding HoK operator to kubernetes cluster
* Clone `hok-operator` repo

```
$ git clone github.com/hokstack/hok-operator.git
```
*  Create `hokstack` namespace

```
$ kubect create ns hokstack
```
* Navigate to `deploy/crds` and create CRD

```
$ cd deploy/crds
```
* Create the ClusterRole, ClusterRoleBinding and ServiceAccount
  
```
$ cd ../
$ kubectl apply -f service_account.yaml -n hokstack
$ kubectl apply -f clusterrole.yaml
```

* Apply cluster role binding

```
$ kubectl apply -f cluster_role_binding.yaml
```

* Deploy the Operator

```
$ kubectl apply -f operator.yaml -n hokstack
```

## Checking operator health

```
kubectl logs <hokstack operator pod> -f -n hokstack
```

## Installing first cluster
* Use [team1.yaml](https://github.com/hokstack/hok-operator/tree/master/deploy/team1.yaml)

* Create `team1` Namespace
```
$ kubectl create ns team1
```
* Apply team1 cr.
  
```
$ kubectl create -f team1.yaml -n team1
```
* Check the cluster status

```
$ kubectl get pods -n team1
$ kubect logs <cluster-watch-pod> -n team1 -f
```
* Check hokstack 

```
$ kubectl get hokstack
```

## Installing second cluster

* Use [team2.yaml](https://github.com/hokstack/hok-operator/tree/master/deploy/team2.yaml)

## Destroying Cluster

```
$ kubectl delete hokstack <name> -n <namespace>
```
