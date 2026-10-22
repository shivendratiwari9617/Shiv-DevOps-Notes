# Kubernetes Platform Coverage Checklist (10/10)

## 1. Kubernetes foundations
- What Kubernetes is and the problem domains it addresses
- Containerization fundamentals
- Linux namespaces and cgroups basics
- Images, registries, and immutability
- Container runtimes and runtime standards
- Declarative configuration and desired state
- Control loops and reconciliation
- API-first infrastructure management
- Idempotency and eventual consistency
- Cluster, node, pod, and workload terminology
- Declarative vs imperative operations
- Kubernetes architecture at a high level
- Resource manifests and YAML structure
- Metadata, spec, and status model
- Labels, selectors, annotations, and namespaces
- API groups, versions, and kinds
- CRUD, watch, and patch semantics
- Object lifecycle and garbage collection
- Events and state transitions

## 2. Kubernetes architecture
- Full control plane and data plane architecture
- Single-cluster architecture
- Multi-cluster architecture
- Private cluster architecture
- Public endpoint vs private endpoint design
- Managed, self-managed, hybrid, and edge deployment models
- Control plane topology and quorum design
- Worker node topology
- Communication paths between components
- Failure domains and blast radius analysis
- Zonal, regional, and multi-region architecture
- Hub-and-spoke fleet patterns
- Shared services cluster models
- Workload cluster models
- Separation of infrastructure, platform, and application concerns
- Platform boundaries and ownership domains
- Scalability bottlenecks and architectural limits
- API-driven extensibility model

## 3. Control plane internals
### kube-apiserver
- Request processing pipeline
- Authentication
- Authorization
- Admission chain
- Validation, defaulting, and conversion
- API aggregation
- OpenAPI schema publishing
- Watch semantics and resourceVersion handling
- API priority and fairness
- Rate limits and throttling
- Audit logging
- HA fronting and endpoint protection
- Secure exposure and network hardening

### etcd
- Raft consensus model
- Quorum and leader election
- Read/write consistency behavior
- Membership management
- Compaction and defragmentation
- Backup and snapshot strategies
- Restore workflows and cluster recovery
- Disaster scenarios and quorum loss handling
- Encryption and TLS
- Performance sensitivity to latency and storage

### kube-scheduler
- Queueing and scheduling lifecycle
- Filter and score phases
- Scheduling framework plugins
- Resource fit logic
- Node selectors
- Node affinity
- Pod affinity and anti-affinity
- Taints and tolerations
- Topology spread constraints
- Priority and preemption
- NUMA awareness concepts
- Specialized hardware placement
- Custom scheduler profiles

### kube-controller-manager
- Reconciliation internals
- Deployment controller
- ReplicaSet controller
- StatefulSet controller
- DaemonSet controller
- Job and CronJob controller
- Namespace controller
- Node controller
- Garbage collector
- ServiceAccount controller
- PV/PVC related control loops
- Endpoint and EndpointSlice controllers

### cloud-controller-manager
- Cloud-provider abstraction
- Node initialization
- Load balancer provisioning
- Route management
- Volume lifecycle hooks
- Cloud failure modes and retry behavior

## 4. Node architecture and runtime
- kubelet internals
- kube-proxy internals
- Container runtime interaction through CRI
- OCI image and runtime standards
- Pod sandbox creation
- CNI invocation from the node
- CSI invocation from the node
- Static pods
- Node registration and certificates
- Node health reporting and heartbeats
- Graceful node shutdown
- Node problem detection concepts
- Device plugins
- GPU support
- HugePages
- CPU Manager and Topology Manager concepts
- RuntimeClass usage
- Sandbox runtimes
- OS hardening
- Minimal node OS patterns
- Image pre-pulling strategies
- Secure bootstrap and node join workflows

## 5. Kubernetes API and resource model
- Namespaced vs cluster-scoped resources
- Metadata fields and common patterns
- Spec and status separation
- Object immutability considerations
- Finalizers
- OwnerReferences
- Garbage collection
- Field management and server-side apply
- Strategic merge, JSON merge, and JSON patch
- ResourceVersion and optimistic concurrency
- Events and auditability
- Discovery APIs
- CRDs and schema design
- Conversion webhooks
- Defaulting webhooks
- Validation webhooks
- Aggregated APIs
- API versioning and deprecation planning

