# **Note about Kubernetes (CKAD)**

**CKAD Exercises - https://github.com/dgkanatsios/CKAD-exercises**
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

