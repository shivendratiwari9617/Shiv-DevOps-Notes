# Kubernetes Technical Coverage Points — Curated Master List

Curated technical coverage map from basic to advanced Kubernetes topics, with noisy and non-essential points removed.

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
29. Managed vs self-managed Kubernetes
30. Kubernetes distribution types

## 2. Cluster Architecture and Topology

31. Cluster architecture
32. Control plane vs worker nodes
33. Single-node cluster model
34. Multi-node cluster model
35. Highly available control plane
36. Stacked etcd topology
37. External etcd topology
38. Control plane endpoint
39. Control plane load balancer
40. North-south traffic model
41. East-west traffic model
42. Single-cluster design
43. Multi-cluster design
44. Regional cluster design
45. Multi-region cluster strategy
46. Private cluster design
47. Air-gapped cluster design
48. Edge cluster design
49. Hybrid cloud cluster design
50. On-premises cluster design
51. Blast radius design
52. Availability zone design
53. Control plane failure scenarios
54. Worker node failure scenarios

## 3. Cluster Bootstrapping, Installation, and PKI

55. Cluster bootstrap concepts
56. kubeadm
57. kubeadm init
58. kubeadm join
59. Bootstrap tokens
60. Discovery token CA cert hash
61. Static Pod control plane bootstrap
62. PKI directory layout
63. Admin kubeconfig
64. Controller manager kubeconfig
65. Scheduler kubeconfig
66. Kubelet kubeconfig
67. Kubeconfig structure
68. Contexts, clusters, and users in kubeconfig
69. ClusterConfiguration
70. InitConfiguration
71. JoinConfiguration
72. KubeletConfiguration
73. KubeProxyConfiguration
74. Control plane certificate upload
75. Joining additional control plane nodes
76. Certificate generation during bootstrap
77. Certificate renewal
78. Certificate rotation
79. Kubelet client certificate rotation
80. Kubelet serving certificate rotation
81. Front-proxy certificates
82. Service account signing keys
83. External CA model
84. Bootstrapping stacked etcd
85. Bootstrapping external etcd
86. Addon installation during bootstrap
88. Bare-metal installation patterns
89. Managed cluster provisioning workflows

## 4. API, Object Model, and Resource Semantics

90. Kubernetes API
91. API groups
92. API versions
93. Core API group
94. Namespaced resources
95. Cluster-scoped resources
96. Kinds
97. Resources
98. Objects
99. Metadata
100. Spec
101. Status
102. Labels
103. Equality-based label selectors
104. Set-based label selectors
105. Annotations
106. Namespaces
107. UIDs
108. OwnerReferences
109. Finalizers
110. Generation
111. ObservedGeneration
112. resourceVersion
113. CreationTimestamp
114. DeletionTimestamp
115. Managed fields
116. Server-side apply
117. Field management
118. Field ownership conflicts
119. Strategic merge patch
120. JSON merge patch
121. JSON patch
122. YAML manifest structure
123. apiVersion field
124. kind field
125. metadata field
126. spec field
127. status field
128. OpenAPI schema
129. Defaulting
130. Validation
131. Schema pruning
132. Unknown field handling
133. Subresources
134. Status subresource
135. Scale subresource
136. List operations
137. Watch operations
138. List-watch pattern
139. Field selectors
140. Graceful deletion semantics
141. Optimistic concurrency

## 5. kube-apiserver and API Machinery

142. kube-apiserver role
143. API request lifecycle
144. Authentication pipeline
145. Authorization pipeline
146. Admission chain
147. Mutating admission
148. Validating admission
149. Built-in admission plugins
150. Admission webhooks
151. Admission webhook failurePolicy
152. Admission webhook matchPolicy
153. Admission webhook sideEffects
154. Admission webhook timeout behavior
155. ValidatingAdmissionPolicy
156. CEL-based admission rules
157. Conversion webhooks
158. CRD registration
159. API aggregation layer
160. Aggregated API servers
161. Aggregated API server failure modes
162. OpenAPI publishing
163. Discovery endpoints
164. Storage version conversion
165. API deprecation policy
166. Feature gates
167. Audit logging
168. Audit policy
169. Audit backends
170. Audit stages
171. Encryption at rest
172. Encryption providers
173. KMS integration
174. KMS v2 concepts
175. API priority and fairness
176. Flow schemas
177. Priority levels
178. API priority and fairness tuning
179. Long-running requests
180. Request timeout handling
181. API server horizontal scaling
182. API server high availability
183. Front-proxy pattern
184. Request header authentication
185. Client certificate authentication
186. Bearer token authentication
187. OIDC authentication
188. Webhook token authentication
189. Anonymous authentication
190. RBAC authorization
191. Webhook authorization
192. Node authorization
193. NodeRestriction admission
194. Impersonation

