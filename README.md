# Kubernets

Kubernetes is an open-source container orchestration platform developed by Google. It helps in automating the deployment, scaling, and management of containerized applications. Containers are a lightweight, portable, and scalable way to package and run applications, and Kubernetes provides the infrastructure needed to deploy and manage these containers at scale.

<b>Here's a breakdown of what Kubernetes does:</b>

1. Automated deployment: Kubernetes allows you to define how your applications should be deployed using declarative configuration files. It then automates the deployment process, ensuring that your applications are running as specified.
2. Scaling: Kubernetes makes it easy to scale your applications up or down based on demand. You can define scaling rules and policies, and Kubernetes will automatically adjust the number of containers running your application to handle changes in traffic or workload.
3. Management: Kubernetes provides a range of features for managing your applications, including service discovery, load balancing, and storage orchestration. It also includes built-in health checking and self-healing capabilities, so it can automatically restart containers that fail or become unresponsive.
4. Portability: Kubernetes runs on any infrastructure, whether it's on-premises, in the cloud, or a hybrid of both. This makes it easy to move your applications between different environments without having to make significant changes to your deployment configuration.

# Kubernetes Components

Kubernetes is composed of several key components, each serving a specific role in the orchestration and management of containerized applications. 

<b>Here's an overview of the main components:</b>

# Master Node:
1. API Server: Acts as the front-end for Kubernetes and is responsible for serving the Kubernetes API. It validates and processes requests from users and other components.
2. Scheduler: Assigns nodes for newly created pods based on resource availability and constraints.
3. Controller Manager: Manages various controllers that regulate the state of the cluster, such as ReplicaSets, Deployments, StatefulSets, etc.
4. etcd: Consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.
# Worker Node:
1. Kubelet: An agent that runs on each node and is responsible for ensuring that containers are running in a pod.
2. Kube-proxy: Maintains network rules on nodes and performs connection forwarding.
3. Container Runtime: Software responsible for running containers, such as Docker, containerd, or CRI-O.
# Networking:
1. Pod Networking: Kubernetes allows pods to communicate with each other across nodes. Various networking solutions like Calico, Flannel, Weave, etc., facilitate this communication.
Service Networking: Kubernetes services provide a stable IP address and DNS name for a set of pods, enabling other applications to access them reliably.
# Add-Ons:
1. DNS: Provides DNS-based service discovery for Kubernetes services.
2. Dashboard: A web-based UI for managing and monitoring Kubernetes clusters.
3. Ingress Controller: Manages external access to services in a Kubernetes cluster, typically by managing external load balancers, SSL/TLS termination, and routing.
# Persistent Volumes:
Kubernetes supports various storage backends and provides PersistentVolume and PersistentVolumeClaim resources for managing persistent storage.
# Security:
Kubernetes supports various security features like Role-Based Access Control (RBAC), Network Policies, Pod Security Policies, and secrets management to secure cluster resources and communication.