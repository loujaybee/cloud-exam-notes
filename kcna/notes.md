
# Meta information (about the exam)
- [Exam Guide](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate/)
- [Kubernetes Docs](https://kubernetes.io/docs)

## Plan
- [ ] Buy the practice exams (Udemy)
- [ ] Initial pass of the Kubernetes Docs
- [ ] A run through of Kubernetes The Hard Way

## Content

### Kubernetes Fundamentals (46%)
**Topics:** Kubernetes Resources, Kubernetes Architecture, Kubernetes API, Containers, Scheduling

**Basics:** Service discovery and load balancing, self-healing, secrets management.

**What it is not:** CI/CD

### [Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/) (infra as code reference)

- Represent the state of your cluster. Your desired declarative end state. Most often provided via `kubectl` by passing a YAML file.

Three methods of interacting:
1. **Imperative** - User interacts directly on live objects. User provides operations to the `kubectl` command as arguments or flags.
2. **Imperative object** - Apply changes given in a single file, but still specifies which operation (create / read / update / delete etc).
3. **Declarative object configuration** - Does not define the operation, nor the specific file, operates on full directory structures.

#### Component: [Nodes](https://kubernetes.io/docs/concepts/overview/components/#node-components)

*Nodes are worker machines which host pods, where every cluster has at least 1 node.*

- Node names are unique
- Kubelet can self-register with the API server

Related node components:
- **[Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)** 
  - The agent running on a node, connects with apiserver. Makes sure that containers are running in a pod.
  - Takes PodSpec typically from `api-server` (but can be provided via a static file or a reference to an HTTP endpoint) to ensure that containers defined in the PodSpec are running and healthy.
- **[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)** 
  - Network proxy that runs on each node in your cluster.
- **[container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)** 
  - Software responsible for running containers (containerd, CRI-O).

#### Component: [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)

- Let's you query and manipulate the state of API objects in Kubernetes. Can be accessed through CLI commands such as `kubectl` and `kubeadm`, also has client libraries. 

- **[kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)** 
  - Exposes the Kubernetes API.

#### [Other Components](https://kubernetes.io/docs/concepts/overview/components/)

- **etcd** - Consistent, highly-available key/value store used as a backing store for cluster data. 
- **kube-scheduler** - Watches for newly created pods and selects nodes for them to run on. 
- **kube-controller-manager** - Runs controller processes. Each controller is a separate process, but are compiled into a single binary.
- **cloud-controller-manager** - Cloud specific control logic. When ran on-premise or on your computer you do not have this component. Executes as a single binary.
- **Addons** - Such as cluster DNS, Web UI, container resource monitoring, cluster-level logging

### Container Orchestration (22%)
**Topics:** Container Orchestration Fundamentals, Runtime, Security, Networking, Service Mesh, Storage

### Networking
- Each pod gets it's own IP
- Kubernetes IP addresses exist at the `Pod` scope
- Containers within a `Pod` can all reach each other's ports on localhost
- Containers within a Pod must coordinate port usage, but this is no different from processes in a VM. 

### Cloud Native Architecture (16%)
**Topics:** Autoscaling, Serverless, Community and Governance, Roles and Personas, Open Standards

- [Cloud Native Landscape](https://landscape.cncf.io/)

### Cloud Native Observability (8%)
**Topics:** Telemetry & Observability, Prometheus, Cost Management

### Cloud Native Application Delivery (8%)
**Topics:** Application Delivery Fundamentals, GitOps, CI/CD

### Questions

- Are things like canary deployments handled with plugins?
- Would you deploy databases from within Kubernetes?
- GRPC vs HTTP