## 6. etcd and State Storage

195. etcd role
196. Key-value storage model
197. Raft consensus
198. Raft leader election
199. Raft followers
200. Raft quorum
201. Write-ahead log
202. Snapshots
203. Snapshot restore
204. Cluster membership
205. Member add
206. Member remove
207. Learner members
208. Compaction
209. Defragmentation
210. Revision history
211. Watch history window
212. Consistent reads
213. Linearizable reads
214. Serializable reads
215. Lease semantics in etcd
216. Peer communication
217. Peer TLS
218. Client TLS
219. Endpoint health
220. Endpoint status
221. Alarm conditions
222. Space quota exhaustion
223. Fragmentation effects
224. IOPS sensitivity
225. Latency sensitivity
226. Data corruption scenarios
227. Backup strategy
228. Restore validation
229. Disaster recovery for etcd

## 7. Scheduler, Placement, and Eviction

230. kube-scheduler role
231. Scheduling queue
232. Active queue
233. Backoff queue
234. Unschedulable queue
235. Scheduling cycle
236. Binding cycle
237. Filter phase
238. Score phase
239. Normalize scoring
240. Reserve phase
241. Permit phase
242. PreBind phase
243. Bind phase
244. PostBind phase
245. Preemption
246. Nominated node
247. PriorityClass
248. Pod priority
249. NodeResourcesFit
250. nodeSelector
251. Node affinity
252. Required node affinity
253. Preferred node affinity
254. Pod affinity
255. Pod anti-affinity
256. Inter-pod affinity topology key
257. Topology spread constraints
258. Taints
259. Tolerations
260. Taint-based eviction
261. Extended resources
262. GPU and device-aware scheduling
263. Volume-aware scheduling
264. WaitForFirstConsumer
265. Image locality scoring
266. Node unschedulable state
267. Scheduler plugins
268. Scheduler framework
269. Scheduler profiles
270. Custom scheduler
271. Multiple schedulers
272. Queue sort plugins
273. Descheduler concepts
274. Descheduler policies
275. Unschedulable Pod backoff
276. Scheduling events

## 8. Controllers and Reconciliation

277. kube-controller-manager role
278. Shared informers
279. Lister caches
280. Work queues
281. Reconciliation model
282. Retry backoff
283. Leader election for controllers
284. Deployment controller
285. ReplicaSet controller
286. StatefulSet controller
287. DaemonSet controller
288. Job controller
289. CronJob controller
290. Namespace controller
291. Node controller
292. ServiceAccount controller
293. Endpoint controller
294. EndpointSlice controller
295. PersistentVolume controller
296. AttachDetach controller
297. Garbage collector
298. TTL-after-finished controller
299. ResourceQuota controller
300. HorizontalPodAutoscaler controller
301. CertificateSigningRequest controller
302. Disruption controller
303. Pod garbage collection
304. Controller ownership model

## 9. cloud-controller-manager and Cloud Integration

305. cloud-controller-manager role
306. Cloud-provider abstraction
307. Cloud node controller
308. Provider ID
309. Node initialization
310. Region labels
311. Zone labels
312. Cloud load balancer controller
313. Route controller
314. Cloud route programming
315. Cloud volume lifecycle hooks
316. External cloud provider model
317. Cloud failure modes
318. LoadBalancer Service provisioning
319. Node address population
320. Cloud network interface reconciliation

## 10. Node Architecture and Node Lifecycle

