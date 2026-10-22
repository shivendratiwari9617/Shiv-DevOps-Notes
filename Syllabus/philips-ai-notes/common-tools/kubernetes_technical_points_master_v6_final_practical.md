# Kubernetes Technical Coverage Points — Final Practical Master List

A high-signal Kubernetes roadmap from basic to advanced, refined for real-world use by removing overlap, merging closely related topics, and keeping the concepts most useful in practice.

## 1. Kubernetes Foundations

1. What Kubernetes is, why it exists, and the problems it solves
2. Containers vs virtual machines
3. Container images, registries, tags, and digests
4. Linux isolation basics: namespaces, cgroups, and filesystems
5. Declarative model, desired state, reconciliation, and self-healing
6. Scaling, high availability, and failure domains
7. Managed vs self-managed Kubernetes clusters

## 2. Cluster Architecture

8. Control plane and worker node responsibilities
9. kube-apiserver, etcd, scheduler, controller-manager, kubelet, and kube-proxy roles
10. Control plane endpoint and highly available cluster design
11. Worker node components and container runtime basics
12. East-west vs north-south traffic
13. Single-cluster, multi-cluster, private, hybrid, and edge patterns

## 3. Cluster Bootstrapping and Access

14. kubeadm init, kubeadm join, and cluster bootstrap flow
15. Static Pod based control plane bootstrap
16. Bootstrap tokens and node join trust model
17. kubeconfig structure: clusters, users, and contexts
18. CNI installation after cluster bootstrap
19. Bare-metal vs managed cluster provisioning basics

## 4. Kubernetes API and Object Model

20. API groups, versions, kinds, resources, and objects
21. Namespaced vs cluster-scoped resources
22. Object structure: metadata, spec, and status
23. Labels, selectors, and annotations
24. OwnerReferences, finalizers, and garbage collection
25. resourceVersion, generation, and observedGeneration
26. YAML manifest structure, validation, and defaulting
27. List, watch, patch, and server-side apply basics

## 5. Authentication, Authorization, and Admission

28. Authentication methods: certificates, tokens, OIDC, and webhooks
29. Authorization modes with focus on RBAC
30. Roles, ClusterRoles, RoleBindings, and ClusterRoleBindings
31. ServiceAccounts and workload identity basics
32. Admission control, built-in plugins, and admission webhooks
33. Pod security standards and namespace enforcement
34. Audit logging and request tracing basics

## 6. etcd and Control Plane State

35. etcd role in Kubernetes and why it matters
36. Raft basics: quorum, leader, and consistency
37. Snapshots, restore, and disaster recovery basics
38. Peer/client TLS and etcd health checks
39. Compaction, defragmentation, and operational care

## 7. Pods and Workload Controllers

40. Pod lifecycle and restart behavior
41. Multi-container Pods, init containers, and sidecar pattern
42. ReplicaSet and Deployment for stateless applications
43. StatefulSet for stable identity and persistent storage
44. DaemonSet for node-level agents
45. Job and CronJob for batch and scheduled workloads
46. Static Pods and when they matter
47. Controller reconciliation and rollout ownership

## 8. Pod Specification and Runtime Behavior

48. Commands, args, env, envFrom, ports, and imagePullPolicy
49. Liveness, readiness, and startup probes
50. Resource requests, limits, and QoS classes
51. SecurityContext at pod and container level
52. Graceful shutdown, termination signals, and lifecycle hooks
53. Image pull secrets and private registry access
54. Ephemeral containers for live debugging

## 9. Scheduling and Placement

55. kube-scheduler workflow and scheduling decisions
56. Requests, limits, and bin-packing basics
57. Node selectors, node affinity, and pod affinity/anti-affinity
58. Taints, tolerations, and dedicated nodes
59. Topology spread constraints and zone-aware placement
60. PriorityClass, preemption, and eviction basics
61. PodDisruptionBudget and workload availability during maintenance

## 10. Services, Networking, and Service Discovery

62. Pod networking model and CNI basics
63. Service types: ClusterIP, NodePort, LoadBalancer, and ExternalName
64. kube-proxy behavior and service virtual IP routing
65. DNS-based service discovery with CoreDNS
66. Endpoints and EndpointSlices
67. Ingress and ingress controller basics
68. NetworkPolicy for traffic control between workloads
69. Internal and external traffic flow
70. Port-forwarding and connectivity debugging

## 11. Storage and Persistent Data

71. Volumes and pod-level storage lifecycle
72. PersistentVolume, PersistentVolumeClaim, and StorageClass
73. Dynamic provisioning and CSI basics
74. Access modes, reclaim policy, and binding behavior
75. Volume expansion and resize basics
76. Stateful storage design patterns
77. Volume snapshots, backup, and restore concepts
78. Troubleshooting storage attach, mount, and claim issues

## 12. Configuration and Secrets

79. ConfigMaps and secret usage patterns
80. Environment variable injection and file-based mounts
81. Immutable configuration patterns
82. Secret types, secret rotation, and protection basics
83. External secret integration concepts

## 13. Security in Practice

84. Workload identity and least-privilege RBAC
85. Secret protection, encryption at rest, and KMS basics
86. Pod and container hardening with SecurityContext
87. Network isolation with NetworkPolicy
88. Certificate management and trust chain basics
89. Auditability and cluster security observability

## 14. Node and Cluster Operations

90. Node registration, readiness, and lifecycle
91. Cordon, drain, and uncordon workflows
92. Kubelet responsibilities and node troubleshooting basics
93. Container runtime inspection basics
94. Cluster upgrade flow: control plane first, then workers
95. Certificate renewal and kubeconfig regeneration
96. etcd backup and restore workflow
97. Safe maintenance, cleanup, and recovery basics
98. Capacity planning and failure handling

## 15. Observability and Troubleshooting

99. kubectl get, describe, logs, exec, events, and top
100. Metrics-server and basic resource observability
101. Troubleshooting Pending, CrashLoopBackOff, OOMKilled, and ImagePullBackOff
102. Probe failures and rollout failures
103. DNS, Service, Ingress, and NetworkPolicy troubleshooting
104. PVC, PV, and storage troubleshooting
105. Node-level troubleshooting with kubelet and runtime logs
106. Control plane troubleshooting with kube-system components
107. Debug containers, temporary test pods, and port-forward workflows
108. Rollout, rollback, and release troubleshooting

## 16. Scaling, Availability, and Reliability

109. Manual scaling and replica management
110. Horizontal Pod Autoscaler
111. Vertical Pod Autoscaler and Cluster Autoscaler concepts
112. Stateless high availability patterns
113. Stateful availability basics and disruption handling
114. Failure recovery and resilience during upgrades or node loss

## 17. Packaging and Delivery

115. Manifest-based deployment workflows
116. Kustomize basics and overlays
117. Helm charts, values, templating, install, upgrade, rollback, and uninstall
118. Release versioning and rollout strategies
119. Blue-green and canary deployment concepts
120. GitOps basics with tools such as Argo CD and Flux

## 18. Extensibility and Production Readiness

121. CRDs, custom resources, and the operator pattern
122. Admission webhooks and API extensibility basics
123. Namespaces, labeling strategy, and environment separation
124. Resource hygiene: requests, limits, quotas, and limits policies
125. Backup, restore, upgrade planning, and version skew awareness
126. Runbooks, operational discipline, and production best practices
