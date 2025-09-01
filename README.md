## Tools to create and run Kubernetes clusters.
- ### kind
```kind``` is a tool for running local Kubernetes clusters using Docker container “nodes”. kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.

- ### minikube
Like Kind, ```minikube``` is a tool that lets you run Kubernetes locally. ``minikube`` runs an all-in-one or a multi-node local Kubernetes cluster on your personal computer.

- ### kubeadm
You can use the ```kubeadm``` tool to create and manage Kubernetes clusters. It performs the actions necessary to get a minimum viable, secure cluster up and running in a user friendly way.

## Command-line tool used to interact with any Kubernetes cluster
- ### kubectl
The Kubernetes command-line tool, **kubectl**, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

# 1. Install Kind Cluster and KubeCTL
### Run the following script to install Kind Cluster and KubeCTL:

```
#!/bin/bash

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

VERSION="v1.30.0"
URL="https://dl.k8s.io/release/${VERSION}/bin/linux/amd64/kubectl"
INSTALL_DIR="/usr/local/bin"
curl -LO "$URL"
chmod +x kubectl
sudo mv kubectl $INSTALL_DIR/
kubectl version --client

rm -f kubectl
rm -rf kind

echo "kind & kubectl installation complete."
```

Kind lets you run Kubernetes on your local computer. This tool requires that you have Docker installed
# 2. Install Docker:

```
sudo apt-get update
sudo apt-get install docker.io
sudo usermod -aG docker $USER && newgrp docker
```

# 3. Setting Up the KIND Cluster

Create a kind-cluster-config.yaml file:
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
    - role: control-plane
      image: kindest/node:v1.33.1
    - role: worker
      image: kindest/node:v1.33.1
    - role: worker
      image: kindest/node:v1.33.1
      extraPortMappings:
        - containerPort: 80
          hostPort: 80
          protocol: TCP
        - containerPort: 443
          hostPort: 443
          protocol: TCP
```
This is a Kind cluster configuration file — used to define how a Kubernetes cluster should be created by the ```kind``` tool

Create the cluster using the configuration file:
```
kind create cluster --config=kind-cluster-config.yaml --name=prathameshCluster
```
Now lets see the architecture of ```Kubernetes``` cluster
![alt text](architecture.jpg)

## 1. Cluster
The entire structure in the image (Master + Nodes) is a Kubernetes Cluster.

A **cluster** consists of:

1. A Control Plane (Master Node) 🧠

2. One or more Worker Nodes 👷

## 2. Master Node (Control Plane)
This controls and manages the cluster. It contains:

1. **API Server**:
- The entry point for all commands (like from kubectl).
- Receives instructions and interacts with other components.
- Everything talks to the API Server.

2. **Scheduler**
- Decides on which worker node a new pod should be placed.
- Runs inside the master node.

3. **Controller Manager**
- Maintains the desired state of the cluster.
- Examples: deployment controller, replica controller, etc.

4. **etcd**
- A key-value store that holds all cluster data and state.
- Stores info like config, secrets, node data, etc.

## 3. Worker Nodes (Nodes)
These are the machines where your application actually runs.

Each node contains:

1. **Kubelet**
- An agent that runs on each worker node.
- Talks to the API server.
- Ensures the containers (Pods) are running as expected.

2. **Service Proxy (kube-proxy)**
- Handles networking and load balancing inside the cluster.
- Forwards traffic to the right pod/container.

3. **Pods (Orange blocks)**
- The smallest unit in Kubernetes that runs your app’s container(s).
- Each pod contains containers (like Docker containers).
- Pods are created by Deployments and run inside Worker Nodes.



Once the cluster is created using cluster configuration yaml file you can check different nodes running in that cluster using:
```
kubectl get nodes
```
**Namespaces** in Kubernetes are virtual clusters within a physical cluster that help organize and isolate resources like pods, services, and deployments.

To Create a new Namespace, use:

```
kubectl apply -f ns-config.yaml
```

Make sure that you have created the namespace configuration yaml file

Check different Namespaces:
```
kubectl get ns
```

A **Pod** is the smallest deployable unit in Kubernetes that encapsulates one or more containers, along with shared storage and networking.
A Pod is **created and runs inside a worker node** within the Kubernetes cluster.


To create a new Pod, use:
```
kubectl apply -f pod-config.yml
```

Make sure that you have created the pod configuration yaml file

Check different pods
```
kubectl get pods -n [namespace]
```
To Enter into the running pod's container
```
kubectl exec -it pod/[pod-name] -n [namespace] -- [cmd-to-be-run]
```
Ex.
```
kubectl exec -it pod/nginx-pod -n nginx -- sh
```
To get a detailed Description about the Pod:
```
kubectl describe pod/nginx-pod -n nginx
```

A **Deployment** in Kubernetes manages the desired state of Pods by automatically creating, updating, or deleting them as needed.
It runs on the **control plane**, which schedules the Pods to run on the **worker nodes**.

To create a new deployment use:
```
kubectl apply -f deployment-config.yml
```

Make sure that you have created the deployment configuration yaml file

Check different deployments
```
kubectl get deployments -n [namespace]
```
