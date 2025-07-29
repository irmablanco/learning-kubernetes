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

## 📘 Get replicasets in Namespace

Use the following command to list all pods within a specific Kubernetes namespace.

### 🔧 Command

```bash
kubectl get replicaset
```

```bash
kubectl run <pod-name> --image=<image-name>
```

### 🔧 Describe replicaset

## 🛠️ Use the `kubectl describe` with the correct pod name and pod id
```bash
kubectl kubectl describe replicaset
```

### 🔧 Fix error in replicaset yml

## 🛠️ Open with nano or vim
```bash
nano /root/replicaset-definition-1.yaml
```

## 🛠️ Apply changes
```bash
kubectl apply -f /root/replicaset-definition-1.yaml
```


### 🔧 scale up replicaset

## 🛠️ Open and edit
```bash
kubectl edit rs new-replica-set
```

## 📘 Get all objects (pods, replicasets and deployments) in Namespace


### 🔧 Command

```bash
kubectl get all
```

## 📘 Get info about the deployment object


### 🔧 Use `kubectl describe deployment` command

```bash
kubectl kubectl describe deployment
```

### 🔧 Fix error in deployment yml

## 🛠️ Open with nano or vim
```bash
vim /root/deployment-definition-1.yaml
```

## 🛠️ Apply changes
```bash
kubectl apply -f /root/deployment-definition-1.yaml
```

### 🔧 Create a new Deployment needed attributes using your own deployment definition file.

## 🛠️ Create a deployment.ytm file with the needed attributes
```bash
kubectl create deployment httpd-frontend --image=httpd:2.4-alpine --replicas=3 -o yaml > deployment.yaml
```

## 🛠️ Apply changes
```bash
kubectl apply -f deployment.yaml
```

### 🔧 Create a new Deployment using kubectl directly
```bash
kubectl create deployment httpd-frontend --image=httpd:2.4-alpine
kubectl scale deployment httpd-frontend --replicas=3
```


## 📘 Get Namespace

### 🔧 Command

```bash
kubectl get namespace
```

## 📘 Get pods in a given namespace

### 🔧 Command

```bash
kubectl get pods --namespace=research
```

## 📘 Get pods in all namespaces

### 🔧 Command

```bash
kubectl get pods --all-namespaces
```


## 📘 Create a pod (redis) in a given namespace (finance)

### 🔧 Command

```bash
kubectl run redis --image=redis -n finance
```


## 🛠️ Set Environment Variables

You can define environment variables for containers using the `env` field inside the `spec.containers` section of a Pod YAML:

```yaml
spec:
  containers:
    - name: my-container
      image: ubuntu
      command: ["sleep"]
      args: ["3600"]
      env:
        - name: MY_VAR
          value: "Hello world"
```

# Understanding `ENTRYPOINT` and `CMD` in Docker vs `command` and `args` in Kubernetes

### In Docker:

- **`ENTRYPOINT`** — defines the **main executable** that always runs.
- **`CMD`** — provides the **default arguments** to that executable.

When you run a container, Docker combines them like this:

```bash
ENTRYPOINT + CMD
```

### Example Dockerfile:

ENTRYPOINT ["sleep"]
CMD ["10"]

Running this container is equivalent to:

sleep 10

**In Kubernetes:

    command corresponds to Docker's ENTRYPOINT — the executable to run.

    args corresponds to Docker's CMD — the arguments passed to the executable.

Example Pod spec snippet:

command: ["sleep"]
args: ["10"]

Kubernetes runs:

sleep 10

### Why is this useful 

Why is this useful?

    You can override the default command or args without changing the container image.

    For example, if your container image runs a web server by default, but for debugging you want to run a shell instead, you can override the command to ["/bin/sh"].

    Or if your container runs a script with default args, you can change just the args for different behavior.

## 🔐 Set Environment Variables from ConfigMap and Secret

Kubernetes allows you to set environment variables in a pod dynamically using values from a `ConfigMap` or a `Secret`. This is done using the `valueFrom` field in the container's `env` section.

