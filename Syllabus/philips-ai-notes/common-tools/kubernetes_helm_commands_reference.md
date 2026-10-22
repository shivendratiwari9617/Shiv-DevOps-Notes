# Kubernetes and Helm Commands Reference

## `kubectl` Basics

### Cluster info
```bash
# This command shows the client and server Kubernetes versions in short format, and it is used to quickly verify version compatibility.
kubectl version --short
# This command displays the cluster control plane and core service endpoints, and it is used to verify cluster connectivity.
kubectl cluster-info
# This command prints the merged kubeconfig configuration, and it is used to inspect clusters, users, and contexts.
kubectl config view
# This command shows the active kubeconfig context, and it is used to confirm which cluster and user you are operating against.
kubectl config current-context
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
# This command lists all namespaces in the cluster, and it is used to view logical workload isolation boundaries.
kubectl get namespaces
```

### Contexts and namespaces
```bash
# This command lists all available kubeconfig contexts, and it is used to view accessible cluster environments.
kubectl config get-contexts
# This command switches the active kubeconfig context, and it is used to target a different cluster or user.
kubectl config use-context my-cluster
# This command sets the default namespace for the current context, and it is used to avoid repeating the namespace flag.
kubectl config set-context --current --namespace=dev
# This command creates a new namespace, and it is used to logically isolate applications or teams.
kubectl create namespace dev
# This command lists namespaces using the short resource alias, and it is used for quick namespace checks.
kubectl get ns
# This command deletes a namespace and its resources, and it is used to remove an isolated environment.
kubectl delete ns dev
```

## Resource Discovery

