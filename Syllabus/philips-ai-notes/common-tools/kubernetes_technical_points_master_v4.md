# Kubernetes Technical Coverage Points — Canonical Master List

Comprehensive coverage map from very basic to very advanced Kubernetes topics.

## 1. Foundations and Core Concepts

1. What Kubernetes is
2. Problems Kubernetes solves
3. Containers vs virtual machines
4. Container lifecycle basics
5. Container images
6. Image layers
7. Container registries
8. Image tags and image digests
9. Linux namespaces
10. cgroups
11. Union filesystems
12. Process isolation
13. Network isolation
14. Filesystem isolation
15. Ephemeral workloads
16. Immutable infrastructure concepts
17. Declarative operations
18. Imperative operations
19. Desired state
20. Reconciliation loop
21. Control loop model
22. Idempotency
23. Self-healing
24. Horizontal scaling concepts
25. Vertical scaling concepts
26. High availability concepts
27. Fault domains
28. Failure domains
29. Platform engineering with Kubernetes
30. Managed vs self-managed Kubernetes
31. Kubernetes distribution types

## 2. Cluster Architecture and Topology

32. Cluster architecture
33. Control plane vs worker nodes
34. Single-node cluster model
35. Multi-node cluster model
36. Highly available control plane
37. Stacked etcd topology
38. External etcd topology
39. Control plane endpoint
40. Control plane load balancer
41. North-south traffic model
42. East-west traffic model
43. Single-cluster design
44. Multi-cluster design
45. Hub-and-spoke fleet model
46. Regional cluster design
47. Multi-region cluster strategy
48. Private cluster design
49. Air-gapped cluster design
50. Edge cluster design
51. Hybrid cloud cluster design
52. On-premises cluster design
53. Blast radius design
54. Availability zone design
55. Control plane failure scenarios
56. Worker node failure scenarios

## 3. Cluster Bootstrapping, Installation, and PKI

57. Cluster bootstrap concepts
58. kubeadm
59. kubeadm init
60. kubeadm join
61. Bootstrap tokens
62. Discovery token CA cert hash
63. Static Pod control plane bootstrap
64. PKI directory layout
65. Admin kubeconfig
66. Controller manager kubeconfig
67. Scheduler kubeconfig
68. Kubelet kubeconfig
69. Kubeconfig structure
70. Contexts, clusters, and users in kubeconfig
71. ClusterConfiguration
72. InitConfiguration
73. JoinConfiguration
74. KubeletConfiguration
75. KubeProxyConfiguration
76. Control plane certificate upload
77. Joining additional control plane nodes
78. Certificate generation during bootstrap
79. Certificate renewal
80. Certificate rotation
81. Kubelet client certificate rotation
82. Kubelet serving certificate rotation
83. Front-proxy certificates
84. Service account signing keys
85. External CA model
86. Bootstrapping stacked etcd
87. Bootstrapping external etcd
88. Addon installation during bootstrap
89. CNI installation after bootstrap
90. Bare-metal installation patterns
91. Managed cluster provisioning workflows

## 4. API, Object Model, and Resource Semantics

92. Kubernetes API
93. API groups
94. API versions
95. Core API group
96. Namespaced resources
97. Cluster-scoped resources
98. Kinds
99. Resources
100. Objects
101. Metadata
102. Spec
103. Status
104. Labels
105. Equality-based label selectors
106. Set-based label selectors
107. Annotations
108. Namespaces
109. UIDs
110. OwnerReferences
111. Finalizers
112. Generation
113. ObservedGeneration
114. resourceVersion
115. CreationTimestamp
116. DeletionTimestamp
117. Managed fields
118. Server-side apply
119. Field management
120. Field ownership conflicts
121. Strategic merge patch
122. JSON merge patch
123. JSON patch
124. YAML manifest structure
125. apiVersion field
126. kind field
127. metadata field
128. spec field
129. status field
130. OpenAPI schema
131. Defaulting
132. Validation
133. Schema pruning
134. Unknown field handling
135. Nullable fields
136. Subresources
137. Status subresource
138. Scale subresource
139. List operations
140. Watch operations
141. List-watch pattern
142. Watch bookmarks
143. Field selectors
144. Pagination and continue tokens
145. Graceful deletion semantics
146. Optimistic concurrency

