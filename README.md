# kubernetes-mpi-operator-samples
Samples Images and YAMLS for kubernetes-mpi-operator

## Install Operator

1. Install Operator Lifecycle Manager


```bash
curl -L https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.27.0/install.sh -o ~/olm/install.sh
chmod +x ~/olm/install.sh
~/olm/install.sh v0.27.0
```

2. Install Kubernetes-mpi-operator catalog `kubectl apply -f ~/files/olm-catalog.yaml`

3. Wait until catalog is ready, check status on resource `kubectl get CatalogSource -n operators -o yaml`

4. Install subscription `kubectl apply -f ~/files/olm-subscription.yaml`

5. Wait until install plan is created and complted `kubectl get InstallPlan -n operators -o yaml`

6. Review Controller for operator is created `kubectl get pods -n operators`

7. Record name of operator controller pod for future logging

8. Log to review there are no errors when init

```bash
kubectl logs kubernetes-mpi-operator-controller-manager-<hash> -n operators
```
## Create Clusters

1. Cd to samples folder `cd ./samples`

2. Apply any of the samples `kubectl apply  -f _v1alpha1_mpich_cluster.yaml`

3. Watch for pods initialization `kubectl get pods -n irazu-ns --watch`

### Test Cluster

1. Exec into cluster

```bash
kubectl exec -it irazu-sts-0 -n irazu-ns -- bash
```

2. clone and compile sample Hello world

```bash
rm -r -f /nfs/mpitutorial && cd /nfs && git clone https://github.com/mpitutorial/mpitutorial && cd /nfs/mpitutorial/tutorials/mpi-hello-world/code && make 
```

3. Execute
```
mpi run -n 120 ./mpi_hellow_world
```