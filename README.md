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