## 5. kube-apiserver and API Machinery

147. kube-apiserver role
148. API request lifecycle
149. Authentication pipeline
150. Authorization pipeline
151. Admission chain
152. Mutating admission
153. Validating admission
154. Built-in admission plugins
155. Admission webhooks
156. Admission webhook failurePolicy
157. Admission webhook matchPolicy
158. Admission webhook sideEffects
159. Admission webhook timeout behavior
160. ValidatingAdmissionPolicy
161. CEL-based admission rules
162. MutatingAdmissionPolicy concepts
163. Conversion webhooks
164. CRD registration
165. API aggregation layer
166. Aggregated API servers
167. Aggregated API server failure modes
168. OpenAPI publishing
169. Discovery endpoints
170. Storage version conversion
171. StorageVersion API concepts
172. API deprecation policy
173. Feature gates
174. Audit logging
175. Audit policy
176. Audit backends
177. Audit stages
178. Encryption at rest
179. Encryption providers
180. KMS integration
181. KMS v2 concepts
182. API priority and fairness
183. Flow schemas
184. Priority levels
185. API priority and fairness tuning
186. Long-running requests
187. Request timeout handling
188. API server horizontal scaling
189. API server high availability
190. API server identity leases
191. Front-proxy pattern
192. Request header authentication
193. Client certificate authentication
194. Bearer token authentication
195. OIDC authentication
196. Webhook token authentication
197. Anonymous authentication
198. RBAC authorization
199. Webhook authorization
200. Node authorization
201. NodeRestriction admission
202. Impersonation

## 6. etcd and State Storage

203. etcd role
204. Key-value storage model
205. Raft consensus
206. Raft leader election
207. Raft followers
208. Raft quorum
209. Write-ahead log
210. Snapshots
211. Snapshot restore
212. Cluster membership
213. Member add
214. Member remove
215. Learner members
216. Compaction
217. Defragmentation
218. Revision history
219. Watch history window
220. Consistent reads
221. Linearizable reads
222. Serializable reads
223. Lease semantics in etcd
224. Peer communication
225. Peer TLS
226. Client TLS
227. Endpoint health
228. Endpoint status
229. Alarm conditions
230. Space quota exhaustion
231. Fragmentation effects
232. IOPS sensitivity
233. Latency sensitivity
234. Data corruption scenarios
235. Backup strategy
236. Restore validation
237. Disaster recovery for etcd

## 7. Scheduler, Placement, and Eviction

238. kube-scheduler role
239. Scheduling queue
240. Active queue
241. Backoff queue
242. Unschedulable queue
243. Scheduling cycle
244. Binding cycle
245. Filter phase
246. Score phase
247. Normalize scoring
248. Reserve phase
249. Permit phase
250. PreBind phase
251. Bind phase
252. PostBind phase
253. Preemption
254. Nominated node
255. PriorityClass
256. Pod priority
257. NodeResourcesFit
258. nodeSelector
259. Node affinity
260. Required node affinity
261. Preferred node affinity
262. Pod affinity
263. Pod anti-affinity
264. Inter-pod affinity topology key
265. Topology spread constraints
266. Taints
267. Tolerations
268. Taint-based eviction
269. Extended resources
270. GPU and device-aware scheduling
271. Volume-aware scheduling
272. WaitForFirstConsumer
273. Image locality scoring
274. Node unschedulable state
275. Scheduler plugins
276. Scheduler framework
277. Scheduler profiles
278. Custom scheduler
279. Multiple schedulers
280. Queue sort plugins
281. Descheduler concepts
282. Descheduler policies
283. Unschedulable Pod backoff
284. Scheduling events

## 8. Controllers and Reconciliation