321. Worker node architecture
322. Node registration
323. Node object
324. Node conditions
325. Node heartbeat
326. Lease renewals
327. Node allocatable
328. Capacity vs allocatable
329. Node labels
330. Node taints
331. Cordoned nodes
332. Drained nodes
333. MemoryPressure
334. DiskPressure
335. PIDPressure
336. NetworkUnavailable
337. Ready condition
338. NotReady state
339. Unreachable state
340. Graceful node shutdown
341. Node problem detection
342. Host OS considerations
343. Kernel dependencies
344. Time synchronization
345. Swap behavior
346. cgroup driver alignment
347. System-reserved resources
348. Kube-reserved resources
349. Node replacement workflow
350. Node decommission workflow
351. Windows node considerations

## 11. kubelet Internals

352. kubelet role
353. Pod sync loop
354. Pod workers
355. Static Pods
356. Mirror Pods
357. Pod admission on node
358. Pod sandbox creation
359. Sandbox recreation
360. Container start sequence
361. Image pull workflow
362. Image pull policy
363. Image credential resolution
364. ImagePullSecrets
365. Readiness probes
366. Liveness probes
367. Startup probes
368. HTTP probes
369. TCP probes
370. Exec probes
371. gRPC probe support concepts
372. Lifecycle hooks
373. postStart hook
374. preStop hook
375. Volume mounting
376. Secret projection
377. ConfigMap projection
378. Projected volumes
379. Downward API projection
380. Pod status updates
381. Container status updates
382. Restart policy enforcement
383. Graceful termination handling
384. Eviction manager
385. Soft eviction thresholds
386. Hard eviction thresholds
387. Image garbage collection
388. Container garbage collection
389. Container log handling
390. Node status reporting
391. Device plugin registration
392. Pod resources API
393. RuntimeClass selection
394. Ephemeral container support
395. CPU Manager
396. Topology Manager
397. Memory Manager
398. Node allocatable enforcement
399. Credential provider plugins
400. Checkpoint API concepts

## 12. Container Runtime and Execution

401. Container runtime
402. CRI
403. CRI runtime service
404. CRI image service
405. PodSandbox model
406. Pause container
407. OCI image spec
408. OCI runtime spec
409. containerd
410. CRI-O
411. runc
412. Shim processes
413. gVisor
414. Kata Containers
415. Snapshotters
416. OverlayFS snapshotter
417. stargz snapshotter concepts
418. Image layer unpacking
419. Container filesystem assembly
420. Container process lifecycle
421. Runtime sandbox failure scenarios
422. Runtime isolation models
423. Rootless containers considerations
424. User namespaces in containers
425. Seccomp interaction with runtime
426. AppArmor interaction with runtime
427. SELinux interaction with runtime

## 13. Pods and Container Model

428. Pod
429. Single-container Pod
430. Multi-container Pod
431. Init containers
432. Sidecar containers
433. Ephemeral containers
434. Pod phases
435. Pod conditions
436. Pod readiness gates
437. Pod scheduling gates
438. Pod IP
439. Pod DNS
440. Pod hostname
441. Pod subdomain
442. Shared network namespace
443. Shared IPC namespace
444. Shared PID namespace
445. Shared volumes in Pod
446. Pod restart policy
447. Pod security context
448. Container security context
449. CPU requests
450. Memory requests
451. Ephemeral storage requests
452. CPU limits
453. Memory limits
454. Ephemeral storage limits
455. QoS classes
456. Guaranteed QoS
457. Burstable QoS
458. BestEffort QoS
459. Pod disruption behavior
460. TerminationGracePeriodSeconds
461. Pod immutability rules
462. Pod debugging workflow
463. Pod overhead
464. Host aliases
465. DNS policy
466. dnsConfig

## 14. Workload Controllers

467. ReplicationController
468. ReplicaSet
469. Deployment
470. Deployment rollout
471. RollingUpdate strategy
472. Recreate strategy
473. maxUnavailable
474. maxSurge
475. Rollback
476. Revision history
477. Paused rollout
478. Progress deadline
479. StatefulSet
480. OrderedReady policy
481. Parallel Pod management policy
482. Stable network identity
483. Stable storage identity
484. PersistentVolumeClaim templates
485. Headless Service for StatefulSet
486. StatefulSet update strategy
487. Partitioned rollout in StatefulSet
488. DaemonSet
489. Node-targeted agents
490. DaemonSet RollingUpdate
491. OnDelete update strategy
492. Job
493. Parallelism
494. Completions
495. BackoffLimit
496. ActiveDeadlineSeconds
497. PodFailurePolicy
498. SuccessPolicy concepts
499. Indexed Jobs
500. CronJob
501. Cron schedule syntax
502. ConcurrencyPolicy
503. StartingDeadlineSeconds
504. Suspended CronJobs