### Get resources
```bash
# This command lists common workload and service resources in the current namespace, and it is used for a quick environment overview.
kubectl get all
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -n dev
# This command lists deployments, and it is used to inspect application rollout objects.
kubectl get deployments
# This command lists services, and it is used to inspect service exposure and cluster networking.
kubectl get svc
# This command lists ingress resources, and it is used to check external HTTP routing configuration.
kubectl get ingress
# This command lists ConfigMaps, and it is used to inspect non-sensitive application configuration.
kubectl get configmaps
# This command lists secrets, and it is used to inspect sensitive configuration objects.
kubectl get secrets
# This command lists persistent volume claims, and it is used to check requested storage resources.
kubectl get pvc
# This command lists persistent volumes, and it is used to inspect cluster storage allocations.
kubectl get pv
# This command lists events ordered by creation time, and it is used to troubleshoot recent cluster activity.
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Wide and output formats
```bash
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -o wide
# This command retrieves a specific pod resource, and it is used to inspect full object data in YAML or JSON format.
kubectl get pod mypod -o yaml
# This command retrieves a specific deployment resource, and it is used to inspect deployment configuration in machine-readable output.
kubectl get deploy myapp -o json
# This command displays Kubernetes resource field documentation, and it is used to understand manifest schema and valid spec fields.
kubectl explain deployment
# This command displays Kubernetes resource field documentation, and it is used to understand manifest schema and valid spec fields.
kubectl explain deployment.spec.template.spec.containers
```

## Creating and Applying Resources

### Apply from files
```bash
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f deployment.yaml
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f k8s/
# This command applies resources from a Kustomize directory, and it is used to deploy customized manifest overlays.
kubectl apply -k overlays/dev
# This command deletes resources defined in a manifest file, and it is used to remove previously applied objects.
kubectl delete -f deployment.yaml
```

### Imperative creation
```bash
# This command creates a deployment imperatively, and it is used to quickly launch a stateless application.
kubectl create deployment nginx --image=nginx
# This command creates a service for a deployment, and it is used to expose application pods on a stable endpoint.
kubectl expose deployment nginx --port=80 --target-port=80 --type=ClusterIP
# This command creates a pod imperatively, and it is used for temporary workloads, tests, and debugging.
kubectl run temp-shell --image=busybox --restart=Never -- sleep 3600
# This command creates a ConfigMap, and it is used to store non-sensitive key-value or file-based configuration.
kubectl create configmap app-config --from-literal=ENV=prod
# This command creates a generic secret, and it is used to store sensitive values such as credentials.
kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=secret
# This command creates a Job, and it is used to run one-time batch workloads.
kubectl create job pi --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'
# This command creates a CronJob, and it is used to schedule recurring batch workloads.
kubectl create cronjob hello --image=busybox --schedule="*/5 * * * *" -- echo hello
```

## Deployments

### Common deployment workflow
```bash
# This command creates a deployment imperatively, and it is used to quickly launch a stateless application.
kubectl create deployment web --image=nginx:1.25
# This command changes the number of deployment replicas, and it is used to scale application capacity manually.
kubectl scale deployment web --replicas=3
# This command updates a container image in a workload, and it is used to trigger a rollout to a new application version.
kubectl set image deployment/web nginx=nginx:1.26
# This command watches rollout progress, and it is used to confirm whether an update completes successfully.
kubectl rollout status deployment/web
# This command shows rollout revision history, and it is used to inspect previous deployment changes.
kubectl rollout history deployment/web
# This command rolls back a workload to a previous revision, and it is used to recover from a failed rollout.
kubectl rollout undo deployment/web
# This command rolls back a workload to a previous revision, and it is used to recover from a failed rollout.
kubectl rollout undo deployment/web --to-revision=2
# This command opens a deployment in an editor, and it is used to make direct live changes to the resource.
kubectl edit deployment web
# This command applies a partial update to a deployment, and it is used for quick targeted spec changes.
kubectl patch deployment web -p '{"spec":{"replicas":5}}'
```

### Restart and pause
```bash
# This command restarts pods managed by a workload, and it is used to refresh running containers without changing the image.
kubectl rollout restart deployment/web
# This command pauses an ongoing deployment rollout, and it is used to temporarily stop update progression.
kubectl rollout pause deployment/web
# This command resumes a paused deployment rollout, and it is used to continue a previously stopped update.
kubectl rollout resume deployment/web
```

## Pods

### Inspect pods
```bash
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods
# This command shows detailed pod information and recent events, and it is used for troubleshooting scheduling and runtime issues.
kubectl describe pod mypod
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs mypod
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs -f mypod
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs mypod -c app
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs deployment/web
# This command shows live CPU and memory usage for pods, and it is used for basic resource troubleshooting.
kubectl top pod
# This command shows live CPU and memory usage for pods, and it is used for basic resource troubleshooting.
kubectl top pod -A
```

### Exec into containers
```bash
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- /bin/sh
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -c app -- /bin/bash
# This command copies files between a local machine and a container, and it is used to collect logs or transfer files.
kubectl cp mypod:/tmp/app.log ./app.log
# This command copies files between a local machine and a container, and it is used to collect logs or transfer files.
kubectl cp ./localfile mypod:/tmp/localfile
```

### Delete pods
```bash
# This command deletes a pod, and it is used to remove a workload instance or force recreation.
kubectl delete pod mypod
# This command deletes a pod, and it is used to remove a workload instance or force recreation.
kubectl delete pod mypod --force --grace-period=0
# This command deletes a pod, and it is used to remove a workload instance or force recreation.
kubectl delete pods --all -n dev
```

## Troubleshooting

### Describe and events
```bash
# This command shows detailed pod information and recent events, and it is used for troubleshooting scheduling and runtime issues.
kubectl describe pod mypod
# This command shows detailed deployment information and events, and it is used to debug rollout and replica issues.
kubectl describe deployment web
# This command shows detailed node information, and it is used to inspect capacity, conditions, taints, and assigned pods.
kubectl describe node worker-1
# This command lists cluster events, and it is used to identify failures, warnings, and recent resource actions.
kubectl get events --sort-by=.lastTimestamp
# This command lists cluster events, and it is used to identify failures, warnings, and recent resource actions.
kubectl get events -A
```

### Debugging
```bash
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs mypod --previous
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- env
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- printenv
# This command forwards a local port directly to a pod, and it is used for targeted debugging of a single workload instance.
kubectl port-forward pod/mypod 8080:80
# This command forwards a local port to a service, and it is used to test service access without external exposure.
kubectl port-forward svc/web 8080:80
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i create deployments
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i list secrets -n dev
```

### Ephemeral/debug containers
```bash
# This command adds a debug container to a pod, and it is used for advanced troubleshooting of running workloads.
kubectl debug pod/mypod -it --image=busybox
# This command starts a debug session on a node, and it is used for low-level host troubleshooting.
kubectl debug node/worker-1 -it --image=ubuntu
```

## Services and Networking

### Services
```bash
# This command lists services, and it is used to inspect service exposure and cluster networking.
kubectl get svc
# This command shows detailed service information, and it is used to verify ports, selectors, and endpoints.
kubectl describe svc web
# This command creates a service for a pod, and it is used to expose a standalone pod on the network.
kubectl expose pod mypod --port=80 --target-port=8080 --type=ClusterIP
# This command creates a service for a deployment, and it is used to expose application pods on a stable endpoint.
kubectl expose deployment web --port=80 --type=LoadBalancer
# This command deletes a service, and it is used to remove stable network exposure for a workload.
kubectl delete svc web
```

### DNS and connectivity tests
```bash
# This command creates a pod imperatively, and it is used for temporary workloads, tests, and debugging.
kubectl run dns-test --image=busybox:1.36 --restart=Never -it -- nslookup kubernetes.default
# This command creates a pod imperatively, and it is used for temporary workloads, tests, and debugging.
kubectl run curl-test --image=curlimages/curl --restart=Never -it -- sh
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it dns-test -- nslookup web.default.svc.cluster.local
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it curl-test -- curl http://web
```

### Ingress
```bash
# This command lists ingress resources, and it is used to check external HTTP routing configuration.
kubectl get ingress
# This command shows detailed ingress information, and it is used to inspect routing rules and backend mappings.
kubectl describe ingress web-ing
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f ingress.yaml
# This command deletes an ingress resource, and it is used to remove external HTTP routing.
kubectl delete ingress web-ing
```

### Network policies
```bash
# This command lists network policies, and it is used to inspect traffic control rules between pods.
kubectl get networkpolicy
# This command shows detailed network policy rules, and it is used to verify allowed and denied traffic behavior.
kubectl describe networkpolicy deny-all
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f networkpolicy.yaml
```

## Storage

### Volumes, PV, PVC
```bash
# This command lists persistent volumes, and it is used to inspect cluster storage allocations.
kubectl get pv
# This command lists persistent volume claims, and it is used to check requested storage resources.
kubectl get pvc
# This command lists storage classes, and it is used to inspect dynamic provisioning options.
kubectl get storageclass
# This command shows detailed persistent volume information, and it is used to troubleshoot bound storage resources.
kubectl describe pv pv001
# This command shows detailed persistent volume information, and it is used to troubleshoot bound storage resources.
kubectl describe pvc data-claim
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f pvc.yaml
# This command runs `kubectl delete pvc data-claim`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl delete pvc data-claim
# This command applies a partial update to a PVC, and it is used for targeted storage configuration changes.
kubectl patch pvc data-claim -p '{"metadata":{"annotations":{"resize":"true"}}}'
```

### Mount inspection
```bash
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- df -h
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- mount
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- ls /data
```

## ConfigMaps and Secrets

### ConfigMaps
```bash
# This command lists ConfigMaps, and it is used to inspect non-sensitive application configuration.
kubectl get configmaps
# This command runs `kubectl describe configmap app-config`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl describe configmap app-config
# This command creates a ConfigMap, and it is used to store non-sensitive key-value or file-based configuration.
kubectl create configmap app-config --from-file=app.properties
# This command creates a ConfigMap, and it is used to store non-sensitive key-value or file-based configuration.
kubectl create configmap app-config --from-env-file=.env
# This command edits a ConfigMap live, and it is used to modify application configuration in place.
kubectl edit configmap app-config
# This command deletes a ConfigMap, and it is used to remove stored configuration.
kubectl delete configmap app-config
```

### Secrets
```bash
# This command lists secrets, and it is used to inspect sensitive configuration objects.
kubectl get secrets
# This command shows secret metadata, and it is used to inspect secret references and type information.
kubectl describe secret db-secret
# This command creates a generic secret, and it is used to store sensitive values such as credentials.
kubectl create secret generic db-secret --from-literal=password=secret
# This command creates a TLS secret, and it is used to store certificates and private keys for ingress or apps.
kubectl create secret tls web-tls --cert=tls.crt --key=tls.key
# This command deletes a secret, and it is used to remove sensitive configuration objects.
kubectl delete secret db-secret
# This command retrieves secret data in YAML output, and it is used to inspect encoded secret content and metadata.
kubectl get secret db-secret -o yaml
```

## Scaling and Autoscaling

### Manual scaling
```bash
# This command changes the number of deployment replicas, and it is used to scale application capacity manually.
kubectl scale deployment web --replicas=5
# This command changes the replica count of a StatefulSet, and it is used to scale stateful workloads.
kubectl scale statefulset db --replicas=3
```

### Autoscaling
```bash
# This command creates a HorizontalPodAutoscaler, and it is used to scale pods automatically based on CPU usage.
kubectl autoscale deployment web --cpu-percent=70 --min=2 --max=10
# This command lists HorizontalPodAutoscalers, and it is used to inspect autoscaling policies and targets.
kubectl get hpa
# This command shows detailed HPA information, and it is used to troubleshoot autoscaling behavior.
kubectl describe hpa web
# This command deletes a HorizontalPodAutoscaler, and it is used to remove automatic scaling control.
kubectl delete hpa web
```

## Stateful and Background Workloads

### StatefulSets
```bash
# This command lists StatefulSets, and it is used to inspect stateful application controllers.
kubectl get statefulsets
# This command shows detailed StatefulSet information, and it is used to debug ordered rollout and storage behavior.
kubectl describe statefulset db
# This command watches rollout progress, and it is used to confirm whether an update completes successfully.
kubectl rollout status statefulset/db
# This command changes the replica count of a StatefulSet, and it is used to scale stateful workloads.
kubectl scale statefulset db --replicas=3
```

### DaemonSets
```bash
# This command lists DaemonSets, and it is used to inspect workloads running one pod per node.
kubectl get daemonsets
# This command shows detailed DaemonSet information, and it is used to troubleshoot node-wide agent deployments.
kubectl describe daemonset fluentd
# This command watches rollout progress, and it is used to confirm whether an update completes successfully.
kubectl rollout status daemonset/fluentd
```

### Jobs and CronJobs
```bash
# This command lists Jobs, and it is used to inspect one-time batch workloads.
kubectl get jobs
# This command shows detailed Job information, and it is used to troubleshoot batch execution.
kubectl describe job pi
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs job/pi
# This command deletes a Job, and it is used to remove completed or failed batch resources.
kubectl delete job pi