285. kube-controller-manager role
286. Shared informers
287. Lister caches
288. Work queues
289. Reconciliation model
290. Retry backoff
291. Leader election for controllers
292. Deployment controller
293. ReplicaSet controller
294. StatefulSet controller
295. DaemonSet controller
296. Job controller
297. CronJob controller
298. Namespace controller
299. Node controller
300. ServiceAccount controller
301. Endpoint controller
302. EndpointSlice controller
303. PersistentVolume controller
304. AttachDetach controller
305. Garbage collector
306. TTL-after-finished controller
307. ResourceQuota controller
308. HorizontalPodAutoscaler controller
309. CertificateSigningRequest controller
310. Disruption controller
311. Pod garbage collection
312. Controller ownership model

## 9. cloud-controller-manager and Cloud Integration

313. cloud-controller-manager role
314. Cloud-provider abstraction
315. Cloud node controller
316. Provider ID
317. Node initialization
318. Region labels
319. Zone labels
320. Cloud load balancer controller
321. Route controller
322. Cloud route programming
323. Cloud volume lifecycle hooks
324. External cloud provider model
325. Cloud API retry behavior
326. Cloud failure modes
327. Provisioning lag
328. LoadBalancer Service provisioning
329. Node address population
330. Cloud network interface reconciliation

## 10. Node Architecture and Node Lifecycle

331. Worker node architecture
332. Node registration
333. Node object
334. Node conditions
335. Node heartbeat
336. Lease renewals
337. Node allocatable
338. Capacity vs allocatable
339. Node labels
340. Node taints
341. Cordoned nodes
342. Drained nodes
343. MemoryPressure
344. DiskPressure
345. PIDPressure
346. NetworkUnavailable
347. Ready condition
348. NotReady state
349. Unreachable state
350. Graceful node shutdown
351. Node problem detection
352. Host OS considerations
353. Kernel dependencies
354. Time synchronization
355. Swap behavior
356. cgroup driver alignment
357. System-reserved resources
358. Kube-reserved resources
359. Node replacement workflow
360. Node decommission workflow
361. Windows node considerations

## 11. kubelet Internals

362. kubelet role
363. Pod sync loop
364. Pod workers
365. Static Pods
366. Mirror Pods
367. Pod admission on node
368. Pod sandbox creation
369. Sandbox recreation
370. Container start sequence
371. Image pull workflow
372. Image pull policy
373. Image credential resolution
374. ImagePullSecrets
375. Readiness probes
376. Liveness probes
377. Startup probes
378. HTTP probes
379. TCP probes
380. Exec probes
381. gRPC probe support concepts
382. Lifecycle hooks
383. postStart hook
384. preStop hook
385. Volume mounting
386. Secret projection
387. ConfigMap projection
388. Projected volumes
389. Downward API projection
390. Pod status updates
391. Container status updates
392. Restart policy enforcement
393. Graceful termination handling
394. Eviction manager
395. Soft eviction thresholds
396. Hard eviction thresholds
397. Image garbage collection
398. Container garbage collection
399. Container log handling
400. Node status reporting
401. Device plugin registration
402. Pod resources API
403. RuntimeClass selection
404. Ephemeral container support
405. CPU Manager
406. Topology Manager
407. Memory Manager
408. Node allocatable enforcement
409. Credential provider plugins
410. Checkpoint API concepts

## 12. Container Runtime and Execution

411. Container runtime
412. CRI
413. CRI runtime service
414. CRI image service
415. PodSandbox model
416. Pause container
417. OCI image spec
418. OCI runtime spec
419. containerd
420. CRI-O
421. runc
422. Shim processes
423. gVisor
424. Kata Containers
425. Snapshotters
426. OverlayFS snapshotter
427. stargz snapshotter concepts
428. Image layer unpacking
429. Container filesystem assembly
430. Container process lifecycle
431. Runtime sandbox failure scenarios
432. Runtime isolation models
433. Rootless containers considerations
434. User namespaces in containers
435. Seccomp interaction with runtime
436. AppArmor interaction with runtime
437. SELinux interaction with runtime

## 13. Pods and Container Model