### 📦 From a ConfigMap

To use a value from a `ConfigMap`, use the `configMapKeyRef` field:

```yaml
spec:
  containers:
    - name: my-container
      image: ubuntu
      command: ["sleep"]
      args: ["3600"]
      env:
        - name: CONFIG_VAR
          valueFrom:
            configMapKeyRef:
              name: my-config         # Name of the ConfigMap
              key: config-key         # Key to fetch the value from
```

A volume in a Kubernetes Pod is a way to provide persistent or shared storage to containers within the Pod. Unlike the container’s local filesystem (which is ephemeral and wiped out when the container restarts), volumes are designed to persist data across container restarts or be shared between containers in the same Pod.
📦 What Is a Volume?

    Definition: A volume is a directory accessible to the containers in a Pod, backed by a storage medium like a local disk, cloud disk, secret, config, etc.

🧠 Why Use Volumes?

    Share data between containers in a Pod.

    Persist data across container restarts (though not across Pod restarts unless using persistent volumes).

    Mount secrets or config files as files.

    Mount external storage (like NFS, AWS EBS, etc.).

# 🔐 Using Kubernetes Secrets in Pods

Kubernetes **Secrets** let you store and manage sensitive information such as passwords, tokens, or SSH keys. They help protect this data from being exposed in container specs or images.

---

## 📘 Official Docs

Refer to the official Kubernetes documentation:  
[Secrets | Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/)

---

## 🧾 What is a Secret?

A `Secret` is a Kubernetes object used to hold sensitive information. Types include:

- `Opaque` (default): Arbitrary key-value pairs
- `kubernetes.io/basic-auth`
- `kubernetes.io/dockerconfigjson`
- `kubernetes.io/tls`

---

## 🛠️ Creating a Secret

### Using `kubectl`:

```bash
kubectl create secret generic my-secret \
  --from-literal=username=admin \
  --from-literal=password='1f2d1e2e67df'
```

### Using YAML

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: YWRtaW4=        # base64 for "admin"
  password: MWYyZDFlMmU2N2Rm # base64 for "1f2d1e2e67df"
```

### Inject a Secret as a Volume:

Create the secret via `kubectl:

```bash
  kubectl create secret generic my-secret \
  --from-literal=username=admin \
  --from-literal=password='s3cr3t'

```

Use yml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-volume-pod
spec:
  containers:
    - name: app-container
      image: busybox
      command: ["sh", "-c", "cat /etc/secrets/username && cat /etc/secrets/password"]
      volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secrets"
          readOnly: true
  volumes:
    - name: secret-volume
      secret:
        secretName: my-secret
```

Apply with 
```bash 
  kubectl apply -f my-secret.yaml
```

Result in Container Filesystem

Once mounted, the contents of the secret are available as individual files:

`/etc/secrets/username   → contains: admin`

`/etc/secrets/password   → contains: s3cr3t`



# 🔐 Using Service Accounts in Kubernetes Pods

A **ServiceAccount** in Kubernetes is an identity assigned to a Pod to authenticate with the Kubernetes API server. You can use it to control what a Pod is allowed to do using **RBAC (Role-Based Access Control)**.

---

## 📘 Official Docs

- [Service Accounts | Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)
- [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

---

## 🧾 What Is a Service Account?

- Automatically used by Pods to access the Kubernetes API.
- By default, every namespace has a `default` service account.
- You can create custom service accounts with specific permissions.

---

## 🛠️ Creating a Service Account

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
```

# 🔐 Using a ServiceAccount Token in a Pod

This example shows how to create a Kubernetes `ServiceAccount`, request a token, and mount it into a Pod so your application can authenticate to the Kubernetes API or other services using that token.

---