## 15. Configuration, Secrets, and Service Accounts

505. ConfigMap
506. Secret
507. Opaque Secret
508. TLS Secret
509. dockerconfigjson Secret
510. Environment variable injection
511. envFrom
512. ConfigMap volumes
513. Secret volumes
514. Projected volumes
515. Immutable ConfigMaps
516. Immutable Secrets
517. ServiceAccount
518. Default ServiceAccount
519. ServiceAccount token projection
520. Projected service account tokens
521. BoundServiceAccountTokenVolume
522. External secret operators
523. CSI secret store patterns
524. Configuration drift
525. Hot-reload patterns
526. Checksum annotation restart pattern

## 16. Services and Discovery

527. Service
528. ClusterIP
529. Headless Service
530. NodePort
531. LoadBalancer Service
532. ExternalName Service
533. Service selectors
534. Services without selectors
535. Cluster DNS
536. CoreDNS
537. CoreDNS plugin chain
538. DNS search paths
539. Service VIP
540. Virtual IP routing
541. Session affinity
542. ExternalTrafficPolicy
543. InternalTrafficPolicy
544. HealthCheckNodePort
545. Node-local traffic handling
546. Topology-aware routing hints
547. Endpoint object
548. EndpointSlice object
549. Service discovery conventions
550. SRV records
551. DNS failure scenarios

## 17. Networking Model and Dataplane

552. Kubernetes networking model
553. Pod-to-Pod communication
554. Pod-to-Service communication
555. Service-to-Pod routing
556. Node-to-Pod communication
557. Pod CIDR
558. Service CIDR
559. Cluster DNS domain
560. CNI
561. IPAM
562. veth pair
563. Bridge networking
564. Overlay networking
565. VXLAN
566. Geneve
567. Underlay networking
568. BGP-based routing
569. Route programming
570. Encapsulation
571. MTU tuning
572. Hairpin traffic
573. HostNetwork Pods
574. hostPort
575. Dual-stack networking
576. IPv4 clusters
577. IPv6 clusters
578. Dual-stack Services
579. NodeLocal DNSCache
580. eBPF dataplane
581. kube-proxy replacement

## 18. kube-proxy, Conntrack, and Service Routing

582. kube-proxy role
583. iptables mode
584. IPVS mode
585. Service rule programming
586. ClusterIP datapath
587. NodePort datapath
588. External IP handling
589. Session affinity implementation
590. EndpointSlice consumption
591. Service update propagation
592. Topology-aware hints
593. Conntrack interaction
594. Conntrack exhaustion scenarios
595. kube-proxy-free cluster operations

## 19. Ingress, Gateway API, and Traffic Management

596. Ingress resource
597. Ingress controller
598. IngressClass
599. Default IngressClass
600. Host-based routing
601. Path-based routing
602. TLS termination
603. Wildcard hosts
604. Rewrite rules
605. Canary ingress patterns
606. Gateway API
607. GatewayClass
608. Gateway
609. Listener
610. HTTPRoute
611. TCPRoute
612. TLSRoute
613. GRPCRoute
614. UDPRoute concepts
615. ReferenceGrant
616. Traffic splitting
617. Weighted routing
618. Blue-green traffic management
619. Retry policy
620. Timeout policy
621. Mirroring traffic
622. North-south gateway patterns
623. East-west gateway patterns

## 20. Network Security and Policy

624. NetworkPolicy
625. Ingress NetworkPolicy
626. Egress NetworkPolicy
627. Default deny ingress
628. Default deny egress
629. Namespace isolation
630. Pod selector semantics
631. Namespace selector semantics
632. IPBlock rules
633. Policy enforcement by CNI
634. L3/L4 policy model
635. DNS egress policy patterns
636. NetworkPolicy limitations by CNI
637. Multi-tenant network segmentation
638. Egress gateway patterns
639. mTLS in service mesh