438. Pod
439. Single-container Pod
440. Multi-container Pod
441. Init containers
442. Sidecar containers
443. Ephemeral containers
444. Pod phases
445. Pod conditions
446. Pod readiness gates
447. Pod scheduling gates
448. Pod IP
449. Pod DNS
450. Pod hostname
451. Pod subdomain
452. Shared network namespace
453. Shared IPC namespace
454. Shared PID namespace
455. Shared volumes in Pod
456. Pod restart policy
457. Pod security context
458. Container security context
459. CPU requests
460. Memory requests
461. Ephemeral storage requests
462. CPU limits
463. Memory limits
464. Ephemeral storage limits
465. QoS classes
466. Guaranteed QoS
467. Burstable QoS
468. BestEffort QoS
469. Pod disruption behavior
470. TerminationGracePeriodSeconds
471. Pod immutability rules
472. Pod debugging workflow
473. Pod overhead
474. Host aliases
475. DNS policy
476. dnsConfig

## 14. Workload Controllers

477. ReplicationController
478. ReplicaSet
479. Deployment
480. Deployment rollout
481. RollingUpdate strategy
482. Recreate strategy
483. maxUnavailable
484. maxSurge
485. Rollback
486. Revision history
487. Paused rollout
488. Progress deadline
489. StatefulSet
490. OrderedReady policy
491. Parallel Pod management policy
492. Stable network identity
493. Stable storage identity
494. PersistentVolumeClaim templates
495. Headless Service for StatefulSet
496. StatefulSet update strategy
497. Partitioned rollout in StatefulSet
498. DaemonSet
499. Node-targeted agents
500. DaemonSet RollingUpdate
501. OnDelete update strategy
502. Job
503. Parallelism
504. Completions
505. BackoffLimit
506. ActiveDeadlineSeconds
507. PodFailurePolicy
508. SuccessPolicy concepts
509. Indexed Jobs
510. CronJob
511. Cron schedule syntax
512. ConcurrencyPolicy
513. StartingDeadlineSeconds
514. Suspended CronJobs

## 15. Configuration, Secrets, and Service Accounts

515. ConfigMap
516. Secret
517. Opaque Secret
518. TLS Secret
519. dockerconfigjson Secret
520. Environment variable injection
521. envFrom
522. ConfigMap volumes
523. Secret volumes
524. Projected volumes
525. Immutable ConfigMaps
526. Immutable Secrets
527. ServiceAccount
528. Default ServiceAccount
529. ServiceAccount token projection
530. Projected service account tokens
531. BoundServiceAccountTokenVolume
532. External secret operators
533. CSI secret store patterns
534. Configuration drift
535. Hot-reload patterns
536. Checksum annotation restart pattern

## 16. Services and Discovery

537. Service
538. ClusterIP
539. Headless Service
540. NodePort
541. LoadBalancer Service
542. ExternalName Service
543. Service selectors
544. Services without selectors
545. Cluster DNS
546. CoreDNS
547. CoreDNS plugin chain
548. DNS search paths
549. Service VIP
550. Virtual IP routing
551. Session affinity
552. ExternalTrafficPolicy
553. InternalTrafficPolicy
554. HealthCheckNodePort
555. Node-local traffic handling
556. Topology-aware routing hints
557. Endpoint object
558. EndpointSlice object
559. Service discovery conventions
560. SRV records
561. DNS failure scenarios

## 17. Networking Model and Dataplane

562. Kubernetes networking model
563. Pod-to-Pod communication
564. Pod-to-Service communication
565. Service-to-Pod routing
566. Node-to-Pod communication
567. Pod CIDR
568. Service CIDR
569. Cluster DNS domain
570. CNI
571. IPAM
572. veth pair
573. Bridge networking
574. Overlay networking
575. VXLAN
576. Geneve
577. Underlay networking
578. BGP-based routing
579. Route programming
580. Encapsulation
581. MTU tuning
582. Hairpin traffic
583. HostNetwork Pods
584. hostPort
585. Dual-stack networking
586. IPv4 clusters
587. IPv6 clusters
588. Dual-stack Services
589. NodeLocal DNSCache
590. eBPF dataplane
591. kube-proxy replacement

## 18. kube-proxy, Conntrack, and Service Routing

