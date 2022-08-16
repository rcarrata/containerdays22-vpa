# Prerequisites for Running the Demo in k8s Vanilla

These are the following steps for running the demo in your environment.

1. [Install Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installing-with-a-package-manager)

2. Create a Kind Cluster for the Demo 

```
kind create cluster --name cd22
```

3. Check that everything in your k8s cluster is up && running:

```
kubectl get pod -A --no-headers | grep -v Running
```

3. Install Kubernetes Metrics

```
kubectl apply -f assets/metrics-server.yaml
sleep 30

kubectl get pod -n kube-system -l k8s-app=metrics-server
```

3. [Install Vertical Pod Autoscaler controller](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#installation)

```
git clone https://github.com/kubernetes/autoscaler
cd autoscaler/vertical-pod-autoscaler/
./hack/vpa-up.sh
```

4. Check the Metrics for the Nodes and for the Pods:

```
kubectl top nodes
```

5. Check that the VPA is working properly:

```
kubectl apply -f examples/hamster.yaml
sleep 60
kubectl top pods
kubectl describe vpa
kubectl get vpa
kubectl delete -f examples/hamster.yaml
```

6. Run the demo in your k8s cluster:

```sh
bash assets/demo-k8s.sh
```
