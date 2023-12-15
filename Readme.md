# Install Prometheus Monitoring on Kubernetes
Prometheus monitoring can be installed on a Kubernetes cluster by using a set of YAML (Yet Another Markup Language) files. These files contain configurations, permissions, and services that allow Prometheus to access resources and pull information by scraping the elements of your cluster.

YAML files are easily tracked, edited, and can be reused indefinitely.
## Step 1: Create Monitoring Namespace
All the resources in Kubernetes are started in a namespace. Unless one is specified, the system uses the default namespace. To have better control over the cluster monitoring process, specify a monitoring namespace.

The name of the namespace needs to be a label compatible with DNS. For easy reference, we are going to name the namespace: monitoring.

There are two ways to create a monitoring namespace for retrieving metrics from the Kubernetes API.

### Option 1:

Enter this simple command in your command-line interface and create the monitoring namespace on your host:

```
kubectl create namespace monitoring
```
### Option 2:
```
apiVersion: v1
kind: Namespace
metadata:
  name: monitoring
```
This method is convenient as you can deploy the same file in future instances.

2. Apply the file to your cluster by entering the following command in your command terminal:
```
kubectl -f apply namespace monitoring.yml
```
3. List existing namespaces by using this command:
```
kubectl get ns
```
## Step 2: Create Persistent Volume and Persistent Volume Claim
A Prometheus deployment needs dedicated storage space to store scraping data.
```
vi pv.yaml
```
1. Paste the following configuration into the file.
```
piVersion: v1
kind: PersistentVolume
metadata:
  name: pv-1
  labels:
    type: local
spec:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/mnt/data"
```
2. to create pvc add new file pv-pvc and paste the following configuration 
```
vi pv-pvc.yaml
```
```
apiVersion: v1

kind: PersistentVolumeClaim

metadata:

  name: pvc-data

  namespace: monitoring

  labels:

    app: prometheus-deployment

spec:

  accessModes:

    - ReadWriteMany

  resources:

    requests:

      storage: 500Mi
```
3.  Apply the configuration with kubectl.
```
kubectl apply -f pv-pvc.yaml
```
## Step 3: Create Cluster Role, Service Account and Cluster Role Binding
Namespaces are designed to limit the permissions of default roles. Hence, if we want to retrieve cluster-wide data, we need to give Prometheus access to all cluster resources.

The steps below explain how to create and apply a basic set of .yaml files that provide Prometheus with cluster-wide access.

1. Apply the file.
```
kubectl apply -f service-account.yaml
```
2. apply the file cluster role:
```
kubectl apply -f clusterRole.yaml
```

## Step 4: Create Prometheus ConfigMap
This section of the file provides instructions for the scraping process. Specific instructions for each element of the Kubernetes cluster should be customized to match individual monitoring requirements and cluster setup.
The example in this article uses a simple ConfigMap that defines the scrape and evaluation intervals, jobs, and targets.

1. Apply the ConfigMap with kubectl.
```
kubectl apply -f config-map.yaml
```
## Step 5: Deploy prometheus
Create a deployment on monitoring namespace using the above file.
```
kubectl apply -f prometheus-deployment.yaml
```
## Step 6: expose your deployment using service.
```
kubectl apply -f prometheus-deployment.yaml
```
## Step 7: Expose your promethus on public using ingress
You can view the deployed Prometheus dashboard using ingress
```
kubectl apply -f prometheus-service.yaml
```
