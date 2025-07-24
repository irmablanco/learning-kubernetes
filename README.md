# 🐳 Kubernetes Quick Commands

This guide provides common Kubernetes commands for working with pods.

---

## 📘 Get Pods in Namespace

Use the following command to list all pods within a specific Kubernetes namespace.

### 🔧 Command

```bash
kubectl get pods -n <namespace>
```

## 🛠️ Use the `kubectl run` Command with the Correct Image

To launch a pod using a specific container image, use the `kubectl run` command followed by the pod name and the `--image` flag.

### 🔧 Command

```bash
kubectl run <pod-name> --image=<image-name>
```

### 🔧 Describe pod by id to get info about image...

## 🛠️ Use the `kubectl describe` with the correct pod name and pod id
```bash
kubectl describe pod podname-<id>
```

### 🔧 Delete a pod

## 🛠️ Use the `kubectl delete pod` 
```bash
kubectl delete pod podname
```

### 🔧 Create a new pod with the name redis and the image redis123

## 🛠️ We use `kubectl run` command with `--dry-run=client -o yaml` option to create a manifest file
```bash
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
```

## 🛠️ After that, using `kubectl create -f` command to create a resource from the manifest file
```bash
kubectl create -f redis-definition.yaml
```

### 🔧 Change the image on a given pod

## 🛠️ We use `kubectl edit` command to update the image on a given pod
```bash
kubectl edit pod redis
```

## 🛠️ If you used a pod definition file then update the image from redis123 to redis in the definition file
via Vi or Nano editor and then run kubectl apply command to update the image:
```bash
kubectl apply -f redis-definition.yaml
```