# This command lists CronJobs, and it is used to inspect scheduled batch workloads.
kubectl get cronjobs
# This command shows detailed CronJob information, and it is used to verify schedules and job history.
kubectl describe cronjob hello
# This command creates a Job, and it is used to run one-time batch workloads.
kubectl create job --from=cronjob/hello hello-manual
# This command deletes a CronJob, and it is used to stop future scheduled executions.
kubectl delete cronjob hello
```

## RBAC and Security

### Service accounts and RBAC
```bash
# This command lists service accounts, and it is used to inspect pod identities in a namespace.
kubectl get serviceaccounts
# This command creates a service account, and it is used to assign an identity to pods.
kubectl create serviceaccount app-sa
# This command lists Roles, and it is used to inspect namespace-scoped RBAC permissions.
kubectl get roles
# This command lists RoleBindings, and it is used to inspect RBAC role assignments in a namespace.
kubectl get rolebindings
# This command lists ClusterRoles, and it is used to inspect cluster-wide RBAC permission sets.
kubectl get clusterroles
# This command lists ClusterRoleBindings, and it is used to inspect cluster-wide RBAC assignments.
kubectl get clusterrolebindings
# This command shows detailed Role rules, and it is used to inspect namespace-scoped permissions.
kubectl describe role pod-reader
# This command shows detailed Role rules, and it is used to inspect namespace-scoped permissions.
kubectl describe rolebinding read-pods
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f role.yaml
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f rolebinding.yaml
```

### Security inspection
```bash
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i get pods --as=system:serviceaccount:default:app-sa
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i list secrets --as=system:serviceaccount:default:app-sa -n default
```

## Node Operations

### Node inspection
```bash
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
# This command shows detailed node information, and it is used to inspect capacity, conditions, taints, and assigned pods.
kubectl describe node worker-1
# This command runs `kubectl top nodes`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl top nodes
```

### Scheduling control
```bash
# This command marks a node unschedulable, and it is used to stop new pods from being placed during maintenance.
kubectl cordon worker-1
# This command marks a node schedulable again, and it is used to return the node to normal workload placement.
kubectl uncordon worker-1
# This command evicts non-daemonset pods from a node, and it is used to safely prepare a node for maintenance or upgrade.
kubectl drain worker-1 --ignore-daemonsets --delete-emptydir-data
```

### Taints and labels
```bash
# This command adds or updates node labels, and it is used to influence workload scheduling.
kubectl label nodes worker-1 disktype=ssd
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes --show-labels
# This command adds or removes node taints, and it is used to restrict which pods can be scheduled onto a node.
kubectl taint nodes worker-1 dedicated=apps:NoSchedule
# This command adds or removes node taints, and it is used to restrict which pods can be scheduled onto a node.
kubectl taint nodes worker-1 dedicated=apps:NoSchedule-
```

## Resource Cleanup

```bash
# This command deletes a pod, and it is used to remove a workload instance or force recreation.
kubectl delete pod mypod
# This command deletes a deployment, and it is used to remove an application controller.
kubectl delete deployment web
# This command deletes a service, and it is used to remove stable network exposure for a workload.
kubectl delete svc web
# This command deletes an ingress resource, and it is used to remove external HTTP routing.
kubectl delete ingress web-ing
# This command deletes a ConfigMap, and it is used to remove stored configuration.
kubectl delete configmap app-config
# This command deletes a secret, and it is used to remove sensitive configuration objects.
kubectl delete secret db-secret
# This command runs `kubectl delete pvc data-claim`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl delete pvc data-claim
# This command deletes a namespace and all its resources, and it is used for environment cleanup.
kubectl delete namespace dev
# This command deletes common workload and service resources in a namespace, and it is used for bulk cleanup.
kubectl delete all --all -n dev
```

## Useful Selectors and Filters

```bash
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -l app=web
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -l 'app in (web,api)'
# This command deletes a pod, and it is used to remove a workload instance or force recreation.
kubectl delete pods -l app=web
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs -l app=web --all-containers=true
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods --field-selector=status.phase=Running
# This command lists cluster events, and it is used to identify failures, warnings, and recent resource actions.
kubectl get events --field-selector involvedObject.name=mypod
```

## Output Formatting

```bash
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -o name
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -o custom-columns=NAME:.metadata.name,IMAGE:.spec.containers[*].image
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
# This command retrieves a specific pod resource, and it is used to inspect full object data in YAML or JSON format.
kubectl get pod mypod -o go-template='{{.metadata.name}}'
```

## Helm Basics

### Helm environment
```bash
# This command shows the Helm client version, and it is used to verify Helm installation and release tooling compatibility.
helm version
# This command prints Helm environment settings, and it is used to inspect cache, config, and data paths.
helm env
# This command lists Helm releases, and it is used to inspect installed charts in a namespace or cluster.
helm list
# This command lists Helm releases, and it is used to inspect installed charts in a namespace or cluster.
helm list -A
# This command lists configured Helm repositories, and it is used to inspect available chart sources.
helm repo list
```

### Repositories
```bash
# This command adds a chart repository, and it is used to make remote Helm charts available for installation.
helm repo add bitnami https://charts.bitnami.com/bitnami
# This command refreshes repository metadata, and it is used to pull the latest chart index information.
helm repo update
# This command removes a configured repository, and it is used to clean up unused chart sources.
helm repo remove bitnami
# This command searches configured Helm repositories, and it is used to find available charts locally.
helm search repo nginx
# This command searches public Helm chart listings, and it is used to discover charts outside local repos.
helm search hub nginx
```

## Helm Chart Discovery and Inspection

```bash
# This command displays chart metadata, and it is used to inspect chart name, version, and dependencies.
helm show chart bitnami/nginx
# This command prints default chart values, and it is used to prepare custom values overrides.
helm show values bitnami/nginx
# This command displays the chart README, and it is used to understand chart usage and configuration.
helm show readme bitnami/nginx
# This command downloads a chart package, and it is used to inspect or install charts locally.
helm pull bitnami/nginx
# This command downloads a chart package, and it is used to inspect or install charts locally.
helm pull bitnami/nginx --untar
# This command renders chart templates locally, and it is used to preview Kubernetes manifests before installation.
helm template mynginx bitnami/nginx
# This command validates chart structure and templates, and it is used to catch chart issues before deployment.
helm lint ./mychart
```

## Helm Install and Upgrade

### Install releases
```bash
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx -n web --create-namespace
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx -f values.yaml
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx --set service.type=LoadBalancer
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx --set image.tag=1.25.0
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx ./mychart
```

### Upgrade releases
```bash
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx -f values.yaml
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx --set replicaCount=3
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade --install mynginx bitnami/nginx -n web --create-namespace
```

## Helm Release Management

```bash
# This command lists Helm releases, and it is used to inspect installed charts in a namespace or cluster.
helm list
# This command lists Helm releases, and it is used to inspect installed charts in a namespace or cluster.
helm list -n web
# This command shows release status, and it is used to inspect deployment state and notes.
helm status mynginx
# This command lists release revisions, and it is used to review release change history.
helm history mynginx
# This command fetches all release details, and it is used to inspect stored manifests, values, and metadata.
helm get all mynginx
# This command retrieves release values, and it is used to inspect effective chart configuration.
helm get values mynginx
# This command retrieves release values, and it is used to inspect effective chart configuration.
helm get values mynginx -a
# This command prints the rendered manifest for a release, and it is used to inspect deployed Kubernetes objects.
helm get manifest mynginx
# This command rolls a release back to a previous revision, and it is used to recover from a failed upgrade.
helm rollback mynginx 1
# This command removes a Helm release, and it is used to delete deployed chart resources.
helm uninstall mynginx
# This command removes a Helm release, and it is used to delete deployed chart resources.
helm uninstall mynginx -n web
```

## Helm Templating and Dry Runs

```bash
# This command renders chart templates locally, and it is used to preview Kubernetes manifests before installation.
helm template mynginx bitnami/nginx
# This command renders chart templates locally, and it is used to preview Kubernetes manifests before installation.
helm template mynginx ./mychart -f values.yaml
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx --dry-run --debug
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx --dry-run --debug
# This command validates chart structure and templates, and it is used to catch chart issues before deployment.
helm lint bitnami/nginx
```

## Helm Values Workflows

```bash
# This command prints default chart values, and it is used to prepare custom values overrides.
helm show values bitnami/nginx > values-default.yaml
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx -f values-prod.yaml
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx -f values-prod.yaml -f values-secrets.yaml
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx --set replicaCount=4 --set service.type=ClusterIP
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx --set-string image.tag=latest
```

## Helm Chart Development

```bash
# This command creates a new chart skeleton, and it is used to start Helm chart development.
helm create mychart
# This command validates chart structure and templates, and it is used to catch chart issues before deployment.
helm lint ./mychart
# This command renders chart templates locally, and it is used to preview Kubernetes manifests before installation.
helm template test ./mychart
# This command packages a chart directory into a distributable archive, and it is used for chart publishing and release.
helm package ./mychart
# This command lists chart dependencies, and it is used to inspect external chart requirements.
helm dependency list ./mychart
# This command downloads and updates chart dependencies, and it is used to sync required subcharts.
helm dependency update ./mychart
# This command rebuilds chart dependencies from the lock file, and it is used for reproducible dependency setup.
helm dependency build ./mychart
```

## Helm Troubleshooting

```bash
# This command shows release status, and it is used to inspect deployment state and notes.
helm status mynginx
# This command lists release revisions, and it is used to review release change history.
helm history mynginx
# This command prints the rendered manifest for a release, and it is used to inspect deployed Kubernetes objects.
helm get manifest mynginx
# This command shows chart post-install notes, and it is used to review access instructions and release guidance.
helm get notes mynginx
# This command lists chart hooks, and it is used to inspect lifecycle actions executed during release operations.
helm get hooks mynginx
# This command upgrades an existing Helm release, and it is used to apply chart or values changes.
helm upgrade mynginx bitnami/nginx --debug --dry-run
# This command lists common workload and service resources in the current namespace, and it is used for a quick environment overview.
kubectl get all -n web
# This command shows detailed pod information and recent events, and it is used for troubleshooting scheduling and runtime issues.
kubectl describe pod -n web <pod-name>
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs -n web <pod-name>
```

## Common End-to-End Workflows

### Deploy an app with `kubectl`
```bash
# This command creates a new namespace, and it is used to logically isolate applications or teams.
kubectl create namespace demo
# This command creates a deployment imperatively, and it is used to quickly launch a stateless application.
kubectl create deployment web --image=nginx -n demo
# This command creates a service for a deployment, and it is used to expose application pods on a stable endpoint.
kubectl expose deployment web --port=80 --type=ClusterIP -n demo
# This command changes the number of deployment replicas, and it is used to scale application capacity manually.
kubectl scale deployment web --replicas=3 -n demo
# This command watches rollout progress, and it is used to confirm whether an update completes successfully.
kubectl rollout status deployment/web -n demo
# This command lists common workload and service resources in the current namespace, and it is used for a quick environment overview.
kubectl get all -n demo
```

### Deploy an app with Helm
```bash
# This command adds a chart repository, and it is used to make remote Helm charts available for installation.
helm repo add bitnami https://charts.bitnami.com/bitnami
# This command refreshes repository metadata, and it is used to pull the latest chart index information.
helm repo update
# This command installs a Helm release, and it is used to deploy an application from a chart.
helm install mynginx bitnami/nginx -n demo --create-namespace
# This command shows release status, and it is used to inspect deployment state and notes.
helm status mynginx -n demo
# This command lists common workload and service resources in the current namespace, and it is used for a quick environment overview.
kubectl get all -n demo
```

### Update a deployment image
```bash
# This command updates a container image in a workload, and it is used to trigger a rollout to a new application version.
kubectl set image deployment/web nginx=nginx:1.26 -n demo
# This command watches rollout progress, and it is used to confirm whether an update completes successfully.
kubectl rollout status deployment/web -n demo
# This command shows rollout revision history, and it is used to inspect previous deployment changes.
kubectl rollout history deployment/web -n demo
```

### Roll back a failed rollout
```bash
# This command rolls back a workload to a previous revision, and it is used to recover from a failed rollout.
kubectl rollout undo deployment/web -n demo
# This command rolls a release back to a previous revision, and it is used to recover from a failed upgrade.
helm rollback mynginx 1 -n demo
```

### Investigate a crashing pod
```bash
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -n demo
# This command shows detailed pod information and recent events, and it is used for troubleshooting scheduling and runtime issues.
kubectl describe pod <pod-name> -n demo
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs <pod-name> -n demo
# This command prints container logs, and it is used to diagnose application startup, runtime, and crash problems.
kubectl logs <pod-name> --previous -n demo
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it <pod-name> -n demo -- /bin/sh
# This command lists cluster events, and it is used to identify failures, warnings, and recent resource actions.
kubectl get events -n demo --sort-by=.lastTimestamp
```

### Check storage issues
```bash
# This command lists persistent volume claims, and it is used to check requested storage resources.
kubectl get pvc -n demo
# This command shows detailed persistent volume information, and it is used to troubleshoot bound storage resources.
kubectl describe pvc data-claim -n demo
# This command lists persistent volumes, and it is used to inspect cluster storage allocations.
kubectl get pv
# This command shows detailed persistent volume information, and it is used to troubleshoot bound storage resources.
kubectl describe pv <pv-name>
# This command lists storage classes, and it is used to inspect dynamic provisioning options.
kubectl get storageclass
```

### Test service networking
```bash
# This command lists services, and it is used to inspect service exposure and cluster networking.
kubectl get svc -n demo
# This command shows detailed service information, and it is used to verify ports, selectors, and endpoints.
kubectl describe svc web -n demo
# This command creates a pod imperatively, and it is used for temporary workloads, tests, and debugging.
kubectl run curl-test --image=curlimages/curl --restart=Never -it -n demo -- sh
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it curl-test -n demo -- curl http://web
# This command lists service endpoints, and it is used to verify which pod IPs back a service.
kubectl get endpoints -n demo
```

### Clean up everything
```bash
# This command removes a Helm release, and it is used to delete deployed chart resources.
helm uninstall mynginx -n demo
# This command deletes a deployment, and it is used to remove an application controller.
kubectl delete deployment web -n demo
# This command deletes a service, and it is used to remove stable network exposure for a workload.
kubectl delete svc web -n demo
# This command deletes a namespace and all its resources, and it is used for environment cleanup.
kubectl delete namespace demo
```


## Cluster Upgrade and Maintenance

### kubeadm upgrade planning
Used to inspect current cluster version state and plan a safe control plane upgrade path before making changes.

```bash
# This command shows the client and server Kubernetes versions in short format, and it is used to quickly verify version compatibility.
kubectl version --short
# This command shows the kubeadm version, and it is used to verify upgrade tool version compatibility.
kubeadm version
# This command checks available upgrade targets and preflight readiness, and it is used to plan a safe kubeadm cluster upgrade.
kubeadm upgrade plan
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A
```

### Upgrade control plane node
Used to upgrade Kubernetes components on the control plane node in the correct sequence with kubeadm-managed clusters.

```bash
# This command unlocks the kubeadm package, and it is used before upgrading to a new Kubernetes version.
sudo apt-mark unhold kubeadm && sudo apt-get update
# This command installs a target kubeadm version, and it is used as part of the node or control plane upgrade process.
sudo apt-get install -y kubeadm='1.30.x-*'
# This command locks the kubeadm package version, and it is used to prevent unintended upgrades.
sudo apt-mark hold kubeadm