## 🛠 Step 1: Create a ServiceAccount

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-service-account
```

## 🧾 Step 2: Generate a Token (Optional – for external apps)

If you're running your app outside the cluster, use this command to generate a token using the TokenRequest API (Kubernetes v1.24+):

`kubectl create token app-service-account`

You can use this token in any external app by setting the Authorization: Bearer <token> header.

##📦 Step 3: Mount the Token into a Pod

The ServiceAccount token will automatically be mounted at:
`/var/run/secrets/kubernetes.io/serviceaccount/token`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-using-sa
spec:
  serviceAccountName: app-service-account
  containers:
    - name: myapp
      image: curlimages/curl
      command: ["sh", "-c"]
      args:
        - |
          echo "Using token to access API"
          TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
          curl -s --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
            -H "Authorization: Bearer $TOKEN" \
            https://kubernetes.default.svc/api
```

🔐 Notes

    Kubernetes mounts the token and CA cert automatically when you assign a serviceAccountName.

    You can control whether the token is mounted using:

`automountServiceAccountToken: false`

    The API server URL inside a cluster is always:
    https://kubernetes.default.svc

✅ Use Cases

    Custom applications that interact with the Kubernetes API

    Controllers, operators, or sidecars needing access

    Secure service-to-service authentication



## 🔐 Exploring the Default ServiceAccount in Kubernetes Pods for kubernetes < v1.24,

Kubernetes automatically assigns a **ServiceAccount** to every Pod. If you don’t explicitly specify one, the **`default` ServiceAccount** in the Pod’s namespace is used.

---

## ✅ What Gets Mounted Into the Pod?

When a Pod is created, Kubernetes automatically mounts the following files at:

`/var/run/secrets/kubernetes.io/serviceaccount/`


These include:

| File        | Purpose                                                      |
|-------------|--------------------------------------------------------------|
| `token`     | JWT token used to authenticate to the Kubernetes API         |
| `ca.crt`    | Cluster CA certificate for verifying the API server's identity |
| `namespace` | The namespace the Pod is running in                          |

---

## 🧪 Demo: Inspecting the ServiceAccount Files Inside a Pod

### 📄 Pod Definition (`pod-definition.yml`)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-kubernetes-dashboard
spec:
  containers:
    - name: my-kubernetes-dashboard
      image: my-kubernetes-dashboard