## 6. Core workload abstractions
- Pods and pod lifecycle phases
- Init containers
- Sidecars
- Ephemeral containers
- Multi-container pod patterns
- ReplicaSets
- Deployments
- StatefulSets
- DaemonSets
- Jobs
- Indexed Jobs
- CronJobs
- Restart policies
- Pod lifecycle hooks
- Graceful shutdown
- Termination grace periods
- PodDisruptionBudgets
- Revision history and rollout status
- Bare pods and operational risks
- Controller selection trade-offs

## 7. Configuration and dependency injection
- ConfigMaps
- Secrets
- Downward API
- Projected volumes
- Environment variables vs mounted configuration
- Config reload patterns
- Immutable ConfigMaps and Secrets
- External secret injection patterns
- Template rendering at deploy time
- Runtime configuration drift risks
- Environment-specific configuration strategies

## 8. Service discovery and networking fundamentals
### Networking model
- Flat pod network model
- Pod IP uniqueness
- Pod-to-pod routing
- Node-to-pod routing
- Pod-to-service access
- East-west traffic
- North-south traffic
- DNS-based discovery
- CoreDNS architecture
- Search domains and name resolution behavior
- Endpoint and EndpointSlice management

### Service model
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName
- Headless Services
- Session affinity
- Traffic policies
- Topology-aware hints
- Internal vs external load balancing
- kube-proxy modes
- Proxy replacement models

### Ingress and edge traffic
- Ingress resources
- IngressClass
- Ingress controller models
- TLS termination
- Re-encryption
- Host and path routing
- Header and redirect behaviors
- External DNS integration concepts
- WAF and edge security integration

### Gateway API
- GatewayClass
- Gateway
- HTTPRoute
- GRPCRoute
- TCPRoute
- TLSRoute
- UDPRoute
- Route attachment models
- Shared gateway models
- Cross-namespace references
- Platform-team vs app-team ownership boundaries
- Gateway API migration planning

### Network policy
- Default deny models
- Ingress restrictions
- Egress restrictions
- Namespace selectors
- Pod selectors
- IPBlock usage
- Multi-tenant segmentation
- Egress control patterns
- Limitations across CNIs

### CNI ecosystem
- CNI plugin architecture
- Overlay networking
- Routed networking
- BGP concepts
- VXLAN concepts
- eBPF data planes
- Dual-stack networking
- IPv4 and IPv6 planning
- IPAM concerns
- MTU tuning awareness
- Network observability
- Network performance trade-offs

## 9. Advanced traffic management and service mesh
- Service mesh purpose and value
- Data plane and control plane separation
- Sidecar architecture
- Ambient or sidecarless models
- mTLS between services
- Identity issuance and rotation
- Traffic splitting
- Progressive traffic shifting
- Retries
- Timeouts
- Circuit breakers
- Outlier detection
- Fault injection
- Egress control
- Authorization policies
- Mesh observability
- Multi-cluster mesh models
- Cross-network mesh
- Ingress and egress gateways
- Operational and latency trade-offs

## 10. Storage architecture
### Volume fundamentals
- Ephemeral storage model
- Persistent storage model
- Volume lifecycle
- Access modes
- Mount semantics
- Raw block support
- Filesystem support
- ReadWriteOncePod awareness

### Volume types
- emptyDir
- hostPath
- configMap volume
- secret volume
- projected volume
- downwardAPI volume
- persistentVolumeClaim volume
- CSI ephemeral volumes

### Persistent storage resources
- PersistentVolume
- PersistentVolumeClaim
- StorageClass
- Dynamic provisioning
- Static provisioning
- Delayed binding
- Reclaim policies
- Expansion and resizing
- Storage topology
- Snapshot classes
- Volume snapshots
- Cloning

### Stateful platform concerns
- StatefulSets and stable identities
- Databases on Kubernetes
- Queue and stream systems on Kubernetes
- ReadWriteMany trade-offs
- Performance classes
- Local storage trade-offs
- Data locality
- Backup integration
- Recovery validation
- Storage security and encryption

