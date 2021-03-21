# **Note about Kubernetes (CKAD)**

**CKAD Exercises - https://github.com/dgkanatsios/CKAD-exercises**

### **Kubenetes resources**
#### **Services:**
   A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. Services enable a loose coupling between dependent Pods. A   Service is defined using YAML (preferred) or JSON, like all Kubernetes objects. The set of Pods targeted by a Service is usually determined by a LabelSelector (see below for     why you might want a Service without including selector in the spec).

  Although each Pod has a unique IP address, those IPs are not exposed outside the cluster without a Service. Services allow your applications to receive traffic. Services can     be exposed in different ways by specifying a type in the ServiceSpec:

    . ClusterIP (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster. 
    . NodePort - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
    . LoadBalancer - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
    . ExternalName - Maps the Service to the contents of the externalName field (e.g. `foo.bar.example.com`), by returning a CNAME record with its value. No proxying of any kind is set up. This type requires v1.7 or higher of kube-dns, or CoreDNS version 0.0.8 or higher.

### **Commons Kubernetes CLI**
```
$ kubectl get pods

$ kubectl run ngnix --image=nginx

$ kubectl describe pod <<podName>>

$ kubectl get pods -o wide

$ kubectl delete pod <<podName>>

$ kubectl apply -f <<file>>

$ kubectl edit pod <<podName>> or edit the archive yaml and apply again
```

### **Create a definition pod file:**

```
$ kubectl run redis --image=redis --dry-run=client -o yaml > redis.yml
```

### **Extract definition of an existing pod:**

```
$ kubectl get pod <pod-name> -o yaml > pod-definition.yaml
```

# **Enviroments variables, configMaps and secrets**

## **Enviroments variables**
Example that how you can define environment variables to use on a POD
```
apiVersion: v1
kind: Pod
metadata:
    name: application-name
spec:
    containers:
    - name: application-name
      image: <nameOfImage>:<version>
      env:
      - name: firstEnviromentKey
        value: "firstEnviromentValue"
      - name: secondEnviromentKey
        value: "secondEnviromentValue"
```

## **ConfigMaps**

Create a new ConfigMap
```
$ kubectl create cm my-first-configmap \       
    --from-literal=firstKey=firstValue \
    --from-literal=secondKey=secondValue

```
The above command creates the following file :
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-first-configmap
  namespace: default
data:
  firstKey: firstValue
  secondKey: secondValue
```

Use the new configMap in a POD
```
apiVersion: v1
kind: Pod
metadata:
    name: application-name
spec:
    containers:
    - name: application-name
      image: <nameOfImage>:<version>
      envFrom:
        - configMapRef:
          name: my-first-configmap
```


## **Secrets**

Create a new Secret
```
$ kubectl create secret generic db-secret --from-literal=DB_PASSWORD=468573
```
The above command creates the following file :

```
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: default
data:
  DB_PASSWORD: NDY4NTcz
type: Opaque
```

Use the new Secret in a POD

```
apiVersion: v1
kind: Pod
metadata:
    name: application-name
spec:
  containers:
    - name: application-name
      image: <nameOfImage>:<version>
      envFrom:
        - secretRef:
          name: db-secret

=============================
            OR
=============================

apiVersion: v1
kind: Pod
metadata:
    name: application-name
spec:
  containers:
    - name: application-name
      image: <nameOfImage>:<version>
      env
        - name: db_password
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_PASSWORD

```
## **Security Context**

## **Service Account**

## **Taint and Tolerations**