```

📌 Commands to Run

    Create the Pod:

`kubectl create -f pod-definition.yml`

Check the available ServiceAccounts in the namespace:

`kubectl get serviceaccount`

You should see something like:

NAME      SECRETS   AGE
default   1         5d

Describe the Pod to see which ServiceAccount is used:

`kubectl describe pod my-kubernetes-dashboard`

Look for the Service Account: and the automatically mounted volume under Volumes.

Inspect the mounted ServiceAccount files inside the Pod:

`kubectl exec -it my-kubernetes-dashboard -- ls /var/run/secrets/kubernetes.io/serviceaccount`

Expected output:

    ca.crt
    namespace
    token

🔍 Why This Is Useful

    No manual setup is needed to use a ServiceAccount token inside a Pod.

    This token can be used by applications (e.g., Prometheus, Vault, custom controllers) to securely authenticate to the Kubernetes API.

    Kubernetes automatically rotates and manages this token.

🔐 Default ServiceAccount Permissions

The default ServiceAccount is intentionally very limited:

    It only has permissions to make basic Kubernetes API requests, such as:

        Reading information about itself

        Basic access to the namespace it's running in

⚠️ It cannot:

    List or modify Pods, Nodes, or Secrets

    Access resources across namespaces

    Perform actions like create/delete/update resources

If your application needs additional permissions, you should:

    Create a custom ServiceAccount

    Use RBAC (Role + RoleBinding) to assign only the permissions it needs

    Reference it using serviceAccountName: in the Pod or Deployment spec

🛡️ Security Note

Always follow the principle of least privilege:

    Use the default ServiceAccount for basic apps that don’t need API access.

    Use custom ServiceAccounts with RBAC for anything else.

🔍 automountServiceAccountToken: false

This disables Kubernetes' default behavior of automatically mounting the service account token at:

`/var/run/secrets/kubernetes.io/serviceaccount/token`

Use this when:

    The app does not need to talk to the Kubernetes API

    You want to reduce the Pod's permissions surface

    You use an external method for secrets or credentials (e.g., Vault, OIDC, etc.)


# 🔐 Kubernetes ServiceAccounts, Tokens, and Pod Authentication (v1.24+)

This guide explains how ServiceAccounts and tokens work in modern Kubernetes (v1.24+), including how to assign them to Pods, disable auto-mounting, and use the TokenRequest API for secure, short-lived tokens.

---

## 📌 Key Changes Since Kubernetes v1.24

| Feature                                 | Before v1.24       | v1.24 and Later              |
|----------------------------------------|--------------------|------------------------------|
| Token stored as Secret                 | ✅ Yes             | ❌ No                        |
| Token auto-generated for every SA      | ✅ Yes             | ❌ No                        |
| Token mounted in Pod                   | ✅ Yes (static)    | ✅ Yes (projected, short-lived) |
| Token lifetime                         | 🔁 Infinite        | ⏱️ Configurable (e.g., 1h)     |
| Token audience scope                   | ❌ No              | ✅ Yes                        |

---

## 📦 Default ServiceAccount Behavior

Every Kubernetes **namespace** has a `default` ServiceAccount.

When a Pod is created without specifying a `serviceAccountName`, Kubernetes:

- Automatically assigns the `default` ServiceAccount
- Automatically **projects a short-lived token** into the Pod at:

`/var/run/secrets/kubernetes.io/serviceaccount/token`


You can inspect this with:

```bash
kubectl exec -it <pod-name> -- ls /var/run/secrets/kubernetes.io/serviceaccount
```
Output:

ca.crt
namespace
token

✅ Using a Custom ServiceAccount in a Pod
1. Create a ServiceAccount

```yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dashboard-sa
```
`kubectl apply -f service-account.yaml`

2. Assign It to a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-kubernetes-dashboard
spec:
  serviceAccountName: dashboard-sa
  containers:
    - name: my-kubernetes-dashboard
      image: my-kubernetes-dashboard
```      

    The token will automatically be projected using the TokenRequest API. No volume mounts needed.

3. (Optional) Disable Auto-Mounting the Token

If your app does not need to access the Kubernetes API, you can disable the token mount:

```yaml
spec:
  serviceAccountName: dashboard-sa
  automountServiceAccountToken: false
```


4. Add RBAC Permissions (If API Access Is Needed)

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: dashboard-read-pods
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dashboard-read-pods-binding
subjects:
  - kind: ServiceAccount
    name: dashboard-sa
    namespace: default
roleRef:
  kind: Role
  name: dashboard-read-pods
  apiGroup: rbac.authorization.k8s.io
```
🔑 Accessing the API Using the Mounted Token

From inside the Pod:

`TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)`
`curl -s --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
  -H "Authorization: Bearer $TOKEN" \
  https://kubernetes.default.svc/api`

🔄 On-Demand Token via kubectl (v1.24+)

If you need a token for an external app (not inside a Pod), generate a short-lived token like this:

kubectl create token dashboard-sa

You can use this token for Kubernetes API access with scoped audiences and expiry.


`kubectl get pod my-kubernetes-dashboard -o yaml`



## 📥 What You’re Seeing: Token Projection Instead of Secret

In Kubernetes v1.24+, when you inspect a Pod’s YAML, you no longer see a Secret mounted for the service account token.
🔍 Instead, you’ll see something like this under volumes:

```yaml
volumes:
  - name: kube-api-access-<random>
    projected:
      sources:
        - serviceAccountToken:
            audience: kubernetes.default.svc
            expirationSeconds: 3607
            path: token
```            

This is called a projected volume, and it's powered by the TokenRequest API.
###🔑 What This Means

    The token is no longer stored in a Secret (kubectl get secret won’t show it).

    Kubernetes generates a short-lived, audience-scoped token on demand when the Pod starts.

    This token is projected into the Pod using the TokenRequest API.

    It is mounted at:

    `/var/run/secrets/kubernetes.io/serviceaccount/token`