# This command runs `sudo kubeadm upgrade plan`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
sudo kubeadm upgrade plan
# This command upgrades the control plane to a target Kubernetes version, and it is used during kubeadm-managed control plane upgrades.
sudo kubeadm upgrade apply v1.30.x

# This command unlocks kubelet and kubectl packages, and it is used before upgrading node components.
sudo apt-mark unhold kubelet kubectl && sudo apt-get update
# This command installs target kubelet and kubectl versions, and it is used to align node binaries with the cluster version.
sudo apt-get install -y kubelet='1.30.x-*' kubectl='1.30.x-*'
# This command locks kubelet and kubectl package versions, and it is used to keep node components stable after upgrade.
sudo apt-mark hold kubelet kubectl

# This command reloads systemd manager configuration, and it is used after package updates that affect service unit files.
sudo systemctl daemon-reload
# This command restarts the kubelet service, and it is used to apply upgraded binaries or refreshed certificates.
sudo systemctl restart kubelet
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A
```

### Upgrade worker node
Used to safely drain, upgrade, and return a worker node to service without disrupting cluster consistency.

```bash
# This command evicts non-daemonset pods from a node, and it is used to safely prepare a node for maintenance or upgrade.
kubectl drain <worker-node> --ignore-daemonsets --delete-emptydir-data