## 21. Storage, CSI, and Data Protection

640. Kubernetes volume model
641. emptyDir
642. hostPath
643. configMap volume
644. secret volume
645. projected volume
646. PersistentVolume
647. PersistentVolumeClaim
648. StorageClass
649. Dynamic provisioning
650. Static provisioning
651. Reclaim policy
652. Retain reclaim policy
653. Delete reclaim policy
654. Recycle policy legacy
655. Access modes
656. ReadWriteOnce
657. ReadOnlyMany
658. ReadWriteMany
659. ReadWriteOncePod
660. Filesystem volume mode
661. Block volume mode
662. Immediate binding
663. WaitForFirstConsumer binding
664. CSI
665. CSI controller plugin
666. CSI node plugin
667. CSI external provisioner
668. CSI attacher
669. CSI resizer
670. CSI snapshotter
671. CSI sidecar failure handling
672. Volume attachment
673. Mount propagation
674. Volume snapshots
675. VolumeSnapshotClass
676. Volume cloning
677. Online volume expansion
678. Offline volume expansion
679. Raw block volumes
680. Topology-aware storage
681. Stateful storage patterns
682. Attach failure scenarios
683. Mount failure scenarios
684. Backup consistency considerations

## 22. Security, Identity, and Policy Enforcement

685. RBAC
686. Role
687. ClusterRole
688. RoleBinding
689. ClusterRoleBinding
690. Aggregation rules for ClusterRoles
691. Workload identity
692. Node identity
693. Bootstrap node identity
694. Client certificate authentication
695. Certificate signer concepts
696. OIDC identity integration
697. Webhook authentication and authorization
698. Pod Security Standards
699. Pod Security Admission
700. Baseline profile
701. Restricted profile
702. Privileged profile
703. securityContext
704. runAsNonRoot
705. readOnlyRootFilesystem
706. allowPrivilegeEscalation
707. privileged containers
708. hostPID
709. hostIPC
710. hostNetwork security implications
711. Linux capabilities
712. Seccomp
713. AppArmor
714. SELinux
715. fsGroup
716. Supplemental groups
717. sysctls
718. Admission policy
719. OPA Gatekeeper
720. Kyverno
721. Secret encryption
722. KMS providers
723. Image signing concepts
724. Image provenance
725. Software bill of materials
726. Supply chain security
727. Control-plane certificate expiry recovery

## 23. Resource Governance, Isolation, and Capacity

728. ResourceQuota
729. LimitRange
730. Default requests
731. Default limits
732. Namespace capacity control
733. CPU overcommit
734. Memory overcommit
735. Ephemeral storage quotas
738. Dedicated node pools
739. GPU scheduling
740. Device plugins
741. RuntimeClass isolation
742. Taints for isolation
743. Tolerations for dedicated workloads
744. Pod overhead
745. Node allocatable reservations
746. System-reserved and kube-reserved tuning
747. Cluster capacity planning
748. Bin-packing strategy

## 24. Autoscaling and Elasticity

749. HorizontalPodAutoscaler
750. autoscaling/v2
751. CPU-based scaling
752. Memory-based scaling
753. Custom metrics scaling
754. External metrics scaling
755. Metrics API
756. Custom Metrics API
757. External Metrics API
758. metrics-server
759. Metrics adapter patterns
760. Prometheus adapter patterns
761. Scale target ref
762. Min replicas
763. Max replicas
764. Scale stabilization
765. HPA behavior policies
766. VerticalPodAutoscaler
767. VPA recommendation mode
768. VPA auto mode
769. VPA and HPA interaction
770. Cluster Autoscaler
771. Node group scaling
772. Expander strategies
773. Scale-down behavior
774. Scale-from-zero
775. KEDA
776. Event-driven autoscaling

## 25. Reliability, Availability, and Resilience