## 11. Security architecture
### Identity and access
- Authentication methods
- Client certificates
- Static tokens and why to avoid them
- OIDC integration
- External IdP federation
- Authorization modes
- RBAC modeling
- Roles and ClusterRoles
- RoleBindings and ClusterRoleBindings
- ServiceAccounts
- Bound service account tokens
- Least privilege design
- Break-glass access controls
- Workload identity
- Cloud IAM integration
- SPIFFE and SPIRE concepts

### Pod and node security
- Pod Security Standards
- Pod Security Admission
- SecurityContext
- PodSecurityContext
- Non-root execution
- UID/GID strategy
- Linux capabilities
- Seccomp
- AppArmor
- SELinux
- Read-only root filesystem
- Privileged containers
- Host network/PID/IPC risk
- RuntimeClass isolation
- Sandboxed runtimes
- Node hardening
- SSH minimization and host access policy

### Secrets and key management
- Secret handling anti-patterns
- Encryption at rest
- KMS integration
- External secrets managers
- Certificate authorities and issuance
- cert-manager
- Secret rotation and expiration policy
- Short-lived credentials

### Admission, policy, and preventive controls
- Native admission controllers
- Mutating admission
- Validating admission
- Policy as code
- OPA Gatekeeper
- Kyverno
- Image policy enforcement
- Tag immutability policies
- Registry allowlists
- Namespace guardrails
- Preventive vs detective controls

### Supply chain security
- Trusted registries
- Signed images
- Provenance
- SBOM generation and verification
- Dependency scanning
- Base image governance
- Build pipeline isolation
- Artifact promotion controls
- SLSA awareness
- Software supply chain incident response

### Detection, audit, and response
- Audit policy design
- Audit log routing and retention
- Runtime threat detection
- Behavioral anomaly detection
- Forensics readiness
- Incident containment
- Compromised credential rotation
- Workload quarantine patterns

## 12. Scheduling, isolation, and resource governance
- CPU, memory, and ephemeral storage requests
- CPU, memory, and ephemeral storage limits
- QoS classes
- OOMKilled analysis
- Eviction policies
- Node pressure handling
- Taints and tolerations
- Affinity and anti-affinity
- Topology spread constraints
- Priority classes
- Preemption
- Extended resources
- Overcommit strategy
- Noisy neighbor mitigation
- Namespace quotas
- LimitRanges
- Fairness and multi-tenant resource controls
- Performance-sensitive workload placement

## 13. Autoscaling and elasticity
- Manual scaling
- Horizontal Pod Autoscaler
- Vertical Pod Autoscaler
- Cluster Autoscaler
- Node pool autoscaling
- Event-driven autoscaling
- KEDA concepts
- Scale-to-zero
- Custom and external metrics
- Safe autoscaling windows
- Stateful workload autoscaling cautions
- Cost-performance balancing
- Buffer capacity strategies

## 14. Reliability engineering and resilience
- Probes: liveness, readiness, startup
- Graceful shutdown
- Pod disruption budgets
- Replica distribution across zones
- Anti-affinity for availability
- Control plane HA
- etcd quorum protection
- Retry and timeout patterns
- Circuit breaking patterns
- Backpressure and load shedding
- Self-healing expectations vs real limitations
- Failure injection testing
- Chaos engineering
- Disaster drills
- SLOs, SLIs, and error budgets
- Toil reduction practices
- Runbook quality and validation

## 15. Deployment and release engineering
- Recreate strategy
- Rolling update strategy
- Blue-green deployments
- Canary deployments
- Progressive delivery
- A/B testing concepts
- Shadow traffic patterns
- Feature flags
- Safe rollback design
- Revision history
- MaxSurge and MaxUnavailable tuning
- Rollout pause and resume
- Argo Rollouts concepts
- Flagger concepts
- Deployment freeze policies
- Database migration coordination
- Contract compatibility between versions
- Safe schema evolution

## 16. Packaging and environment management
- YAML organization patterns
- Kustomize bases and overlays
- Helm chart structure
- Helm values strategy
- Helm dependency management
- Template anti-patterns
- Jsonnet ecosystem awareness
- Environment overlays
- Secret overlays
- Release versioning
- Drift control
- Promotion between environments
- Golden path templates
- Reusable platform blueprints

