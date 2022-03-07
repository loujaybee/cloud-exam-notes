
# Meta information (about the exam)
- [Exam Guide](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate/)
- [Kubernetes Docs](https://kubernetes.io/docs)

# Plan
- [ ] Buy the practice exams (Udemy)
- [ ] Initial pass of the Kubernetes Docs
- [ ] A run through of Kubernetes The Hard Way

## Overview

- [ ] Kubernetes Fundamentals
  - [ ] Kubernetes Resources
  - [ ] Kubernetes Architecture
  - [ ] Kubernetes API
  - [ ] Containers
  - [ ] Scheduling
- [ ] Container Orchestration
  - [ ] Container Orchestration Fundamentals
  - [ ] Runtime
  - [ ] Security
  - [ ] Networking
  - [ ] Service Mesh
  - [ ] Storage
- [ ] Cloud Native Architecture
  - [ ] Autoscaling
  - [ ] Serverless
  - [ ] Community and Governance
  - [ ] Roles and Personas
  - [ ] Open Standards
- [ ] Cloud Native Observability
  - [ ] Telemetry & Observability
  - [ ] Prometheus
  - [ ] Cost Management
- [ ] Cloud Native Application Delivery
  - [ ] Application Delivery Fundamentals
  - [x] GitOps
  - [x] CI/CD

# Kubernetes Fundamentals (46%)
**Topics:** Kubernetes Resources, Kubernetes Architecture, Kubernetes API, Containers, Scheduling

**Basics:** Service discovery and load balancing, self-healing, secrets management.

## Containers

- Decouple applications from underlying host infrastructure

## [Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/) (infra as code reference)

- Represent the state of your cluster. Your desired declarative end state. Most often provided via `kubectl` by passing a YAML file.

### Methods of interacting with Kubernetes objects
1. **Imperative** - User interacts directly on live objects. User provides operations to the `kubectl` command as arguments or flags.
2. **Imperative object** - Apply changes given in a single file, but still specifies which operation (create / read / update / delete etc).
3. **Declarative object configuration** - Does not define the operation, nor the specific file, operates on full directory structures.

## Component: [Nodes](https://kubernetes.io/docs/concepts/overview/components/#node-components)

*Nodes are worker machines which host pods, where every cluster has at least 1 node.*

- Node names are unique
- Kubelet can self-register with the API server

### Node authorisation

https://kubernetes.io/docs/reference/access-authn-authz/node/

### Related components
- **[Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)** 
  - The agent running on a node, connects with apiserver. Makes sure that containers are running in a pod.
  - Takes PodSpec typically from `api-server` (but can be provided via a static file or a reference to an HTTP endpoint) to ensure that containers defined in the PodSpec are running and healthy.
- **[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)** 
  - Network proxy that runs on each node in your cluster.
- **[container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)** 
  - Software responsible for running containers (containerd, CRI-O).
  - systemd generates and consumes a root control group and acts as a cgroup manager
  - There is a [cgroup v1 and cgroup v2](https://medium.com/some-tldrs/tldr-understanding-the-new-control-groups-api-by-rami-rosen-980df476f633#:~:text=In%20cgroups%20v1%2C%20a%20process,only%20to%20a%20single%20subgroup.)


## Component: [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

- Smallest deployable units of compute that you can deploy in Kubernetes
- Shared storage and network resources (co-located, co-scheduled, run in shared context)
- Can include [init](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) - These run before app containers in a pod.
- And [ephemeral containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) - Used for inspecting running applicataions, rather than for running applications. They lack guarentees for completion, and are not automatically restarted. Useful when you can't exec into a container.
- "User accounts" are for humans, "service accounts" are for processes. User accounts are global, service accounts are namespaced. Pods created use the `default` "service account".

## Component: [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)

- Let's you query and manipulate the state of API objects in Kubernetes. Can be accessed through CLI commands such as `kubectl` and `kubeadm`, also has client libraries. 

- **[kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)** 
  - Exposes the Kubernetes API.

- **[kube-ctl](https://kubernetes.io/docs/reference/kubectl/)** 
  - `kubectl config set-cluster` - 


## [etcd](https://etcd.io/) 

- [How etcd works with and without kubernetes](https://learnk8s.io/etcd-kubernetes)
- Strongly consistent (must be strongly consistent, not eventual), highly-available (designed to be ran on many nodes, unlike SQL) key/value store used as a backing store for cluster data. Can be encrypted at rest. 

## [Kube Scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/)

Watches for newly created pods and selects nodes for them to run on. Assigns pods to nodes

## [Kube Controller Manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/) and [Cloud Controller Manager](https://kubernetes.io/docs/concepts/architecture/cloud-controller/)

**Kube controller manager** - Runs controller processes. Each controller is a separate process, but are compiled into a single binary.

**cloud-controller-manager** - Cloud specific control logic. When ran on-premise or on your computer you do not have this component. Executes as a single binary.

## [Other Components](https://kubernetes.io/docs/concepts/overview/components/)
- **Addons** - Such as cluster DNS, Web UI, container resource monitoring, cluster-level logging

# Container Orchestration (22%)
**Topics:** Container Orchestration Fundamentals, Runtime, Security, Networking, Service Mesh, Storage



# Networking

## Overview
- Each pod gets it's own IP
- Kubernetes IP addresses exist at the `Pod` scope
- Containers within a `Pod` can all reach each other's ports on localhost
- Containers within a Pod must coordinate port usage, but this is no different from processes in a VM. 

# Cloud Native Architecture (16%)
**Topics:** Autoscaling, Serverless, Community and Governance, Roles and Personas, Open Standards

- [Cloud Native Landscape](https://landscape.cncf.io/)

# Cloud Native Observability (8%)
**Topics:** Telemetry & Observability, Prometheus, Cost Management

# Cloud Native Application Delivery (8%)
**Topics:** Application Delivery Fundamentals, GitOps, CI/CD

**GitOps**
* CI/CD + IaC + Code Review
* Push/Pull pipeline - push pipeline defines infra and has access to the environment, pull based syncs the source control with the cluster architecture
* Pull based GitOps (the cluster has access to source control but doesn't expose credentials outside of the system)

**OCI**
* The `runtime-spec` and the `image-spec`

## Questions

- Are things like canary deployments handled with plugins?
- Would you deploy databases from within Kubernetes?
- GRPC vs HTTP