So your Pod still gets a token — but it’s securely managed, ephemeral, and not visible through kubectl get secret.
📌 Example: What You’ll See in `kubectl get pod -o yaml`

```yaml
serviceAccountName: default
automountServiceAccountToken: true
volumes:
  - name: kube-api-access-4vghz
    projected:
      sources:
        - serviceAccountToken:
            expirationSeconds: 3607
            path: token
            audience: kubernetes.default.svc
 ```           

✅ TL;DR
Feature	Pre v1.24	v1.24+
Token stored as Secret	✅ Yes	❌ No
Pod references the token Secret	✅ Yes	❌ No (uses projected volume instead)
Pod still receives a token	✅ Yes	✅ Yes (via TokenRequest API)
Token lifetime	🔁 Infinite	⏱️ Short-lived (~1hr default)

✅ Summary
Step	Task
1	Create a custom ServiceAccount
2	Assign it to your Pod using serviceAccountName
3	(Optional) Disable token mounting if not needed
4	Grant RBAC permissions if API access is required
5	Use kubectl create token for external apps
🛡️ Best Practices

    Do not rely on legacy Secrets for tokens — they’re deprecated.

    Always use short-lived, projected tokens.

    Disable automountServiceAccountToken unless the token is needed.

    Use least privilege RBAC when assigning roles.

    Set token audience and expiration when using TokenRequest API.

# 🏷️ Labels, Selectors, and Annotations in Kubernetes

Kubernetes uses **labels**, **selectors**, and **annotations** to organize, filter, and manage resources.

---

## 🏷️ Labels

Labels are key-value pairs used to categorize and group Kubernetes objects. They are commonly used in selectors for Services, Deployments, and `kubectl` queries.

### 📌 Common Labeling Strategy

| Label     | Purpose                                 | Example         |
|-----------|------------------------------------------|-----------------|
| `app`     | Identifies the application/component     | `app: nginx`    |
| `function`| Describes the role or functional purpose | `function: web` |

### 🔧 Example Pod with Labels

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    app: nginx
    function: web
spec:
  containers:
    - name: nginx
      image: nginx
```      

🎯 Label Selectors

Selectors allow you to filter and match resources based on their labels.
They are commonly used in:

    Services (to select Pods)

    ReplicaSets, Deployments

    kubectl commands

🔍 Types of Selectors
1. Equality-based Selectors

Selects resources that match an exact value.

```yaml
selector:
  matchLabels:
    app: nginx
    function: web
```


This selects resources with the label: app=web.
2. Set-based Selectors

Allows complex selection using in, notin, and exists.

```yaml
selector:
  matchExpressions:
    - key: env
      operator: In
      values: [production, staging]
```      

💡 Example: Service Selecting Pods

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 8080
```
This service will send traffic to Pods with the label app=web.
📝 Annotations

Annotations are also key-value pairs, but unlike labels:

    They are not used for selection or filtering.

    They store arbitrary metadata (often used by tools or controllers).

    Used for non-identifying info like:

        Build version

        Documentation links

        Monitoring config

        External tooling (e.g., kubectl.kubernetes.io/last-applied-configuration)

🔧 Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: annotated-pod
  annotations:
    description: "This pod runs the web frontend"
    contact: "team@example.com"
spec:
  containers:
    - name: nginx
      image: nginx
