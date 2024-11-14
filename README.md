# Deploying a Wordpress application

This guide provides steps for deploying a Helm chart on a Kubernetes cluster using [K3s](https://k3s.io/) and [Helm](https://helm.sh/).

## Prerequisites

Ensure that the following tools are installed:

1. [K3s](https://k3s.io/)
2. [Helm](https://helm.sh/docs/intro/install/)
3. [kubectl](https://kubernetes.io/docs/tasks/tools/)
> **Note**: K3s includes `kubectl` by default, so installing `kubectl` separately is optional.


## Steps to Deploy a Helm Chart to K3s Locally

### Step 1: Install and Start K3s

To install and start K3s, use the following command:

```bash
curl -sfL https://get.k3s.io | sh -
```
This will install and start K3s as a single-node Kubernetes cluster.

After installation, verify the K3s cluster is running by checking the node and pods:

```bash
kubectl get nodes
```

### Step 2: Install the Helm Chart

1. Create a separate namespace:
    ```bash
    kubectl create namespace wordpress
    ```

2. Navigate to the chart's directory:

    ```bash
    cd wordpress
    ```

3. Install the chart on K3s with a chosen release name. Replace wordpress with a preferred name for this deployment:

    ```bash
    helm install wordpress . -n wordpress
    
    ```

### Step 4: Verify the Deployment
After installation, verify that the Helm chart has been deployed successfully by listing pods and services:

```bash
kubectl get pods
kubectl get svc
```
To check the status of the Helm release, use:

```bash
helm status wordpress
```

### Step 5: Access the Application
Port-forwarding can be used to access it locally, as K3s doesn’t automatically expose services outside the cluster. Replace <service-name> and <port> as appropriate:

```bash
kubectl port-forward service/<service-name> <port>:<target-port>
```

For example, to access a service running on port 80:

```bash
kubectl port-forward service/wordpress 8080:80
```
This command will allow access to the service at http://localhost:8080.

### Uninstall the Helm Release (Cleanup)
To delete the release and remove its associated Kubernetes resources:

```bash
helm uninstall wordpress
```

or

```bash
kubectl delete namespace wordpress
```