# This command unlocks the kubeadm package, and it is used before upgrading to a new Kubernetes version.
sudo apt-mark unhold kubeadm && sudo apt-get update
# This command installs a target kubeadm version, and it is used as part of the node or control plane upgrade process.
sudo apt-get install -y kubeadm='1.30.x-*'
# This command locks the kubeadm package version, and it is used to prevent unintended upgrades.
sudo apt-mark hold kubeadm

# This command upgrades node-specific kubeadm state, and it is used when updating worker nodes after draining.
sudo kubeadm upgrade node

# This command unlocks kubelet and kubectl packages, and it is used before upgrading node components.
sudo apt-mark unhold kubelet kubectl && sudo apt-get update
# This command installs target kubelet and kubectl versions, and it is used to align node binaries with the cluster version.
sudo apt-get install -y kubelet='1.30.x-*' kubectl='1.30.x-*'
# This command locks kubelet and kubectl package versions, and it is used to keep node components stable after upgrade.
sudo apt-mark hold kubelet kubectl

# This command reloads systemd manager configuration, and it is used after package updates that affect service unit files.
sudo systemctl daemon-reload
# This command restarts the kubelet service, and it is used to apply upgraded binaries or refreshed certificates.
sudo systemctl restart kubelet

# This command marks a node schedulable again, and it is used to return the node to normal workload placement.
kubectl uncordon <worker-node>
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
```

### Post-upgrade validation
Used to confirm cluster health, node readiness, workload recovery, and version alignment after an upgrade.

```bash
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A
# This command lists component statuses on older clusters, and it is used for a basic legacy control plane health check.
kubectl get cs
# This command shows the client and server Kubernetes versions in short format, and it is used to quickly verify version compatibility.
kubectl version --short
# This command runs `kubectl top nodes`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl top nodes
# This command shows live CPU and memory usage for pods, and it is used for basic resource troubleshooting.
kubectl top pods -A
```

## etcd Backup and Restore

### Snapshot backup
Used to create a point-in-time etcd snapshot so cluster state can be restored during disaster recovery.

```bash
# This command runs `ETCDCTL_API=3 etcdctl snapshot save snapshot.db \`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
ETCDCTL_API=3 etcdctl snapshot save snapshot.db \
# This command runs `--endpoints=https://127.0.0.1:2379 \`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
  --endpoints=https://127.0.0.1:2379 \