```      

🧠 Labels vs Annotations
Feature	Labels	Annotations
Used for	Selection, grouping	Metadata, tooling info
Filterable	✅ Yes	❌ No
Max size	Small (limited in number/size)	Larger, for long text or blobs
CLI use	Used in kubectl get, kubectl delete, etc.	Mostly informational
✅ Best Practices

    Use labels to organize and query objects (e.g., by app, tier, environment).

    Use annotations to store metadata or integration data (e.g., Prometheus scrape configs, last applied state).

    Prefer matchLabels for simple selectors and matchExpressions for complex logic.

📌 Bonus: Querying with kubectl

# Get all pods with label app=web
`kubectl get pods -l app=web`

# Get all resources with label env=prod
`kubectl get all -l env=prod`

# Show labels on all pods
`kubectl get pods --show-labels`



💻 Useful kubectl Commands
🎯 Query by Single Label

`kubectl get pods --selector app=app1`

Returns all Pods with app=app1.
🔍 Query by Multiple Labels

`kubectl get pods -l app=app1,function=web`

Returns Pods with both app=app1 and function=web.
🗂 Show Labels for All Pods

`kubectl get pods --show-labels`

Displays all Pods along with their assigned labels.
📌 Other Resources

You can use selectors with other objects too:

`kubectl get svc -l app=app1`
`kubectl get deployments -l function=api`
`kubectl delete pods -l app=app1`


## 🧾 What Is a ReplicaSet?

A **ReplicaSet** is a Kubernetes object that ensures a specified number of **identical Pods** are always running.

- If a Pod fails, the ReplicaSet **automatically creates a new one**.
- If too many Pods are running, it will **remove extras**.
- It uses **labels and selectors** to identify the Pods it manages.

---

## 📌 Key Concepts Recap

- ✅ The **ReplicaSet** creates Pods using a **Pod template**
- ✅ It uses a **`selector`** to match Pods with specific **labels**
- ✅ A **Service** can use the same labels to **route traffic** to those Pods
- ⚠️ Labels in the ReplicaSet’s Pod **template** must **match the selector**

---

## 🧱 Complete YAML Example: ReplicaSet + Service

```yaml
# ReplicaSet
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
  labels:
    component: backend-controller   # Labels for the ReplicaSet itself
spec:
  replicas: 3
  selector:                         # 🔗 Selector to find/manage Pods
    matchLabels:
      app: myapp
      function: api
  template:                         # 🧬 Pod template
    metadata:
      labels:                       # ✅ Must match the selector
        app: myapp
        function: api
    spec:
      containers:
        - name: my-container
          image: nginx
          ports:
            - containerPort: 80


 ```yaml
 # Service
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:                         # 🔗 Matches the same Pod labels
    app: myapp
    function: api
  ports:
    - port: 80                      # Port exposed by the Service
      targetPort: 80               # Port on the container
  type: ClusterIP
```

🔍 How It All Connects

[Service] ---> Selects Pods with app=myapp, function=api
   |
   ▼
[Pods] ---> Created and managed by ReplicaSet
   ▲
   |
[ReplicaSet] ---> Selects Pods with app=myapp, function=api

    The ReplicaSet keeps 3 Pods running with the right labels.

    The Service discovers those Pods and routes traffic to them.

✅ Best Practices

    Always make sure selector.matchLabels == template.metadata.labels

    Label both app and function (or similar) to organize and group logically

    Use the same labels for your Service selector

    Never assume top-level metadata.labels (on the ReplicaSet) apply to Pods — define them inside the template

💻 Useful Commands

# Create the resources
`kubectl apply -f replicaset.yaml`
`kubectl apply -f service.yaml`

# View Pods with matching labels
`kubectl get pods -l app=myapp,function=api`

# Describe ReplicaSet to inspect selector and status
`kubectl describe rs my-replicaset`

# Test connectivity (from another Pod in the cluster)
`kubectl run test --rm -it --image=busybox -- sh`
`wget -qO- http://myapp-service`


## 📝 Annotations in Kubernetes

**Annotations** in Kubernetes are key-value pairs used to attach **non-identifying metadata** to objects such as Pods, Services, Deployments, etc.

> 🧠 Unlike labels, annotations are **not used for filtering or selecting** objects. They are purely for storing additional information.


## 🧾 Example: Pod with Annotations

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: annotated-pod
  annotations:
    description: "This pod runs the frontend for App1"
    contact: "frontend-team@example.com"
    prometheus.io/scrape: "true"
    prometheus.io/port: "8080"
spec:
  containers:
    - name: nginx
      image: nginx
```      

