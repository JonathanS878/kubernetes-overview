# Kubernetes Objects README

## Overview
This README provides an overview of Kubernetes objects, which are the fundamental building blocks in Kubernetes. Kubernetes objects represent various resources and functionalities within the cluster, such as managing containers, networking, storage, and more.

## List of Kubernetes Objects

### 1. Pods
Pods are the smallest and simplest Kubernetes objects. They consist of one or more containers that are scheduled and run together on a single node. Pods are often used to group tightly coupled containers that need to share resources.

**Example Pod:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: web
    image: nginx
  - name: db
    image: postgres
```

### 2. Services
Services provide a way to expose Pods to the network and enable communication between them. They can load balance traffic across multiple Pods or provide a single endpoint for a group of Pods.

**Example Service:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

### 3. Replication Controllers
Replication Controllers ensure that a specified number of Pods are running at all times. They are useful for creating and managing stateless Pods or those without a consistent identity.

**Example Replication Controller:**
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-replica-controller
spec:
  replicas: 3
  selector:
    app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: web
        image: nginx
```

### 4. ReplicaSets
ReplicaSets are a more flexible and advanced version of Replication Controllers. They allow you to create and manage Pods that have a consistent identity or require a specific version of a container image.

**Example ReplicaSet:**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replica-set
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: web
        image: nginx
        imagePullPolicy: Always
```

### 5. Deployments
Deployments provide a higher-level abstraction for managing the rollout of changes to Pods. They are ideal for updating and scaling applications while ensuring high availability.

**Example Deployment:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: web
        image: nginx
        imagePullPolicy: Always
```

### 6. DaemonSets
DaemonSets guarantee that a particular Pod is running on every node in the Kubernetes cluster. They are typically used for deploying system-level daemons like monitoring or logging agents.

**Example DaemonSet:**
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        imagePullPolicy: Always
```

### 7. Jobs
Jobs are used to run Pods that are terminated after completing their tasks. They are suitable for running batch jobs or one-off tasks.

**Example Job:**
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        imagePullPolicy: Always
      restartPolicy: Never
```

### 8. CronJobs
CronJobs are a type of Job that is scheduled to run on a recurring basis, similar to a cron job in UNIX-like systems. They are useful for automating repetitive tasks, such as backups or software updates.

**Example CronJob:**
```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "0 0 * * *"
  jobTemplate:
    spec:
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - name: my-container
            image: nginx
            imagePullPolicy: Always
          restartPolicy: Never
```

### 9. ConfigMaps
ConfigMaps store configuration data as key-value pairs in Kubernetes. They are helpful for managing environment variables, startup scripts, or any other configuration data.

**Example ConfigMap:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  database: |
    host: localhost
    port: 5432
    user: root
    password: secret
```

### 10. Secrets
Secrets securely store sensitive data, such as passwords or API keys, in Kubernetes. They ensure that sensitive information is encrypted and only accessible to authorized entities.

**Example Secret:**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  password: bXktc2VjcmV0LXBhc3N3b3Jk  # Base64 encoded secret
```

### 11. Volumes
Volumes enable Pods to store data persistently. They can store files, directories, or other types of data required by the application running in the Pod.

**Example Volume:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    volumeMounts:
    - name: my-volume
      mountPath: /data
  volumes:
  - name: my-volume
    hostPath:
      path: /path/to/my/data
```

### 12. PersistentVolumes
PersistentVolumes (PVs) provide persistent storage in Kubernetes. They are commonly used to store data that needs to be preserved even if a Pod is terminated or rescheduled.

**Example PersistentVolume:**
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-storage-class
```

Certainly! Here's the updated section with just the information for PersistentVolumeClaims:

### 13. PersistentVolumeClaims
PersistentVolumeClaims (PVCs) are used to request a PersistentVolume from the Kubernetes cluster. PVCs allow users to dynamically claim storage resources for their Pods.

**Example PersistentVolumeClaim:**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: my-storage-class
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

PersistentVolumeClaims are essential for dynamically provisioning and using storage resources in Kubernetes. They define the desired storage characteristics and allow Kubernetes to bind the appropriate PersistentVolume that matches the requested criteria.