592. kube-proxy role
593. iptables mode
594. IPVS mode
595. Userspace proxy mode legacy
596. Service rule programming
597. ClusterIP datapath
598. NodePort datapath
599. External IP handling
600. Session affinity implementation
601. EndpointSlice consumption
602. Service update propagation
603. Topology-aware hints
604. Conntrack interaction
605. Conntrack exhaustion scenarios
606. kube-proxy-free cluster operations

## 19. Ingress, Gateway API, and Traffic Management

607. Ingress resource
608. Ingress controller
609. IngressClass
610. Default IngressClass
611. Host-based routing
612. Path-based routing
613. TLS termination
614. Wildcard hosts
615. Rewrite rules
616. Canary ingress patterns
617. Gateway API
618. GatewayClass
619. Gateway
620. Listener
621. HTTPRoute
622. TCPRoute
623. TLSRoute
624. GRPCRoute
625. UDPRoute concepts
626. ReferenceGrant
627. Traffic splitting
628. Weighted routing
629. Blue-green traffic management
630. Retry policy
631. Timeout policy
632. Mirroring traffic
633. North-south gateway patterns
634. East-west gateway patterns

## 20. Network Security and Policy

635. NetworkPolicy
636. Ingress NetworkPolicy
637. Egress NetworkPolicy
638. Default deny ingress
639. Default deny egress
640. Namespace isolation
641. Pod selector semantics
642. Namespace selector semantics
643. IPBlock rules
644. Policy enforcement by CNI
645. L3/L4 policy model
646. DNS egress policy patterns
647. NetworkPolicy limitations by CNI
648. Multi-tenant network segmentation
649. Egress gateway patterns
650. mTLS in service mesh

## 21. Storage, CSI, and Data Protection

651. Kubernetes volume model
652. emptyDir
653. hostPath
654. configMap volume
655. secret volume
656. projected volume
657. PersistentVolume
658. PersistentVolumeClaim
659. StorageClass
660. Dynamic provisioning
661. Static provisioning
662. Reclaim policy
663. Retain reclaim policy
664. Delete reclaim policy
665. Recycle policy legacy
666. Access modes
667. ReadWriteOnce
668. ReadOnlyMany
669. ReadWriteMany
670. ReadWriteOncePod
671. Filesystem volume mode
672. Block volume mode
673. Immediate binding
674. WaitForFirstConsumer binding
675. CSI
676. CSI controller plugin
677. CSI node plugin
678. CSI external provisioner
679. CSI attacher
680. CSI resizer
681. CSI snapshotter
682. CSI sidecar failure handling
683. Volume attachment
684. Mount propagation
685. Volume snapshots
686. VolumeSnapshotClass
687. Volume cloning
688. Online volume expansion
689. Offline volume expansion
690. Raw block volumes
691. Topology-aware storage
692. Stateful storage patterns
693. Attach failure scenarios
694. Mount failure scenarios
695. Backup consistency considerations

## 22. Security, Identity, and Policy Enforcement

696. RBAC
697. Role
698. ClusterRole
699. RoleBinding
700. ClusterRoleBinding
701. Aggregation rules for ClusterRoles
702. Workload identity
703. Node identity
704. Bootstrap node identity
705. Client certificate authentication
706. Certificate signer concepts
707. OIDC identity integration
708. Webhook authentication and authorization
709. Pod Security Standards
710. Pod Security Admission
711. Baseline profile
712. Restricted profile
713. Privileged profile
714. securityContext
715. runAsNonRoot
716. readOnlyRootFilesystem
717. allowPrivilegeEscalation
718. privileged containers
719. hostPID
720. hostIPC
721. hostNetwork security implications
722. Linux capabilities
723. Seccomp
724. AppArmor
725. SELinux
726. fsGroup
727. Supplemental groups
728. sysctls
729. Admission policy
730. OPA Gatekeeper
731. Kyverno
732. Secret encryption
733. KMS providers
734. Image signing concepts
735. Image provenance
736. Software bill of materials
737. Supply chain security
738. Control-plane certificate expiry recovery

## 23. Resource Governance, Isolation, and Capacity