## 17. CI/CD and GitOps
- Build pipelines for images
- Registry scanning gates
- Manifest linting and validation
- Policy checks in CI
- Integration tests and smoke tests
- Progressive delivery pipelines
- GitOps pull model
- Reconciliation loop in delivery
- Drift detection
- Sync ordering and dependency waves
- Secrets in GitOps
- Promotion strategies
- Multi-repo vs mono-repo trade-offs
- Change approval and auditability
- Argo CD concepts
- Flux concepts
- Rollback automation and release recovery

## 18. Observability platform
### Logging
- Stdout/stderr conventions
- Structured logs
- Correlation IDs
- Central collection pipelines
- Retention and archival
- Compliance retention considerations

### Metrics
- Metrics Server
- Prometheus ecosystem
- Scrape design
- ServiceMonitor and PodMonitor concepts
- Node exporter concepts
- kube-state-metrics concepts
- Alertmanager concepts
- RED and USE models
- Saturation and capacity monitoring

### Tracing
- Distributed tracing
- OpenTelemetry concepts
- Trace context propagation
- Linking traces, logs, and metrics
- Sampling strategies
- Troubleshooting distributed latency

### Debugging and diagnostics
- kubectl debug workflows
- Ephemeral containers
- Event analysis
- Audit trail analysis
- Network debugging
- DNS debugging
- Packet capture concepts
- Performance profiling awareness

### Reporting and ops visibility
- Dashboards by audience
- SLO dashboards
- Capacity dashboards
- Cost dashboards
- Executive reporting vs engineering reporting

## 19. Cluster administration and lifecycle management
- Managed vs self-managed trade-offs
- kubeadm-based clusters
- Cluster API concepts
- Bootstrap clusters
- Cluster creation pipelines
- Version skew policy
- Control plane upgrades
- Worker upgrades
- Surge upgrades and rolling node replacement
- Add-on lifecycle management
- Certificate rotation
- etcd maintenance
- Draining and cordoning workflows
- Node image management
- Decommissioning workflows
- End-of-life planning
- Support windows and release cadence management

## 20. Multi-cluster, fleet, and global platform patterns
- Why multi-cluster is adopted
- Region-based cluster design
- Compliance-based cluster separation
- Tenant-based cluster separation
- Fleet management
- Central policy propagation
- Central identity and trust
- Shared services patterns
- Global traffic steering concepts
- Cross-cluster failover
- Multi-cluster service discovery
- Cluster placement policies
- Workload mobility limits
- Federation concepts
- Fleet observability
- Fleet security posture management

## 21. Platform engineering and internal developer platforms
- Platform team operating model
- Product mindset for platform teams
- Internal developer platform concepts
- Self-service provisioning
- Golden paths
- Paved roads
- Platform APIs
- Developer portals
- Service catalogs
- Standardized templates
- Secure-by-default abstractions
- Namespace vending
- Environment lifecycle automation
- Developer experience metrics
- Adoption and platform governance
- Chargeback and cost transparency for teams

## 22. Governance, policy, and compliance
- Governance operating model
- Policy lifecycle management
- Exception workflows
- Separation of duties
- Change management
- Audit readiness
- Compliance evidence collection
- Labeling and metadata standards
- Resource ownership tagging
- Data residency controls
- Retention policy alignment
- Budget guardrails
- Cost governance
- Showback and chargeback
- Regulatory segmentation
- Governance across fleets

## 23. Multi-tenancy models
- Soft multi-tenancy
- Hard multi-tenancy
- Namespace-per-team patterns
- Cluster-per-tenant patterns
- Virtual cluster concepts
- Tenant identity isolation
- Tenant network isolation
- Tenant storage isolation
- Tenant quota enforcement
- Tenant observability boundaries
- Shared ingress concerns
- Noisy neighbor risk management
- Support and escalation boundaries

## 24. Backup, disaster recovery, and business continuity
- etcd backups
- PV backups
- Snapshot orchestration
- Application-consistent backup patterns
- Velero concepts
- RPO definition
- RTO definition
- Recovery runbooks
- Control plane recovery
- Data plane recovery
- Region failover patterns
- Active-passive models
- Active-active considerations
- Recovery testing
- Restore validation
- Corruption recovery planning
- Business continuity governance