# This command runs `--cacert=/etc/kubernetes/pki/etcd/ca.crt \`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
# This command runs `--cert=/etc/kubernetes/pki/etcd/server.crt \`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
  --cert=/etc/kubernetes/pki/etcd/server.crt \
# This command runs `--key=/etc/kubernetes/pki/etcd/server.key`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
  --key=/etc/kubernetes/pki/etcd/server.key

# This command runs `ETCDCTL_API=3 etcdctl snapshot status snapshot.db -w table`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
ETCDCTL_API=3 etcdctl snapshot status snapshot.db -w table
```

### Snapshot restore
Used to restore Kubernetes control plane state from a previously created etcd snapshot.

```bash
# This command runs `ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \
# This command runs `--data-dir=/var/lib/etcd-restore`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
  --data-dir=/var/lib/etcd-restore

# This command stops the kubelet service, and it is used during sensitive control plane maintenance or etcd recovery.
sudo systemctl stop kubelet
# This command stops the etcd service, and it is used before restoring or replacing etcd data.
sudo systemctl stop etcd
# This command moves the existing etcd data directory, and it is used to back up current state before a restore.
sudo mv /var/lib/etcd /var/lib/etcd.bak
# This command moves the existing etcd data directory, and it is used to back up current state before a restore.
sudo mv /var/lib/etcd-restore /var/lib/etcd
# This command starts the etcd service, and it is used to bring restored cluster state back online.
sudo systemctl start etcd
# This command starts the kubelet service, and it is used to resume node agent operation after maintenance.
sudo systemctl start kubelet
```

## Certificate Management

### Check certificate expiration
Used to identify expiring Kubernetes control plane certificates before they cause authentication or API outages.

```bash
# This command checks expiration dates of kubeadm-managed certificates, and it is used to prevent certificate-related outages.
sudo kubeadm certs check-expiration
# This command shows certificate validity dates, and it is used to manually inspect control plane certificate expiration.
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -noout -dates
```

### Renew certificates
Used to renew Kubernetes certificates generated by kubeadm and restore secure component communication.

```bash
# This command renews all kubeadm-managed certificates, and it is used to restore secure communication before certificates expire.
sudo kubeadm certs renew all
# This command restarts the kubelet service, and it is used to apply upgraded binaries or refreshed certificates.
sudo systemctl restart kubelet
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A
```

### Regenerate kubeconfig files
Used to recreate kubeconfig files for control plane components after certificate or configuration updates.

```bash
# This command regenerates the admin kubeconfig file, and it is used after certificate or control plane configuration changes.
sudo kubeadm init phase kubeconfig admin
# This command regenerates the controller-manager kubeconfig file, and it is used after certificate renewal or config updates.
sudo kubeadm init phase kubeconfig controller-manager
# This command regenerates the scheduler kubeconfig file, and it is used after certificate renewal or control plane updates.
sudo kubeadm init phase kubeconfig scheduler
```

## Control Plane and Node Troubleshooting

### kubelet and system services
Used to diagnose node-level failures by checking kubelet health, runtime state, and service logs.

```bash
# This command shows kubelet service status, and it is used to diagnose node agent startup and runtime issues.
sudo systemctl status kubelet
# This command streams kubelet logs, and it is used for real-time troubleshooting of node and pod problems.
sudo journalctl -u kubelet -f
# This command lists containers known to the container runtime, and it is used to inspect runtime-level container state.
sudo crictl ps
# This command lists container images in the runtime, and it is used to verify image availability on a node.
sudo crictl images
# This command prints container logs through the CRI, and it is used when kubectl logs is not sufficient or unavailable.
sudo crictl logs <container-id>
```

### Container runtime checks
Used to troubleshoot containerd or CRI-related issues when pods fail to start or behave unexpectedly.

```bash
# This command shows containerd service status, and it is used to diagnose runtime failures.
sudo systemctl status containerd
# This command streams containerd logs, and it is used for real-time container runtime troubleshooting.
sudo journalctl -u containerd -f
# This command shows CRI runtime information, and it is used to inspect low-level runtime configuration and health.
sudo crictl info
```

### Control plane component logs
Used to investigate advanced control plane problems involving the API server, scheduler, controller manager, DNS, or etcd.

```bash
# This command lists system pods in the kube-system namespace, and it is used to inspect control plane and core add-on health.
kubectl -n kube-system get pods
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <kube-apiserver-pod>
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <kube-controller-manager-pod>
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <kube-scheduler-pod>
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <coredns-pod>
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <etcd-pod>
```

## Advanced Networking

### Endpoints and EndpointSlices
Used to verify which pod IPs a Service is routing traffic to and debug service discovery problems.

```bash
# This command lists service endpoints, and it is used to verify which pod IPs back a service.
kubectl get endpoints
# This command lists service endpoints, and it is used to verify which pod IPs back a service.
kubectl get endpointslices
# This command shows detailed endpoint information, and it is used to debug service backend registration.
kubectl describe endpoints web
# This command shows detailed endpoint information, and it is used to debug service backend registration.
kubectl describe endpointslice <name>
```

### More port-forward examples
Used to securely access in-cluster applications from a local machine for debugging, testing, or admin access.

```bash
# This command forwards a local port to a deployment pod, and it is used to access in-cluster apps from a local machine.
kubectl port-forward deployment/web 8080:80
# This command forwards a local port to a service, and it is used to test service access without external exposure.
kubectl port-forward svc/web 8443:443
# This command forwards a local port directly to a pod, and it is used for targeted debugging of a single workload instance.
kubectl port-forward pod/mypod 9000:9000 --address 0.0.0.0
```

### DNS troubleshooting
Used to debug in-cluster name resolution issues affecting service-to-service communication.

```bash
# This command lists system pods in the kube-system namespace, and it is used to inspect control plane and core add-on health.
kubectl -n kube-system get pods -l k8s-app=kube-dns
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs deployment/coredns
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it dns-test -- cat /etc/resolv.conf
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it dns-test -- nslookup kubernetes.default.svc.cluster.local
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it dns-test -- dig web.default.svc.cluster.local
```

### Network policy testing
Used to validate whether NetworkPolicies are allowing or blocking traffic as intended.

```bash
# This command creates a pod imperatively, and it is used for temporary workloads, tests, and debugging.
kubectl run test-a --image=busybox --restart=Never -- sleep 3600
# This command creates a pod imperatively, and it is used for temporary workloads, tests, and debugging.
kubectl run test-b --image=busybox --restart=Never -- sleep 3600
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it test-a -- wget -qO- http://web
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f deny-all.yaml
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it test-a -- wget -qO- http://web
# This command shows detailed network policy rules, and it is used to verify allowed and denied traffic behavior.
kubectl describe networkpolicy deny-all
```

## Advanced Storage

### StorageClass operations
Used to inspect and manage storage provisioning behavior, including default class selection.

```bash
# This command lists storage classes, and it is used to inspect dynamic provisioning options.
kubectl get storageclass
# This command runs `kubectl describe storageclass standard`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl describe storageclass standard
# This command applies a partial update to a StorageClass, and it is used to change annotations such as the default class setting.
kubectl patch storageclass standard -p '{"metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
# This command edits a StorageClass live, and it is used to modify dynamic provisioning behavior.
kubectl edit storageclass standard
```

### Volume snapshots
Used to create and inspect CSI-backed storage snapshots for backup, cloning, or restore workflows.

```bash
# This command lists VolumeSnapshot resources, and it is used to inspect storage snapshot objects.
kubectl get volumesnapshots
# This command lists VolumeSnapshotClasses, and it is used to inspect available snapshot drivers and policies.
kubectl get volumesnapshotclasses
# This command shows detailed VolumeSnapshot information, and it is used to troubleshoot snapshot creation and readiness.
kubectl describe volumesnapshot data-snap
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f volumesnapshot.yaml
```

### PVC expansion checks
Used to verify persistent volume claim resizing and confirm filesystem expansion inside running pods.

```bash
# This command lists persistent volume claims, and it is used to check requested storage resources.
kubectl get pvc
# This command shows detailed persistent volume information, and it is used to troubleshoot bound storage resources.
kubectl describe pvc data-claim
# This command runs `kubectl edit pvc data-claim`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl edit pvc data-claim
# This command opens an interactive command session inside a container, and it is used for in-container debugging.
kubectl exec -it mypod -- df -h
```

## Resource Governance

### Quotas and limits
Used to enforce namespace-level consumption controls for CPU, memory, storage, and object counts.

```bash
# This command lists ResourceQuotas, and it is used to inspect namespace resource usage constraints.
kubectl get resourcequota
# This command shows detailed ResourceQuota information, and it is used to verify quota consumption and limits.
kubectl describe resourcequota compute-quota
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f resourcequota.yaml