739. ResourceQuota
740. LimitRange
741. Default requests
742. Default limits
743. Namespace capacity control
744. CPU overcommit
745. Memory overcommit
746. Ephemeral storage quotas
747. HugePages
748. NUMA-aware placement
749. Dedicated node pools
750. GPU scheduling
751. Device plugins
752. RuntimeClass isolation
753. Taints for isolation
754. Tolerations for dedicated workloads
755. Pod overhead
756. Node allocatable reservations
757. System-reserved and kube-reserved tuning
758. Cluster capacity planning
759. Bin-packing strategy

## 24. Autoscaling and Elasticity

760. HorizontalPodAutoscaler
761. autoscaling/v2
762. CPU-based scaling
763. Memory-based scaling
764. Custom metrics scaling
765. External metrics scaling
766. Metrics API
767. Custom Metrics API
768. External Metrics API
769. metrics-server
770. Metrics adapter patterns
771. Prometheus adapter patterns
772. Scale target ref
773. Min replicas
774. Max replicas
775. Scale stabilization
776. HPA behavior policies
777. VerticalPodAutoscaler
778. VPA recommendation mode
779. VPA auto mode
780. VPA and HPA interaction
781. Cluster Autoscaler
782. Node group scaling
783. Expander strategies
784. Scale-down behavior
785. Scale-from-zero
786. KEDA
787. Event-driven autoscaling

## 25. Reliability, Availability, and Resilience

788. Self-healing
789. Replica-based availability
790. Readiness gating
791. Liveness-based recovery
792. Startup protection
793. Graceful shutdown
794. PodDisruptionBudget
795. PDB edge cases during upgrades
796. Eviction API
797. Voluntary disruptions
798. Involuntary disruptions
799. Anti-affinity for high availability
800. Topology spread for high availability
801. Multi-zone resilience
802. Control plane high availability
803. etcd quorum resilience
804. Failure domain awareness
805. SLOs
806. SLIs
807. Error budgets
808. Canary analysis
809. Chaos engineering
810. Fault injection testing

## 26. Observability, Telemetry, and Diagnostics

811. Cluster logging
812. Container stdout/stderr logging
813. Node log collection
814. Centralized logging stack
815. Metrics
816. Application metrics
817. Node metrics
818. kube-state-metrics
819. Prometheus
820. Alerting
821. Tracing
822. Distributed tracing
823. OpenTelemetry
824. Events
825. Audit logs
826. Correlation across telemetry signals
827. Debug containers
828. kubectl debug
829. CrashLoopBackOff analysis
830. ImagePullBackOff analysis
831. Pending Pod diagnosis
832. Service connectivity debugging
833. DNS troubleshooting
834. NetworkPolicy troubleshooting
835. Volume mount troubleshooting
836. Scheduler diagnostics
837. kubelet diagnostics
838. etcd diagnostics

## 27. Upgrades, Version Skew, and Lifecycle Management

839. Kubernetes version skew policy
840. Version skew between control plane and kubelet
841. Version skew between kubectl and cluster
842. Control plane upgrade strategy
843. kubeadm upgrade workflow
844. Node upgrade strategy
845. Rolling node upgrades
846. Surge node upgrades
847. In-place node upgrades
848. Drain before upgrade
849. Cordoning nodes
850. Uncordoning nodes
851. PDB-aware upgrades
852. Managed node group upgrades
853. Self-managed node upgrade workflow
854. OS patching
855. Kernel upgrade considerations
856. Container runtime upgrades
857. CNI upgrades
858. CSI upgrades
859. Addon compatibility checks
860. CoreDNS upgrades
861. kube-proxy upgrades
862. kubelet upgrades
863. API deprecation review
864. Feature gate changes across upgrades
865. Certificate renewal during lifecycle
866. Blue-green cluster upgrades
867. Canary upgrades
868. Rollback planning

## 28. Backup, Disaster Recovery, and Recovery Drills