## 25. Cost, performance, and efficiency engineering
- Rightsizing practices
- Request/limit tuning
- Cluster density optimization
- Autoscaling efficiency
- Idle capacity management
- Spot and preemptible node strategies
- Storage cost classes
- Egress cost awareness
- Image optimization
- Startup latency reduction
- Scheduling for utilization
- Cost visibility by namespace/team/app
- Unit economics for platform usage
- Performance benchmarking
- Cost-performance trade-off decisions

## 26. Cloud provider and hybrid integration
- Cloud load balancers
- Cloud block and file storage integration
- Cloud IAM and workload identity
- Private connectivity
- VPC/VNet integration
- Hybrid networking
- On-prem to cloud cluster connectivity
- DNS and certificate integration

## 27. Stateful platforms and data-intensive systems
- Databases on Kubernetes
- Operators for data systems
- Message brokers and streaming systems
- Search engines on Kubernetes
- Caching systems
- Object storage gateways
- Data gravity
- Persistent I/O tuning
- Recovery guarantees
- Backup and failover planning
- Performance isolation
- When not to place stateful workloads on Kubernetes

## 28. Edge, disconnected, and specialized deployments
- Edge cluster topology
- Single-node and compact clusters
- Remote cluster lifecycle management
- Intermittent connectivity handling
- Air-gapped environments
- Offline image distribution
- Update channels for disconnected sites
- Edge observability constraints
- Small-footprint distributions
- Industrial and healthcare edge considerations
- Security patching in disconnected environments

## 29. Ecosystem and extensibility
- Operators pattern
- controller-runtime ecosystem
- API aggregation
- Admission webhooks
- Service mesh ecosystem
- Gateway controllers
- Ingress controller choices
- cert-manager
- external-dns
- Secret management operators
- Policy engines
- Backup operators
- Cost tooling ecosystem
- Developer portal ecosystem
- Platform packaging tools
- Cluster add-on management

## 30. Production readiness and operational excellence
- HA validated
- Backups tested
- Restore tested
- Upgrade plan documented
- Rollback procedures documented
- RBAC least privilege reviewed
- Pod security enforced
- Network policies in place
- Secrets encrypted
- Cert rotation automated
- Monitoring and alerting operational
- Logging centralized
- SLOs defined
- Runbooks tested
- DR exercises performed
- Capacity and cost controls active
- Ownership and escalation matrix defined

## 31. Common pitfalls and anti-patterns
- Using cluster-admin broadly
- Missing resource requests and limits
- No probes
- No PDBs for critical services
- Hardcoded secrets
- Tag-based image drift
- Over-privileged containers
- Excessive hostPath usage
- No network policies
- No backup testing
- In-place changes outside GitOps
- Ignoring upgrade planning
- Weak tenancy boundaries
- Overengineering the platform too early
- Running unsuitable stateful systems without SRE maturity



## 32. Kubernetes commands and CLI coverage
### kubectl fundamentals
- Installing kubectl
- kubectl version
- kubectl cluster-info
- kubectl api-resources
- kubectl api-versions
- kubectl explain
- kubectl completion
- kubectl plugin concepts
- kubectl aliases and productivity patterns

### kubeconfig and context management
- kubeconfig file structure
- kubectl config view
- kubectl config get-contexts
- kubectl config current-context
- kubectl config use-context
- kubectl config set-context
- kubectl config set-cluster
- kubectl config set-credentials
- Merging multiple kubeconfig files
- Context switching safety practices

### Resource inspection and discovery
- kubectl get
- kubectl describe
- kubectl explain
- kubectl get events
- kubectl top
- kubectl api-resources
- kubectl api-versions
- Namespaced vs all-namespaces queries
- Wide, YAML, and JSON output
- Resource filtering with labels and field selectors

### Manifest creation and lifecycle commands
- kubectl create
- kubectl apply
- kubectl replace
- kubectl delete
- kubectl edit
- kubectl patch
- kubectl diff
- kubectl label
- kubectl annotate
- kubectl kustomize
- Declarative apply workflows
- Server-side apply awareness

### Workload operations
- kubectl scale
- kubectl rollout status
- kubectl rollout history
- kubectl rollout restart
- kubectl rollout undo
- kubectl set image
- kubectl autoscale
- kubectl wait
- Safe rollout verification workflows