777. Self-healing
778. Replica-based availability
779. Readiness gating
780. Liveness-based recovery
781. Startup protection
782. Graceful shutdown
783. PodDisruptionBudget
784. PDB edge cases during upgrades
785. Eviction API
786. Voluntary disruptions
787. Involuntary disruptions
788. Anti-affinity for high availability
789. Topology spread for high availability
790. Multi-zone resilience
791. Control plane high availability
792. etcd quorum resilience
793. Failure domain awareness
794. SLOs
795. SLIs
796. Error budgets
797. Canary analysis
798. Chaos engineering
799. Fault injection testing

## 26. Observability, Telemetry, and Diagnostics

800. Cluster logging
801. Container stdout/stderr logging
802. Node log collection
803. Centralized logging stack
804. Metrics
805. Application metrics
806. Node metrics
807. kube-state-metrics
808. Prometheus
809. Alerting
810. Tracing
811. Distributed tracing
812. OpenTelemetry
813. Events
814. Audit logs
815. Correlation across telemetry signals
816. Debug containers
817. kubectl debug
818. CrashLoopBackOff analysis
819. ImagePullBackOff analysis
820. Pending Pod diagnosis
821. Service connectivity debugging
822. DNS troubleshooting
823. NetworkPolicy troubleshooting
824. Volume mount troubleshooting
825. Scheduler diagnostics
826. kubelet diagnostics
827. etcd diagnostics

## 27. Upgrades, Version Skew, and Lifecycle Management

828. Kubernetes version skew policy
829. Version skew between control plane and kubelet
830. Version skew between kubectl and cluster
831. Control plane upgrade strategy
832. kubeadm upgrade workflow
833. Node upgrade strategy
834. Rolling node upgrades
835. Surge node upgrades
836. In-place node upgrades
837. Drain before upgrade
838. Cordoning nodes
839. Uncordoning nodes
840. PDB-aware upgrades
841. Managed node group upgrades
842. Self-managed node upgrade workflow
843. OS patching
844. Kernel upgrade considerations
845. Container runtime upgrades
846. CNI upgrades
847. CSI upgrades
848. Addon compatibility checks
849. CoreDNS upgrades
850. kube-proxy upgrades
851. kubelet upgrades
852. API deprecation review
853. Feature gate changes across upgrades
854. Certificate renewal during lifecycle
855. Blue-green cluster upgrades
856. Canary upgrades
857. Rollback planning

## 28. Backup, Disaster Recovery, and Recovery Drills

858. Disaster recovery strategy
859. Recovery point objective
860. Recovery time objective
861. Control plane backup
862. etcd backup
863. etcd restore
864. Application state backup
865. Persistent volume backup
866. Crash-consistent backup
867. Application-consistent backup
868. Volume snapshot backup
869. Restore testing
870. Backup validation drills
871. Namespace-level restore
872. Cluster-level restore
873. Cross-region recovery
874. Multi-cluster failover
875. Backup retention policy
876. Recovery runbooks
877. Control plane rebuild
878. Control plane static Pod recovery
879. Worker node replacement
880. Secret recovery strategy
881. Certificate recovery
882. DNS recovery
883. Ingress and load balancer recovery


## 31. Custom Resources, Operators, and Extensibility

966. CustomResourceDefinition
967. Structural schemas
968. CRD versions
969. Served versions
970. Stored versions
971. Conversion strategy
972. Conversion webhook
973. Additional printer columns
974. Subresources for CRDs
975. Status subresource for CRDs
976. Scale subresource for CRDs
977. Custom controllers
978. client-go informers
979. controller-runtime
980. Operators
981. Operator reconciliation patterns
982. Finalizer patterns in operators
983. Status conditions in custom resources
984. Kubebuilder concepts
985. Operator SDK concepts

## 32. Service Mesh, Multi-Cluster, and Platform Engineering

986. Service mesh
987. Sidecar injection
988. mTLS
989. Traffic policy in service mesh
990. Service mesh operational concerns
991. eBPF service acceleration
992. Bare-metal load balancing
993. MetalLB
994. Node Feature Discovery
995. Cluster federation concepts
996. Multi-cluster policy management
997. Multi-cluster service discovery
998. Multi-cluster traffic failover
999. Multi-tenancy patterns
1000. Platform guardrails
1001. Compliance controls in Kubernetes
1002. Policy as code
1003. Workload placement governance
1004. Advanced scheduling integrations