869. Disaster recovery strategy
870. Recovery point objective
871. Recovery time objective
872. Control plane backup
873. etcd backup
874. etcd restore
875. Application state backup
876. Persistent volume backup
877. Crash-consistent backup
878. Application-consistent backup
879. Volume snapshot backup
880. Restore testing
881. Backup validation drills
882. Namespace-level restore
883. Cluster-level restore
884. Cross-region recovery
885. Multi-cluster failover
886. Backup retention policy
887. Recovery runbooks
888. Control plane rebuild
889. Control plane static Pod recovery
890. Worker node replacement
891. Secret recovery strategy
892. Certificate recovery
893. DNS recovery
894. Ingress and load balancer recovery

## 29. Packaging, Delivery, GitOps, and Release Engineering

895. Helm
896. Helm charts
897. Chart dependencies
898. Chart repositories
899. Values files
900. Templates
901. Release lifecycle
902. helm install
903. helm upgrade
904. helm rollback
905. helm history
906. helm diff patterns
907. Kustomize
908. Bases
909. Overlays
910. Strategic merge patches in Kustomize
911. JSON patches in Kustomize
912. GitOps
913. Desired state delivery
914. Drift detection
915. Argo CD
916. Flux
917. Sync waves
918. Hooks
919. Secret management in GitOps
920. Progressive delivery
921. Canary rollout controllers
922. Release safety checks

## 30. Commands, Tools, and Day-2 Operations

923. kubectl get
924. kubectl describe
925. kubectl apply
926. kubectl create
927. kubectl delete
928. kubectl edit
929. kubectl patch
930. kubectl replace
931. kubectl diff
932. kubectl logs
933. kubectl exec
934. kubectl attach
935. kubectl cp
936. kubectl port-forward
937. kubectl top
938. kubectl events
939. kubectl rollout
940. kubectl rollout restart
941. kubectl rollout status
942. kubectl rollout undo
943. kubectl scale
944. kubectl expose
945. kubectl label
946. kubectl annotate
947. kubectl taint
948. kubectl cordon
949. kubectl uncordon
950. kubectl drain
951. kubectl debug
952. kubectl auth can-i
953. kubectl api-resources
954. kubectl api-versions
955. kubectl explain
956. kubectl config contexts
957. kubectl config use-context
958. kubectl config current-context
959. kubectl cluster-info
960. kubectl get events
961. jsonpath output
962. custom-columns output
963. helm list
964. helm status
965. helm get values
966. helm template
967. helm lint
968. kustomize build
969. etcdctl
970. crictl
971. ctr
972. nerdctl concepts
973. journalctl
974. systemctl
975. ss and netstat for networking diagnostics
976. iptables and nft diagnostics
977. ip route diagnostics

## 31. Custom Resources, Operators, and Extensibility

978. CustomResourceDefinition
979. Structural schemas
980. CRD versions
981. Served versions
982. Stored versions
983. Conversion strategy
984. Conversion webhook
985. Additional printer columns
986. Subresources for CRDs
987. Status subresource for CRDs
988. Scale subresource for CRDs
989. Custom controllers
990. client-go informers
991. controller-runtime
992. Operators
993. Operator reconciliation patterns
994. Finalizer patterns in operators
995. Status conditions in custom resources
996. Kubebuilder concepts
997. Operator SDK concepts

## 32. Service Mesh, Multi-Cluster, and Platform Engineering

998. Service mesh
999. Sidecar injection
1000. Ambient mesh concepts
1001. mTLS
1002. Traffic policy in service mesh
1003. Service mesh operational concerns
1004. Cilium advanced networking
1005. eBPF service acceleration
1006. Cluster API
1007. Machine objects
1008. MachineDeployments
1009. Bare-metal load balancing
1010. MetalLB
1011. Node Feature Discovery
1012. Virtual kubelet concepts
1013. Cluster federation concepts
1014. Multi-cluster policy management
1015. Multi-cluster service discovery
1016. Multi-cluster traffic failover
1017. Multi-tenancy patterns
1018. Platform guardrails
1019. Golden path templates
1020. Internal developer platforms
1021. FinOps in Kubernetes
1022. Cost allocation by namespace
1023. Compliance controls in Kubernetes
1024. Policy as code
1025. Workload placement governance
1026. Advanced scheduling integrations
1027. Future-facing Kubernetes API evolution