### Pod interaction and debugging
- kubectl logs
- kubectl exec
- kubectl attach
- kubectl cp
- kubectl port-forward
- kubectl debug
- kubectl describe pod
- kubectl get pod -o yaml
- Previous container logs
- Multi-container pod debugging patterns

### Node and cluster maintenance commands
- kubectl cordon
- kubectl uncordon
- kubectl drain
- kubectl taint
- kubectl describe node
- kubectl top node
- kubectl get nodes
- Safe node maintenance workflow
- Pod eviction behavior during drain

### Authentication, authorization, and access checks
- kubectl auth can-i
- kubectl auth reconcile
- Token and certificate usage awareness
- ServiceAccount inspection commands
- RBAC troubleshooting workflows
- Impersonation flags awareness

### Output formatting and query techniques
- -o yaml
- -o json
- -o wide
- jsonpath output
- custom-columns output
- go-template awareness
- Label selectors
- Field selectors
- Namespace flags
- Watch mode and live observation

### Namespace and object scoping patterns
- -n and --namespace usage
- -A and --all-namespaces usage
- Context-bound namespaces
- Cluster-scoped resource handling
- Cross-namespace troubleshooting patterns

### Imperative generators and practical shortcuts
- kubectl run
- kubectl expose
- kubectl create deployment
- kubectl create namespace
- kubectl create configmap
- kubectl create secret
- Dry-run client and server modes
- Exporting generated YAML for declarative workflows

## 33. Package management and configuration CLI coverage
### Helm commands
- helm version
- helm repo add
- helm repo update
- helm search repo
- helm search hub awareness
- helm show values
- helm show chart
- helm template
- helm lint
- helm install
- helm upgrade
- helm rollback
- helm uninstall
- helm list
- helm status
- helm history
- helm dependency update
- values files and --set usage
- Release troubleshooting patterns

### Kustomize commands
- kubectl kustomize
- kustomize build
- Bases and overlays workflow
- Patch strategies
- Image overrides
- ConfigMap and Secret generation
- Environment-specific build validation

## 34. Runtime, node, and lower-level operational CLI coverage
### etcdctl
- etcdctl endpoint health
- etcdctl endpoint status
- etcdctl snapshot save
- etcdctl snapshot restore
- etcdctl member list
- etcdctl alarm list
- TLS flags and certificate handling for etcdctl

### Container runtime tools
- crictl ps
- crictl images
- crictl inspect
- crictl logs
- crictl pods
- ctr basics awareness
- nerdctl awareness
- Runtime troubleshooting at node level

### Linux and node diagnostics around Kubernetes
- journalctl for kubelet and runtime logs
- systemctl for kubelet and runtime services
- iptables/ipvs awareness
- ss/netstat awareness
- nslookup/dig for DNS troubleshooting
- curl and wget for service checks
- df, free, top, and vmstat for node health
- tcpdump awareness for packet troubleshooting

## 35. Ecosystem and platform CLI coverage
### GitOps and delivery CLIs
- argocd login
- argocd app list
- argocd app get
- argocd app sync
- argocd app rollback
- flux get all
- flux reconcile
- flux suspend and resume awareness

### Service mesh and networking CLIs
- istioctl analyze
- istioctl proxy-status
- istioctl x describe
- cilium status
- cilium connectivity test awareness
- calicoctl basics awareness
- ingress controller troubleshooting CLIs where relevant

### Certificate, DNS, and secret ecosystem CLIs
- cmctl basics awareness
- external-dns operational awareness
- secret manager integration CLIs where relevant

## 36. Command usage patterns, safety, and operational discipline
- Read-only inspection before mutation
- Prefer declarative workflows over ad hoc mutation
- Safe use of production contexts
- Confirmation and dry-run practices
- Namespace scoping discipline
- Auditability of command use
- Break-glass operational commands
- Rollback-first mindset
- Troubleshooting workflow from get -> describe -> logs -> exec/debug
- Node maintenance workflow from cordon -> drain -> maintain -> uncordon
- Safe restore testing for etcd and backups
- Avoiding direct production edits when GitOps is authoritative
- Using commands as part of runbooks and incident response