# This command lists LimitRanges, and it is used to inspect default and maximum container resource limits.
kubectl get limitrange
# This command shows detailed LimitRange rules, and it is used to verify enforced resource defaults and caps.
kubectl describe limitrange mem-limit-range
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f limitrange.yaml
```

### Requests and limits inspection
Used to inspect pod resource allocation and diagnose throttling, eviction, or capacity pressure issues.

```bash
# This command shows detailed pod information and recent events, and it is used for troubleshooting scheduling and runtime issues.
kubectl describe pod mypod
# This command shows live CPU and memory usage for pods, and it is used for basic resource troubleshooting.
kubectl top pod mypod
# This command runs `kubectl top node`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl top node
```

## Stateful and Availability Operations

### Pod disruption budgets
Used to protect application availability during voluntary disruptions such as node drains or upgrades.

```bash
# This command lists PodDisruptionBudgets, and it is used to inspect workload availability constraints during voluntary disruptions.
kubectl get pdb
# This command shows detailed PodDisruptionBudget information, and it is used to verify minimum availability or maximum disruption rules.
kubectl describe pdb web-pdb
# This command creates or updates resources from a manifest file, and it is used for declarative Kubernetes management.
kubectl apply -f pdb.yaml
```

### StatefulSet rollout controls
Used to manage controlled updates and rollout behavior for stateful applications with stable identities.

```bash
# This command shows rollout revision history, and it is used to inspect previous deployment changes.
kubectl rollout history statefulset/db
# This command restarts pods managed by a workload, and it is used to refresh running containers without changing the image.
kubectl rollout restart statefulset/db
# This command applies a partial update to a StatefulSet, and it is used to change rollout strategy or workload settings.
kubectl patch statefulset db -p '{"spec":{"updateStrategy":{"type":"RollingUpdate"}}}'
```

## Advanced RBAC and Security

### Impersonation
Used to test effective RBAC permissions for users or service accounts without logging in as them.

```bash
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i get pods --as=user1
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i create deployments --as=user1 -n dev
# This command checks RBAC permissions, and it is used to verify whether a user or service account can perform an action.
kubectl auth can-i list secrets --as=system:serviceaccount:dev:app-sa -n dev
```

### Pod security inspection
Used to enforce namespace-level pod security admission policies for safer workload execution.

```bash
# This command lists namespaces using the short resource alias, and it is used for quick namespace checks.
kubectl get ns --show-labels
# This command adds or updates namespace labels, and it is used to enforce policies such as pod security admission.
kubectl label ns dev pod-security.kubernetes.io/enforce=restricted --overwrite
# This command adds or updates namespace labels, and it is used to enforce policies such as pod security admission.
kubectl label ns dev pod-security.kubernetes.io/audit=restricted --overwrite
# This command adds or updates namespace labels, and it is used to enforce policies such as pod security admission.
kubectl label ns dev pod-security.kubernetes.io/warn=restricted --overwrite
```

## More Helm Commands

### Diff and dependency workflows
Used to build, package, and manage chart dependencies during Helm chart development and release preparation.

```bash
# This command lists chart dependencies, and it is used to inspect external chart requirements.
helm dependency list ./mychart
# This command downloads and updates chart dependencies, and it is used to sync required subcharts.
helm dependency update ./mychart
# This command rebuilds chart dependencies from the lock file, and it is used for reproducible dependency setup.
helm dependency build ./mychart
# This command packages a chart directory into a distributable archive, and it is used for chart publishing and release.
helm package ./mychart
```

### Namespace and release targeting
Used to inspect and manage Helm releases across namespaces in multi-environment clusters.

```bash
# This command lists Helm releases, and it is used to inspect installed charts in a namespace or cluster.
helm list -A
# This command shows release status, and it is used to inspect deployment state and notes.
helm status mynginx -n demo
# This command removes a Helm release, and it is used to delete deployed chart resources.
helm uninstall mynginx -n demo
# This command lists release revisions, and it is used to review release change history.
helm history mynginx -n demo
```

## Important Operational Workflows

### Safe node maintenance
Used to temporarily remove a node from scheduling, evict workloads safely, and return it after maintenance.

```bash
# This command marks a node unschedulable, and it is used to stop new pods from being placed during maintenance.
kubectl cordon <node>
# This command evicts non-daemonset pods from a node, and it is used to safely prepare a node for maintenance or upgrade.
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A -o wide
# This command marks a node schedulable again, and it is used to return the node to normal workload placement.
kubectl uncordon <node>
```

### Verify cluster health
Used to perform a quick operational health check across nodes, workloads, events, and live resource usage.

```bash
# This command lists all nodes in the cluster, and it is used to check node readiness, roles, and Kubernetes versions.
kubectl get nodes
# This command lists pods in the current scope, and it is used to display pod names, readiness, status, restarts, and age.
kubectl get pods -A
# This command lists cluster events, and it is used to identify failures, warnings, and recent resource actions.
kubectl get events -A --sort-by=.lastTimestamp
# This command runs `kubectl top nodes`, and it is used in Kubernetes or Helm workflows to inspect, manage, troubleshoot, or update cluster resources.
kubectl top nodes
# This command shows live CPU and memory usage for pods, and it is used for basic resource troubleshooting.
kubectl top pods -A
```

### Investigate API or scheduling issues
Used to troubleshoot pending pods, scheduler failures, and control plane decisions affecting workload placement.

```bash
# This command lists cluster events, and it is used to identify failures, warnings, and recent resource actions.
kubectl get events -A --sort-by=.lastTimestamp
# This command shows detailed pod information and recent events, and it is used for troubleshooting scheduling and runtime issues.
kubectl describe pod <pending-pod>
# This command shows detailed node information, and it is used to inspect capacity, conditions, taints, and assigned pods.
kubectl describe node <node>
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <kube-scheduler-pod>
# This command prints logs from a kube-system pod, and it is used to troubleshoot control plane or cluster add-on issues.
kubectl -n kube-system logs <kube-controller-manager-pod>
```
