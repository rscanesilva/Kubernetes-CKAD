# Note about Kubernetes-CKAD

### Commons Kubernetes CLI 
```
$ kubectl get pods

$ kubectl run ngnix --image=nginx

$ kubectl describe pod <<podName>>

$ kubectl get pods -o wide

$ kubectl delete pod <<podName>>

$ kubectl apply -f <<file>>

$ kubectl edit pod <<podName>> or edit the archive yaml and apply again
```
**Create a definition pod file:**

- $ kubectl run redis --image=redis --dry-run=client -o yaml > redis.yml

**Extract definition of an existing pod:**

- $ kubectl get pod <pod-name> -o yaml > pod-definition.yaml
