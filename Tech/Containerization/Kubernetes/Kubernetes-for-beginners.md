# Links
Beginner Concepts and Examples [Beginner Examples](../Kubernetes)

# Course Options
1. Kubernetes for Beginners
2. Kubernetes for Admistrators -> Certified Adminstrators exam
3. Kubernetes for (Application) Developers -> Certified Developer exam
   
# Container Orchestration
Multiple orchestration technology options:
* Docker Swarm: simple to use but fewer features
* Mesos: from Apache, fully featured but complex setup
* Kubernetes or K8s: from Google

**What is Kubernetes?**: It is a container orchestration technology used to deploy containers in a distributed + clustered environment.

# Kubernetes Architecture
## **Components**
* Nodes (or minions): Physical nodes 
* Cluster: A set of nodes deployed together
* Master: Another node in the cluster that manages other nodes and orchestrate containers on nodes

## Parts of kubernetes deployment
* API server: Gateway for user interaction
* Scheduler: schedules containers on nodes
* Controller: decides to bring new containers and nodes up/down
* Container runtime: underlying runtime to run the containers (eg. Docker)
* Kubelet (service): Agent that runs on all nodes, make sure containers are running.
* etcd (service) - distributed key-value pair DB
* cubectl (cube-ctl / cube command line tool)

Container runtime options:
* Docker
* RKT (rocket)
* CRI-O (cryo)

Worker nodes run: Kubelets + Container runtime
Master nodes run: Apiserver + etcd + controller + scheduler

### CRI
- container runtime interface
- Kubernetes was designed to only work with Docker. CRI was introduced to add support for other container runtimes eg. Rkt

### OCI
- Open container initiative
- Contains
  - Imagespec : how the container image is built
  - runtimespec: how to build the container runtime
- dockershim - a shim layer built to support docker runtime in kubernetes without the use of CRI

### containerd
- dockers container runtime env
- is seperate project and part of CNCF (Cloud Native Computing Foundation)
- ctr - CLI for debugging containerd
- nerdctl (nerd control) - docker-like CLI for managing containerd
- crictl (cry control) - CLI used to manage the CRI, mostly for debugging.


| Tool -> | CTR | nerdctl | crictl |
| ------- | --- | ------- | -------|
| Purpose | Debugging | General Purpose | Debugging |
| Communnity | containerd | containerd | Kubernetes |
| Works with | containerd| containerd | All CRI compatible runtimes |

# Kubernetes Concepts
## Pods
What is a pod?
- A pod is an instance of an application (on a node)
- A pod is the smallest object managed by kubernetes
- A pod is usually a 1 container 
  - a pod <-> container relation is usually 1-to-1
- A pod COULD contain more than 1 container but they are usually not the same kind
- containers within the pod share the same network space (talk via localhost) and share the same storage as well

## Replication Controller
- Replication controller helps load balance/scale pods across multiple paths on different nodes (or the same node)
### Replication Controller vs Replica Set
- Replica Sets are the newer preffered way of setting up replication as opposed to the Replication Controller

## Deployments
- Higher object that contains replica-sets, pods and allows capability to do seamless rolling updates, undo, pause/resume changes.
- Each deployment starts as a rollout with a revision=1
- Depolyment stategies:
  - Recreate: destroy all old pods and create new ones
  - Rolling: Take down pods one-by-one and bring up new ones in its place. This is the default update strategy. Under the hood a new Replica-set is created and new pods are created in the new replica-set.

Hierarchy:
Deployments => Replica-Set => Pods => containers <-> Nodes

# Networking in Kubernetes
- All pods on single node are attached to the 10.244.0.0 network and get private IP addresses from the 10.244.0.x address space.
- It is not advised to rely on the internal IP addresses for pod-2-pod communication:
  - The Pod IP addresses can change during updates and rollbacks
  - If the Pods are on different nodes, the local IP would not work.
- It is the users job to configure the networking for the Pods in the cluster
  - All containers/Pods should be able to communicate without a NAT
  - All nodes and containers can communicate with each other without a NAT
- Ready made solutions for setting up networking include:
  - Flannel
  - VMware NSX
  - Cilium
  - Cisco ACI
  - Calico
  
# Kubernetes Services
- Services within Kubernetes that allow communication with the cluster
  - NodePort: Exposes a port that is externally visible to the internal network (for the pods to listen)
  - ClusterIP: Creates a virtual IP within the cluster to enable communication between services within the cluster
  - LoadBalancer: Creates a load balancer to distribute load between servers within the group

## NodePort
- service type: NodePort
- spec:
  - targetPort: listening port of the pods
  - port: internal port of the service
  - nodePort: external port where the service listens

## ClusterIP
- service type: ClusterIp
- spec:
  - targetPort: Port of the backend of the service
  - port: port where the service listens

## LoadBalancer
- service type: LoadBalancer
- Only works on supported cloud platforms eg. GCP, Azure, AWS etc. Does not work on virtualBox etc.
- On unsupported platforms this fallbacks to NodePort service.

# Kubernetes deployment
- Self hosted / turnkey: VMs and clusters managed manually e.g. kops or KubeOne
- Hosted / Managed: kubernetes-as-a-service, VMs and clusters managed by providers e.g.
  - Google Kubernetes Engine - GKE
  - Azure Kubernetes Engine - AKS
  - Amazon Elastic Kubernetes Service - EKS
