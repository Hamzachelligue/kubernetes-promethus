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

