# Week 2 — Networking Deep Dive — Candidate Answers (60 LPA+)

## Q1 \| DNS Resolution Flow \| P1 \| Production Troubleshooting

**Interview Question:**

> A production application suddenly fails to resolve a critical dependency hostname from only one environment, while the same hostname resolves correctly from laptops and other servers. How would you isolate the DNS resolution path across local resolver, search domains, cache, recursive resolver, authoritative records, and network reachability?

**Candidate Answer:**

I would first confirm the blast radius: only one environment, one dependency, and whether the failure is NXDOMAIN, SERVFAIL, timeout, or wrong answer. Then I would test from inside the failing runtime, not only from my laptop, because DNS can differ by resolver, search suffix, namespace, VPC/VNet, or sidecar. I would compare \`getent hosts\`, \`dig +trace\`, \`dig @local-resolver\`, and \`dig @recursive-resolver\` outputs, checking \`/etc/resolv.conf\`, search domains, ndots, CoreDNS or systemd-resolved cache, and any node-local DNS cache. If local lookup is fine but application fails, I would check application resolver behavior and cached connections. If the local resolver times out, I would verify UDP/TCP 53 reachability, route table, firewall, resolver endpoint, and cloud DNS forwarding. I would also compare authoritative records and private zone associations to ensure the environment is using the expected DNS view. For mitigation, I would switch the dependency to a known-good resolver path or temporary stable endpoint if safe, then validate with synthetic checks from the affected environment. Permanent fixes would include DNS path runbooks, resolver metrics, flow logs for port 53, and environment-level DNS conformance tests before production changes.

## Q2 \| DNS TTL / Cache Behavior \| P1 \| Production Troubleshooting

**Interview Question:**

> A DNS cutover was completed during a migration, but a subset of users and workloads still reach the old endpoint for several hours. How would you investigate TTL, resolver caching, negative caching, application DNS cache, and rollout safety for production traffic?

**Candidate Answer:**

I would treat this as a traffic-safety issue first, not only a DNS issue. I would measure how much traffic is still hitting the old endpoint and whether it is safe, then keep the old endpoint healthy until cached clients drain. For investigation, I would check the original TTL, whether recursive resolvers respected it, whether negative caching was involved through SOA minimum TTL, and whether any enterprise resolver or CDN DNS layer overrides TTL. I would compare answers from multiple public resolvers, corporate resolvers, cloud resolvers, and application pods. If command-line lookup is correct but traffic still reaches the old endpoint, I would check JVM DNS cache TTL, Go resolver behavior, Node.js agents, connection pools, HTTP keep-alive, service mesh DNS capture, and long-lived TCP sessions. Rollout safety should use low TTL well before cutover, weighted records or dual-write compatible backends, health-checked routing, and monitoring on both old and new endpoints. Mitigation would be to keep the old target serving or redirecting safely, avoid deleting old records too early, and gradually reduce traffic. RCA would document planned TTL reduction, cache validation, and synthetic DNS checks as mandatory migration gates.

## Q3 \| A and CNAME Records \| P2 \| Architecture

**Interview Question:**

> A service migration from VM-based endpoints to a load-balanced endpoint requires DNS changes with minimal downtime. How would you design the A, CNAME, weighted, and alias record strategy to support rollback, traffic shifting, and health-based routing?

**Candidate Answer:**

I would design DNS so it becomes a safe traffic-control layer, not a single irreversible switch. For client-facing names, I prefer a stable CNAME such as \`api.company.com\` pointing to a provider-supported alias or load-balanced DNS name. For apex domains, I would use alias records where supported instead of hard-coded A records. Behind that, I would use weighted or failover records to gradually move traffic from VM endpoints to the load-balanced endpoint, starting with a very small percentage and increasing only after error rate, latency, and target health are clean. Before migration, I would lower TTL in advance and validate that recursive resolvers are observing the new TTL. The rollback strategy is to change the weight or failover policy back to the VM target, not to edit application config or redeploy clients. Health-based routing should remove only unhealthy targets, but I would not rely on DNS health checks alone for application readiness; the load balancer health check must validate a dependency-light readiness path. I would also keep old targets warm until TTL drain completes, log traffic split by target, and add dashboards for DNS answer distribution, load balancer 5xx, P95 latency, and backend saturation. This gives controlled migration, measurable rollback, and low downtime.

## Q4 \| Internal DNS Failure \| P1 \| Production Troubleshooting

**Interview Question:**

> An internal service hostname resolves correctly from one subnet but fails from another subnet in the same cloud account or subscription. How would you debug split-horizon DNS, private hosted zones, resolver rules, VPC/VNet association, and conditional forwarding?

**Candidate Answer:**

I would compare the successful and failing subnet from the same execution context: same hostname, same resolver, same VPC/VNet, same route and security posture. First I would verify whether the failing subnet receives NXDOMAIN, SERVFAIL, timeout, or public instead of private IP. That tells me whether the issue is DNS view, forwarding, or reachability. I would inspect private hosted zone or private DNS zone association, resolver rules, conditional forwarders, inbound/outbound resolver endpoints, and whether the new subnet was added to the correct zone link. In cloud environments, split-horizon DNS often fails when a new VPC/VNet, subnet, or peering is created without the matching private zone association or DNS forwarding rule. I would also check DHCP options or custom DNS servers because two subnets can point to different resolvers. If DNS resolves but connectivity fails, I would move to route tables, NSGs/security groups, NACLs, and target firewall rules. Mitigation is to associate/link the subnet or VPC/VNet to the correct private DNS zone or forwarder and validate using synthetic DNS checks from every subnet. Long term, subnet provisioning should include DNS-zone linkage as IaC, with policy checks preventing orphaned subnets.

## Q5 \| External DNS Failure \| P1 \| Production Troubleshooting

**Interview Question:**

> A public API domain intermittently fails DNS resolution for customers in specific regions after a DNS provider change. How would you investigate authoritative name server propagation, DNSSEC, resolver behavior, geo-routing, stale glue records, and monitoring gaps?

**Candidate Answer:**

I would first separate authoritative DNS failure from recursive resolver behavior. I would query the domain through multiple regional resolvers and directly against the authoritative name servers to see whether the new provider is serving consistent records globally. I would check NS delegation at the parent zone, glue records, DS records if DNSSEC is enabled, and whether stale name servers remain in the registrar configuration. DNSSEC is a common cause after provider changes because an old DS record can make validating resolvers fail while non-validating resolvers continue working. For geo-routing, I would verify that regional policies return valid records for affected customer regions and do not accidentally return empty or unhealthy pools. I would review DNS provider change logs, propagation timeline, TTLs before the cutover, and monitoring coverage from impacted regions. Mitigation depends on cause: fix delegation or DNSSEC quickly, revert NS records if required, or disable broken geo policy while keeping a safe default record. I would validate with RIPE Atlas/public probes or regional synthetics and monitor DNS error rate, authoritative availability, and API traffic recovery. Permanent prevention includes pre-cutover DNSSEC checks, delegation validation, multi-region DNS probes, and rollback criteria for provider migrations.

## Q6 \| DNS in Application Runtime \| P2 \| Production Troubleshooting

**Interview Question:**

> An application continues calling an old service IP after DNS has changed, but command-line DNS lookup from the host shows the new IP. How would you prove whether the issue is JVM, Go, Node.js, libc, sidecar, or connection-pool DNS caching?

**Candidate Answer:**

I would prove this from the application process, not from host-level \`dig\` alone. If host lookup returns the new IP but the application calls the old IP, the likely causes are runtime DNS cache, HTTP connection pooling, service mesh cache, or long-lived connections. I would capture outbound connections using \`ss -tpn\`, \`lsof -i\`, flow logs, or sidecar access logs to confirm the destination IP and whether new connections are still being created to the old target. For JVM, I would check \`networkaddress.cache.ttl\` and framework client pools. For Go, I would verify whether it uses pure Go resolver or cgo and whether the client reuses persistent connections. For Node.js, I would inspect agent keep-alive, custom lookup functions, and library-level DNS caching. For libc/systemd-resolved, I would check nscd, dnsmasq, or node-local DNS. Mitigation may be safely restarting only affected instances, draining connection pools, or forcing client refresh without a full outage. Long term, I would define DNS cache TTL standards per runtime, instrument target IP in dependency metrics, and make migrations use dual-serving windows so stale cached clients do not break production.

## Q7 \| TCP Handshake \| P1 \| Production Troubleshooting

**Interview Question:**

> A client reports connection timeout to a production service, but the server team says the port is listening. How would you troubleshoot whether SYN packets reach the server, SYN-ACK returns, handshake completes, or packets are dropped by a firewall, route, NAT, or asymmetric path?

**Candidate Answer:**

I would not accept “port is listening” as proof of reachability. A listening socket only proves local bind state. I would trace the TCP handshake from client to server and back. From the client side, I would test DNS, route, \`nc -vz\`, and a packet capture to see SYN attempts and retransmissions. On the server or target subnet, I would capture traffic for the client IP and port to confirm whether SYN packets arrive. If SYN arrives but no SYN-ACK leaves, I would check host firewall, process backlog, kernel drops, security policy, and local routing. If SYN-ACK leaves but client never receives it, I would investigate return path, NAT state, route table, asymmetric routing, firewall statefulness, and network ACLs. If handshake completes but the application times out later, I would move up to TLS and HTTP. I would also check load balancer target health and whether client traffic reaches the expected target. For mitigation, I would route traffic through a known-good path or temporarily relax the narrowest blocking rule with approval. The permanent fix would be IaC-controlled network rules, flow-log dashboards, and a standard packet-path runbook.

## Q8 \| TCP Retransmission / Packet Loss \| P1 \| Production Troubleshooting

**Interview Question:**

> A service has intermittent latency spikes without application errors, and packet captures show retransmissions. How would you determine whether packet loss is caused by network congestion, MTU issues, load balancer behavior, host NIC saturation, or upstream dependency instability?

**Candidate Answer:**

I would correlate retransmissions with latency spikes, interface metrics, and network path changes. First, I would confirm whether retransmissions are client-side, server-side, or only between a proxy and backend using packet captures at both ends when possible. I would check TCP retransmit rate, dropped packets, NIC errors, bandwidth utilization, queue drops, CPU softirq, and load balancer target metrics. If large packets are affected, I would test MTU and path MTU discovery using DF probes and look for ICMP fragmentation-needed drops, especially across VPN, overlay, or private connectivity. If loss appears during peak traffic, I would check congestion, NAT/SNAT limits, load balancer surge queues, backend accept queues, and retry storms. I would also compare dependency latency to determine whether the upstream service is unstable and causing clients to hold connections longer. Short-term mitigation could include scaling proxy/backend capacity, reducing retry aggressiveness, shifting traffic away from a bad AZ/path, or lowering MSS where MTU is confirmed. Permanent prevention would add TCP retransmit dashboards, flow logs, packet capture triggers for P1 incidents, MTU standards, and load testing that includes network saturation and retry behavior.

## Q9 \| Connection Reset \| P1 \| Production Troubleshooting

**Interview Question:**

> Clients receive intermittent connection reset errors during peak traffic while server CPU and memory are normal. How would you isolate whether resets are generated by the application, kernel, load balancer, reverse proxy, firewall, NAT gateway, or idle timeout mismatch?

**Candidate Answer:**

I would isolate who sends the RST because that immediately narrows the fault domain. I would capture packets near the client, load balancer/proxy, and backend if possible, checking sequence numbers and source IP of the reset. If the application sends RST or closes abruptly, I would review server logs, worker crashes, connection limits, keep-alive settings, and thread/accept queue saturation. If the kernel sends resets, I would inspect backlog overflow, ephemeral port pressure, conntrack table exhaustion, and local firewall behavior. If a load balancer or reverse proxy sends resets, I would check idle timeout, max connection limits, target deregistration, connection draining, and TLS policy. During peak traffic, reset errors can also come from NAT gateways or firewalls exhausting state tables. I would correlate resets with traffic rate, connection age, request size, and upstream retry patterns. Mitigation could be aligning idle timeouts, increasing backlog or worker limits, reducing aggressive retries, and draining targets properly during deploys. The long-term fix would include connection lifecycle standards across client, proxy, LB, and app, plus dashboards for RST rate, connection count, conntrack usage, and target deregistration events.

## Q10 \| Connection Timeout \| P1 \| Production Troubleshooting

**Interview Question:**

> A deployment pipeline can connect to an artifact repository from one runner group but times out from another. How would you investigate egress routing, proxy configuration, DNS, firewall rules, NAT capacity, TLS inspection, and runner subnet differences?

**Candidate Answer:**

I would compare the two runner groups as separate network products: subnet, route table, DNS resolver, proxy environment, firewall policy, NAT gateway, and TLS inspection path. First I would run the same tests from both groups: DNS resolution, TCP connect to the artifact host, TLS handshake, proxy variables, and HTTP response. A timeout usually points to routing, firewall, NAT, or proxy path, while TLS errors point to trust store or inspection. I would check whether the failing runner subnet has a default route to NAT or proxy, whether outbound rules allow the repository IPs or domains, and whether DNS allowlists include CNAME targets. If a corporate proxy is required, I would verify \`HTTP_PROXY\`, \`HTTPS_PROXY\`, \`NO_PROXY\`, and certificate trust in the runner image. NAT capacity is also important if many builds pull artifacts simultaneously; I would inspect SNAT port usage and NAT gateway errors. Mitigation could be moving critical jobs to the healthy runner group, adding a temporary egress allow rule, or routing through the approved proxy. Permanent fixes include a standardized runner network baseline, preflight connectivity checks, artifact mirror/cache, and IaC-managed egress policies.

## Q11 \| HTTP Status Codes \| P1 \| Production Troubleshooting

**Interview Question:**

> After a release, customers see a mix of 404, 429, 500, 502, and 503 responses through the same endpoint. How would you separate routing errors, rate limiting, application failures, upstream unavailability, and reverse proxy or load balancer issues?

**Candidate Answer:**

I would split the responses by status code, route, tenant, backend, and release version instead of treating them as one incident. 404 usually suggests routing, path rewrite, missing route, or wrong backend version. 429 points to rate limit identity, burst policy, NAT-shared IPs, or bad client retry behavior. 500 is likely application error and needs app logs and traces. 502 often means proxy or load balancer could not talk correctly to upstream: connection refused, TLS mismatch, bad DNS, or upstream closed connection. 503 usually means no healthy targets, overloaded backend, or maintenance/load shedding. I would use gateway access logs with upstream status, route ID, request ID, latency breakdown, and backend target. Then I would correlate with deployment time, config drift, autoscaling, health checks, and error budgets. Mitigation would be targeted: rollback route config for 404/502, tune or disable a bad rate limit for 429, rollback app version for 500, and add/restore healthy targets for 503. Validation would use synthetic checks for each important route and customer segment. RCA would require release gating on route tests, per-status dashboards, and canarying gateway config separately from application code.

## Q12 \| HTTP Headers / Routing \| P2 \| Implementation

**Interview Question:**

> A multi-tenant API gateway routes requests based on host and headers, and one tenant is being routed to the wrong backend after a config rollout. How would you debug host headers, forwarded headers, path rewriting, route precedence, and config drift?

**Candidate Answer:**

I would start with a real failing request and trace the exact routing decision. I would capture host, \`X-Forwarded-Host\`, \`X-Forwarded-Proto\`, tenant header, path before and after rewrite, matched route, and selected upstream. Wrong-tenant routing is usually caused by route precedence, wildcard host rules, stale config on one gateway replica, or a header trust issue where an external client can influence forwarded headers. I would compare the desired config in Git with the effective runtime config and check whether all gateway instances loaded the same version. I would also replay the request against staging and production with the same headers to prove whether behavior differs by config or data.

A concise debug flow I would use is:

curl -vk https://api.example.com/orders -H 'Host: tenant-a.example.com' -H 'X-Tenant-Id: tenant-a' -H 'X-Debug-Route: true'

grep request_id gateway-access.log

nginx -T \| grep -A8 'tenant-a.example.com'

kubectl rollout history deploy/api-gateway

kubectl diff -f gateway-routes.yaml

For mitigation, I would revert the gateway config or pin the tenant to the correct backend with an emergency reviewed rule. Permanent prevention would include route precedence tests, config canarying, drift detection, and rejecting untrusted forwarded headers at the edge.

## Q13 \| HTTP Latency Debugging \| P1 \| SRE

**Interview Question:**

> A customer-facing API latency increased from 100 ms to 2 seconds, but application logs show normal processing time. How would you determine whether latency is introduced by DNS, TCP connect, TLS handshake, proxy queueing, backend wait time, or client-side behavior?

**Candidate Answer:**

I would challenge the assumption that application processing time equals user latency. I would break the request into phases: DNS lookup, TCP connect, TLS handshake, proxy queueing, upstream connect, backend processing, response transfer, and client-side wait. I would start with synthetic tests using curl timing fields from the same region or network as the customer and compare them with gateway and load balancer logs. If application logs show 100 ms but the user sees 2 seconds, the delay may be before the request reaches the app or after the response leaves it. I would inspect CDN/WAF logs, load balancer target response time versus total time, proxy queue length, connection pool wait, TLS handshake time, and DNS resolver latency. Distributed traces are very useful if spans include edge, gateway, app, and downstream calls. I would also check whether clients are reusing connections or creating new TLS connections per request. Mitigation could be bypassing a degraded proxy hop, scaling gateway workers, increasing connection pools carefully, or routing traffic to a healthy region/AZ. The permanent fix is latency budget instrumentation across every hop, SLO alerts on P95/P99, and release tests that capture network timing, not only app duration.

## Q14 \| HTTP Keep-Alive / Connection Pooling \| P2 \| Production Troubleshooting

**Interview Question:**

> A high-throughput service causes sudden connection storms against an upstream API after a minor release. How would you investigate keep-alive behavior, connection pooling changes, ephemeral port exhaustion, upstream limits, and retry amplification?

**Candidate Answer:**

I would investigate the release diff first because a minor client-library or HTTP-agent change can disable keep-alive or change pool limits. I would compare connection count, new connections per second, TIME_WAIT sockets, upstream 429/503, ephemeral port usage, and NAT/SNAT consumption before and after release. If the service suddenly creates a new TCP/TLS connection per request, upstream latency and load balancer/NAT pressure will rise even if request volume is unchanged. I would check client pool configuration: max idle connections, max connections per host, idle timeout, DNS refresh, TLS session reuse, retry limits, and circuit breaker behavior. I would also confirm whether retries amplify traffic during upstream slowness. Short-term mitigation would be rollback, restore keep-alive, cap concurrency, reduce retries, or enable a local connection pool/proxy. If ephemeral ports are exhausted, scaling horizontally without fixing reuse may make NAT exhaustion worse. Permanent prevention includes client-library regression tests for connection behavior, dependency-level connection metrics, sane retry budgets with jitter, and platform defaults for HTTP clients so teams do not accidentally create connection storms.

## Q15 \| TLS Certificate Expiry \| P1 \| Production Troubleshooting

**Interview Question:**

> A production endpoint starts failing only for some clients after a certificate renewal. How would you validate expiry, certificate chain completeness, intermediate certificates, trust stores, client compatibility, and rollback options?

**Candidate Answer:**

I would compare failing and successful clients because certificate issues are often client-trust or chain-related, not global availability. I would inspect the served certificate using SNI, verify expiry, SANs, issuer, full chain, and whether the intermediate certificate is included. Some modern clients can fetch missing intermediates, while older clients or private runtimes cannot. I would test with \`openssl s_client -servername domain -connect host:443 -showcerts\` and validate from affected client trust stores, not just a browser. I would also check whether the load balancer has multiple listeners or certificates and whether all edge locations received the renewed certificate. If only some clients fail after renewal, possible causes include missing intermediate, changed root CA, weaker/stricter signature algorithm, unsupported key type, or pinned certificate. Mitigation would be to attach the previous known-good certificate if still valid, serve the complete chain, or roll traffic to a listener with a compatible certificate. Permanent fixes include automated certificate expiry and chain monitoring, pre-renewal compatibility tests, cert deployment canaries, and a rollback plan that includes all load balancers/CDN edges.

## Q16 \| TLS Handshake Failure \| P1 \| Security

**Interview Question:**

> A service is reachable on port 443 but TLS handshake fails with different errors across clients. How would you troubleshoot protocol versions, cipher suites, certificate chain, mutual TLS, SNI, ALPN, and load balancer TLS policy?

**Candidate Answer:**

I would first collect the exact TLS error from each client because “handshake failed” can mean very different things. I would test protocol versions, cipher suites, SNI, ALPN, and certificate chain using OpenSSL from affected networks. If the service listens on 443 but TLS fails, I would verify whether the listener terminates TLS at the load balancer, passes through to backend, or uses mTLS. A recent TLS policy change may reject older clients due to TLS 1.0/1.1 or cipher removal. An SNI issue may cause the wrong certificate to be served. ALPN issues can affect HTTP/2 clients. With mTLS, I would validate client cert chain, SAN, EKU, expiry, and CA trust bundle. I would also compare load balancer TLS logs, handshake failure metrics, and backend logs for failed client auth. Mitigation should be risk-based: temporarily restore the previous TLS policy if business impact is high, attach a correct certificate chain, or roll back mTLS bundle rotation. Long term, I would maintain TLS compatibility matrices, synthetic TLS checks per client class, controlled certificate rollout, and security-approved deprecation windows.

## Q17 \| SNI Issues \| P2 \| Production Troubleshooting

**Interview Question:**

> Multiple HTTPS services share the same load balancer, and one domain suddenly presents the wrong certificate. How would you debug SNI routing, listener rules, certificate binding order, default certificate behavior, and DNS-to-LB mapping?

**Candidate Answer:**

I would prove which certificate is being served for the exact hostname and load balancer endpoint. I would run TLS checks with SNI explicitly set, because testing by IP or without \`-servername\` can show the default certificate and mislead the investigation. Then I would inspect DNS to confirm the domain points to the expected load balancer and listener. On the load balancer, I would check certificate binding order, listener rules, host-based routing, wildcard certificate precedence, and the default certificate. If multiple HTTPS services share one listener, a recently added wildcard or default cert can override a more specific cert depending on platform behavior. I would also verify that all regions or edge nodes have the same certificate deployment. Mitigation would be to bind the correct cert to the specific host rule, remove the unsafe wildcard/default if needed, or temporarily move the affected domain to a dedicated listener. Validation would include \`openssl s_client -servername affected.domain\`, browser checks, and external synthetic TLS monitoring. Permanent prevention includes automated SNI/certificate mapping tests, GitOps/IaC review for listener changes, and alerts when served certificate SAN does not match the requested hostname.

## Q18 \| mTLS Connectivity \| P3 \| Security

**Interview Question:**

> A service-to-service call using mutual TLS fails after certificate rotation, while one-way TLS endpoints still work. How would you isolate client certificate validity, CA trust bundle, SAN mismatch, revocation, sidecar configuration, and rollout sequencing?

**Candidate Answer:**

I would isolate mTLS separately from one-way TLS. First I would confirm that the server certificate is valid, then focus on client authentication: client certificate expiry, key match, SAN/SPIFFE identity, EKU, issuing CA, and whether the server trusts the rotated CA bundle. Certificate rotation often fails because server trust is updated after clients rotate, or because old and new CAs are not both trusted during the overlap period. I would inspect sidecar or proxy config, SDS/cert-manager status, secret versions, and actual certificate mounted in the running workload. I would check handshake logs from Envoy/NGINX/service mesh and use OpenSSL with the client certificate to reproduce. Revocation or OCSP policy can also break if the revocation endpoint is unreachable. Mitigation would be to restore the previous client cert or temporarily trust both old and new CA bundles while maintaining security approvals. I would avoid disabling client verification unless it is an extreme emergency with compensating controls. Permanent prevention includes staged CA rotation, dual-trust windows, certificate expiry dashboards, identity conformance checks, and rollout sequencing that updates trust bundles before leaf certificates.

## Q19 \| Load Balancer Health Checks \| P1 \| Production Troubleshooting

**Interview Question:**

> A load balancer marks all targets unhealthy even though the application process is running on every node. How would you troubleshoot health check path, port, protocol, host header, security rules, readiness dependency, and target group configuration?

**Candidate Answer:**

I would not equate “process running” with “ready for traffic.” I would inspect the load balancer health check configuration: protocol, port, path, host header, expected status code, interval, timeout, thresholds, and target type. Then I would run the same health check from inside the load balancer path or a similar subnet using the exact host header and port. Common causes include health check hitting the wrong port, \`/health\` depending on a down database, redirecting HTTP to HTTPS when the LB expects HTTP 200, host-header rejection, security group/NSG blocking the LB source, or targets registered with the wrong IP/instance mode. I would compare application readiness logs, target group reason codes, flow logs, and backend access logs. Mitigation could be correcting the health check path/port, temporarily using a dependency-light readiness endpoint, or rolling back the target group change. I would be careful not to mark unhealthy apps healthy just to restore traffic. Long term, I would define readiness versus liveness contracts, health-check integration tests, IaC validation for LB-target rules, and dashboards for target health by AZ and deployment version.

## Q20 \| 502 Bad Gateway \| P1 \| Production Troubleshooting

**Interview Question:**

> A reverse proxy starts returning 502 after a backend deployment, while the backend service appears healthy locally. How would you investigate upstream connection refusal, wrong port, DNS resolution, socket backlog, TLS mismatch, and proxy upstream configuration?

**Candidate Answer:**

I would start from the proxy’s upstream error details, not only the backend’s local health. A backend can be healthy on localhost but unreachable or incompatible from the proxy path. I would check whether the proxy resolves the correct upstream DNS, connects to the right port, and uses the correct protocol. After a backend deployment, common causes are port changes, container readiness mismatch, TLS enabled on backend while proxy expects plaintext, service name change, socket backlog exhaustion, or the app binding only to \`127.0.0.1\` instead of the pod/node interface. I would inspect NGINX/API gateway error logs, upstream status, connect time, target endpoint list, and backend access logs to see whether requests arrive. I would test from the proxy container/node using \`curl -v\` to the upstream endpoint. Mitigation would be rollback backend or proxy config, restore the previous service port, or shift traffic to the last healthy upstream. Permanent fixes include contract tests between proxy and backend, deployment canaries, upstream config generated from service discovery, and alerts on 502 rate with upstream connection failure reason labels.

## Q21 \| 503 Service Unavailable \| P1 \| SRE

**Interview Question:**

> A production load balancer returns 503 during a traffic spike even though autoscaling eventually adds capacity. How would you analyze target availability, connection draining, backend saturation, scaling lag, health check thresholds, and load-shedding behavior?

**Candidate Answer:**

I would analyze the 503 as a capacity and availability failure across time. First I would check whether the load balancer had zero healthy targets, whether existing targets were saturated, or whether connection queues overflowed before autoscaling caught up. Autoscaling eventually adding capacity does not mean the scaling policy was good enough for the traffic ramp. I would review target health by AZ, request count, active connections, surge queue/spillover metrics, backend CPU/thread/connection pool saturation, health check thresholds, and target warm-up time. I would also check connection draining during deployments; aggressive draining plus traffic spike can reduce effective capacity. Short-term mitigation could be manual scale-out, traffic shedding, temporarily relaxing rate limits for critical flows or adding them for noncritical flows, and routing to a healthy region/AZ. Long term, I would tune autoscaling on leading indicators like request rate per target or queue depth, configure pre-warming/scheduled scaling for known peaks, implement load shedding and backpressure, and set SLO burn-rate alerts before full 503 impact. The RCA should include whether capacity planning, health checks, and deployment timing contributed to the incident.

## Q22 \| 504 Gateway Timeout \| P1 \| Production Troubleshooting

**Interview Question:**

> Users receive 504 errors only for long-running requests after a gateway migration. How would you debug timeout alignment across client, CDN, WAF, API gateway, reverse proxy, load balancer, backend, and dependency calls?

**Candidate Answer:**

I would map every timeout on the path because a 504 is often caused by misaligned timeout budgets after a gateway migration. I would list client timeout, CDN/WAF timeout, API gateway timeout, reverse proxy read/connect timeout, load balancer idle timeout, backend server timeout, database/API dependency timeout, and retry policy. Long-running requests may have worked before because the old gateway had a larger timeout or streamed responses differently. I would use request IDs and trace spans to identify where the request stops: did it reach backend, did backend continue processing after gateway returned 504, and did any dependency exceed its budget. Mitigation can include temporarily routing long-running endpoints through the previous gateway, increasing a specific timeout within platform limits, or converting the operation to async if it is not suited for synchronous HTTP. I would avoid simply raising all timeouts because that can exhaust workers and connections. The permanent fix is an explicit timeout hierarchy where upstream timeouts are slightly higher than downstream operation limits, plus idempotency for retries, progress/status APIs for long jobs, and SLO-based latency budgets per endpoint.

## Q23 \| Reverse Proxy Routing \| P1 \| Implementation

**Interview Question:**

> An NGINX or API gateway configuration works in staging but routes production traffic to the wrong upstream after promotion. How would you validate route order, path rewrites, upstream blocks, host matching, default server behavior, and safe rollback?

**Candidate Answer:**

I would validate production’s effective config, not just the repository version. I would compare staging and production route order, host matching, path rewrite rules, upstream blocks, default server, and generated config after templating. Wrong upstream after promotion often happens due to wildcard routes matching before specific routes, environment values pointing to staging/prod names incorrectly, or a default backend catching unmatched paths. I would use a single failing request ID, inspect access logs for matched location/route, and test with explicit host headers.

A concise validation flow would be:

nginx -t

nginx -T \| sed -n '/server_name api.example.com/,/}/p'

curl -vk https://api.example.com/v1/orders -H 'Host: api.example.com' -H 'X-Request-Id: route-debug-001'

grep route-debug-001 /var/log/nginx/access.log

grep route-debug-001 /var/log/nginx/error.log

kubectl rollout undo deploy/nginx-gateway

For mitigation, I would rollback to the previous gateway config or shift traffic to the last known-good gateway deployment. Permanent prevention would include config rendering tests, route precedence unit tests, production canary for gateway config, and automated diff approval before promotion.

## Q24 \| NGINX Connection Issues \| P2 \| Production Troubleshooting

**Interview Question:**

> NGINX shows upstream prematurely closed connection errors during peak traffic. How would you determine whether the root cause is backend crashes, proxy buffering, timeout mismatch, keep-alive reuse, request body size, or network resets?

**Candidate Answer:**

I would start by separating backend close from proxy/network close. NGINX \`upstream prematurely closed connection\` means the upstream closed before NGINX received the full response, but the reason may be backend crash, worker timeout, request body issue, buffering, keep-alive reuse problem, or reset. I would correlate NGINX error logs with backend logs using request IDs and check whether backend pods/processes restart, hit OOM, exceed worker/thread limits, or close idle keep-alive connections earlier than NGINX expects. During peak traffic, I would inspect upstream response time, active connections, NGINX worker connections, file descriptors, proxy buffer usage, request/response size, and timeout settings. If large uploads are involved, body size and buffering settings are candidates. If packet captures show RST, I would identify which side sends it. Mitigation could be rollback, align keep-alive and read timeouts, increase worker/file descriptor limits, scale backend capacity, or disable reuse for the affected upstream temporarily. Long term, I would standardize proxy-backend timeout contracts, add load tests for peak traffic and large payloads, and dashboard NGINX upstream errors by reason and backend version.

## Q25 \| API Gateway Rate Limiting \| P2 \| SRE

**Interview Question:**

> A newly enabled API gateway rate limit blocks legitimate production traffic and causes elevated 429 responses. How would you validate client identity, burst versus sustained limits, NAT-shared IP impact, route-specific policy, and emergency mitigation?

**Candidate Answer:**

I would first quantify which legitimate traffic is blocked: tenants, routes, client identities, source IPs, API keys, and NAT gateways. Rate limits should normally be based on authenticated identity or tenant, not only source IP, because many real users may share one corporate NAT, mobile carrier NAT, or cloud egress IP. I would inspect gateway logs for rate-limit key, remaining quota, burst policy, window size, route policy, and whether the new policy inherited a default that is too strict. I would also compare sustained traffic versus burst behavior because blocking may happen only at traffic spikes. Emergency mitigation would be to raise or disable the limit for critical routes or known legitimate identities, while keeping abuse protection on public unauthenticated paths. I would communicate customer impact and recovery criteria because 429s can look like client bugs. Permanent prevention includes shadow-mode rate limits before enforcement, per-tenant quotas, allowlists with expiry for critical integrations, dashboards for 429 by route/key, and load tests that simulate NAT-shared clients. I would also ensure retry guidance uses backoff to avoid amplifying the rate-limit event.

## Q26 \| Firewall / Inbound Rules \| P1 \| Security

**Interview Question:**

> A production service becomes unreachable after a firewall hardening change, but monitoring still shows the host as healthy. How would you trace inbound packet flow across host firewall, security groups, NSG, subnet ACLs, load balancer rules, and application listener state?

**Candidate Answer:**

I would trace inbound flow hop by hop because host health checks often do not prove service reachability. First I would confirm DNS points to the expected load balancer or host, then verify load balancer listener, target group health, subnet/NACL, security group or NSG, host firewall, and application listener state. I would compare before/after firewall hardening changes and inspect effective rules, not only intended rules. Flow logs are useful to prove whether packets are accepted or rejected at subnet/security group level. On the host, I would check \`ss -tulnp\`, local firewall rules, and packet capture to see whether SYN packets arrive. If monitoring still shows healthy, the monitoring path may be from an internal subnet while customer traffic is blocked from external sources. Mitigation would be to restore the last known-good inbound rule set or add the minimum required source/port rule with change approval. I would validate from real customer paths and synthetic probes. Permanent prevention includes firewall changes through IaC, pre-change reachability tests, blast-radius analysis, and monitoring from the same network path users take.

## Q27 \| Firewall / Outbound Rules \| P1 \| Production Troubleshooting

**Interview Question:**

> An application can receive traffic but fails to call external payment and notification services after a network policy update. How would you debug outbound firewall rules, proxy routing, DNS allowlists, NAT path, TLS inspection, and dependency-specific egress requirements?

**Candidate Answer:**

I would treat this as dependency egress failure and identify exactly which external calls fail: payment, notification, DNS, proxy, or TLS. I would test from the application subnet/container using the dependency FQDN, not only IP, because allowlists often depend on DNS CNAME chains and SNI. I would inspect outbound firewall rules, egress network policy, proxy configuration, NAT route, DNS resolver rules, and TLS inspection certificates. Some payment providers require both HTTPS egress and OCSP/CRL endpoints for certificate validation, so I would check those too. Flow logs should show whether traffic is denied by rule, routed incorrectly, or never leaving because DNS fails. If a proxy is mandatory, I would verify app runtime proxy variables and \`NO_PROXY\`. Mitigation could be restoring the previous egress policy, allowing the specific dependency FQDN/IP ranges through the approved path, or routing only affected traffic through a known-good proxy. Permanent fixes include dependency egress inventory, domain-based policy automation where supported, preflight synthetic checks for critical third parties, and change gates that validate payment/notification flows before enforcing network policy.

## Q28 \| Security Groups / NSG Drift \| P2 \| Platform Engineering

**Interview Question:**

> Different nodes in the same production tier show different connectivity behavior after repeated emergency changes. How would you identify security group or NSG drift, prove the effective rules, and convert manual fixes into governed platform automation?

**Candidate Answer:**

I would start by proving the effective rule difference between nodes, because repeated emergency changes often leave attached security groups, NSGs, or host firewall rules inconsistent. I would group nodes by connectivity outcome, then compare their subnet, NIC, security groups/NSGs, route tables, tags, and IaC state. Cloud “effective security rules” views, flow logs, and connection tests can prove whether traffic is denied by missing allow rule, higher-priority deny, wrong source prefix, or stale attachment. I would also check whether auto-scaled nodes are launched from an old template that attaches different groups. For mitigation, I would converge the production tier to one approved rule set, preferably by reattaching the correct managed group or applying the last known-good IaC state. I would avoid one-off console fixes except as audited emergency change. Long term, I would codify security rules in Terraform/Bicep/CloudFormation, enforce drift detection, block unmanaged manual edits through policy where possible, and add connectivity conformance tests for every node group. The platform improvement is to make network policy a product-level golden path, not tribal manual knowledge.

## Q29 \| NAT Gateway Egress Failure \| P1 \| Production Troubleshooting

**Interview Question:**

> Private subnet workloads suddenly fail to reach the internet for image pulls, package downloads, and third-party APIs. How would you investigate NAT gateway health, route tables, subnet association, SNAT port exhaustion, DNS, and cloud provider limits?

**Candidate Answer:**

I would first confirm whether the failure is universal egress or only specific destinations. For private subnet workloads, I would inspect DNS resolution, route table association, default route to NAT gateway or firewall, NAT gateway health, public IP attachment, subnet NACL/security rules, and provider NAT metrics. If failures affect image pulls, package downloads, and third-party APIs at once, the common path is likely NAT, egress firewall, proxy, or DNS. I would check SNAT port allocation/errors, connection count, bytes processed, dropped packets, and whether a route table change removed the \`0.0.0.0/0\` route to NAT. I would also verify whether new subnets were created without NAT route association. Mitigation could be restoring the route table, failing over to another NAT gateway/AZ, routing through an approved proxy, or temporarily using private endpoints for registries if available. Long term, I would design NAT per AZ for resilience, monitor SNAT and NAT errors, use private endpoints/mirrors for high-volume services, and enforce subnet-route association through IaC and policy tests.

## Q30 \| SNAT Port Exhaustion \| P2 \| Production Troubleshooting

**Interview Question:**

> A large number of private workloads intermittently fail outbound calls to the same external endpoint under load. How would you identify SNAT port exhaustion, connection reuse issues, NAT gateway scaling limits, and application-level connection management problems?

**Candidate Answer:**

I would suspect SNAT exhaustion when many private workloads call the same external IP/port and failures are intermittent under load. I would confirm using NAT gateway metrics, firewall connection table, flow logs, and application connection behavior. Symptoms include connect timeouts, sporadic resets, and high \`TIME_WAIT\` while DNS and target service are otherwise healthy. I would check whether applications disabled keep-alive, create excessive short-lived connections, or retry aggressively, because that consumes many source ports. I would also identify whether all workloads share one NAT IP and one destination tuple, which limits available SNAT ports. Short-term mitigation could be restoring keep-alive, reducing concurrency/retries, adding NAT public IPs or additional NAT gateways, sharding egress by subnet/AZ, or using a proxy with connection pooling. Scaling pods alone may worsen exhaustion if connection reuse is poor. Permanent prevention includes connection-pool standards, SNAT utilization dashboards, load testing for dependency egress, private endpoints where possible, and retry budgets with jitter and circuit breakers so failures do not create a connection storm.

## Q31 \| Private Subnet Routing \| P1 \| Architecture

**Interview Question:**

> A platform team must run workloads in private subnets while still allowing controlled egress for updates, artifact pulls, and external APIs. How would you design routing, NAT, private endpoints, proxying, logging, and failure isolation?

**Candidate Answer:**

I would design private subnets with controlled, observable egress rather than open internet access. Workloads would have no public IPs, and default egress would route through per-AZ NAT gateways, an egress firewall, or an approved proxy depending on compliance needs. High-volume cloud dependencies like container registry, object storage, secrets manager, package mirror, and database should use private endpoints/service endpoints where available to reduce NAT dependency and improve security. Route tables must be explicit per subnet, with per-AZ NAT to avoid cross-AZ blast radius. Egress policies should be domain/IP scoped, logged, and managed by IaC. For third-party APIs, I would use an egress proxy or firewall that supports allowlisting, TLS policy, and audit logs. Failure isolation matters: if one NAT gateway or firewall path fails, only that AZ/subnet should degrade, and critical workloads should have tested failover. I would add flow logs, NAT metrics, proxy logs, DNS logs, and synthetic checks for image pulls, artifact downloads, and critical APIs. Rollout should begin with observe-only mode, then enforce in stages. This design balances security, reliability, cost, and operational debuggability.

## Q32 \| Route Table Misconfiguration \| P1 \| Production Troubleshooting

**Interview Question:**

> A service is reachable from one subnet but not another after a route table change. How would you verify effective routes, subnet associations, longest-prefix matching, default routes, blackhole routes, and return-path symmetry?

**Candidate Answer:**

I would compare the working and failing subnet route evaluation. First I would identify the source IP, destination IP, subnet association, and effective route table. Then I would apply longest-prefix matching: a more specific route to a blackhole, firewall, peering, or appliance can override the default route. I would check whether the route table change affected only one subnet association, whether a propagated route disappeared, or whether a blackhole route exists because a NAT gateway, peering, or next hop was deleted. Return-path symmetry is equally important; the request may reach the service, but the response may return through a different firewall or route. I would verify with flow logs, packet capture, cloud network watcher/reachability tools, and traceroute where ICMP is allowed. Mitigation would be restoring the previous route table or adding the correct specific route with careful blast-radius review. For production, I would validate from both directions before declaring recovery. Permanent prevention includes route-table changes through IaC, route diff review highlighting affected subnets, automated reachability tests for critical paths, and alerts for blackhole routes or unexpected propagated-route changes.

## Q33 \| VPC / VNet Peering \| P1 \| Production Troubleshooting

**Interview Question:**

> Two services in peered networks cannot communicate after an environment expansion, while existing environments still work. How would you debug CIDR overlap, route propagation, DNS resolution, security rules, transitive routing assumptions, and peering directionality?

**Candidate Answer:**

I would start with the expansion details because peering failures often come from assumptions that worked in older environments but do not scale automatically. I would verify CIDR ranges first; overlap or too-broad routes can silently break new environments. Then I would inspect both peering directions, route tables, propagated routes, security groups/NSGs, network ACLs, and whether DNS resolution across the peering is enabled. Many cloud peerings are non-transitive, so if the new environment expects to reach a third network through an existing peer, it will fail unless a transit gateway, hub-spoke, or route server is designed for that. I would also confirm that private hosted zones/private DNS zones are associated or forwarded correctly for the new network. Flow logs on both sides would show whether packets leave the source and whether they are rejected at destination. Mitigation could be adding the missing routes, correcting security rules, enabling DNS resolution, or routing through the intended transit component. Permanent prevention includes environment-expansion templates that allocate non-overlapping CIDRs, create bidirectional routes/security rules, link DNS zones, and run automated cross-network connectivity tests before production use.

## Q34 \| Private Endpoint / Private Link \| P1 \| Production Troubleshooting

**Interview Question:**

> A database private endpoint works from one application subnet but fails from another, and public access is disabled. How would you troubleshoot DNS override, private zone linkage, endpoint network interface, route paths, security rules, and service-level firewall settings?

**Candidate Answer:**

I would debug private endpoint issues as DNS plus network plus service firewall. First I would resolve the database FQDN from both application subnets. The successful subnet should resolve to the private endpoint IP, not the public service IP. If the failing subnet resolves differently, I would check private DNS zone records, zone linkage to the VNet/VPC, resolver forwarding, and search/split-horizon behavior. If both resolve to the private IP, I would inspect routes to the endpoint NIC, subnet NSGs, endpoint network policies, NACLs, and whether the source subnet is allowed by service-level firewall rules. I would also verify the private endpoint connection state is approved and attached to the correct resource/subresource. Packet capture and flow logs can show whether traffic reaches the endpoint NIC. Mitigation would be linking the missing private DNS zone, correcting the route/security rule, or temporarily enabling a tightly restricted fallback path only if approved. Permanent rollout should include private DNS validation from every subnet, IaC-managed endpoint and zone links, service firewall tests, and a pre-disable-public-access checklist.

## Q35 \| Service Endpoint / Private Access \| P2 \| Security

**Interview Question:**

> A storage or database service was locked down to private access, and production workloads lost connectivity immediately. How would you recover safely and redesign the rollout to validate service endpoints, private DNS, subnet policies, and fallback access?

**Candidate Answer:**

I would recover by restoring controlled access first, then redesign the rollout. If production lost connectivity immediately after locking down to private access, I would confirm whether workloads resolve the service to the private endpoint/service endpoint path and whether subnet policies are enabled. The safest mitigation may be to temporarily re-enable public access only for known egress IPs or revert the firewall change, while keeping audit visibility. I would avoid a broad public opening. Then I would validate service endpoint/private endpoint configuration: subnet association, private DNS record, route path, service firewall, managed identity or IAM permissions, and application connection strings. The redesigned rollout would be phased: create private endpoint, link private DNS, test from each workload subnet, enable dual-path if supported, monitor traffic moving to private IPs, then disable public access only after synthetics and production canaries pass. For permanent controls, I would add IaC checks that prevent public lockdown without private DNS and subnet validation, dashboards showing public versus private access, and a rollback plan with exact firewall changes. This keeps security goals intact while reducing outage risk.

## Q36 \| VPN Routing \| P1 \| Production Troubleshooting

**Interview Question:**

> On-premises systems can reach some cloud workloads over VPN but not a newly added subnet. How would you investigate route advertisement, BGP sessions, tunnel health, firewall rules, overlapping CIDRs, asymmetric routing, and MTU constraints?

**Candidate Answer:**

I would compare old reachable cloud subnets with the newly added subnet. Over VPN, the most common issue is missing route advertisement or firewall policy for the new prefix. I would check tunnel status, BGP session health if dynamic routing is used, advertised prefixes from cloud to on-prem and on-prem to cloud, route tables on both sides, and whether the new subnet CIDR overlaps with any on-prem network. I would also check firewall rules at on-prem edge, cloud NSGs/security groups, and subnet NACLs. If traffic partially works or large packets fail, I would investigate MTU and MSS because VPN overhead can break path MTU discovery. Asymmetric routing is another risk if return traffic chooses a different tunnel or internet path. Mitigation would be advertising the missing prefix, updating firewall rules, correcting route priority, or temporarily routing through a known-good transit path. I would validate with packet captures on both tunnel endpoints and application-level tests. Permanent prevention includes automated route advertisement validation for new subnets, CIDR allocation governance, VPN monitoring for BGP/tunnel health, and pre-production connectivity tests for each new subnet.

## Q37 \| ExpressRoute / Direct Connect \| P2 \| Architecture

**Interview Question:**

> A latency-sensitive enterprise workload depends on ExpressRoute or Direct Connect, and failover to VPN must be controlled. How would you design routing preference, redundancy, health checks, failover testing, observability, and rollback for production?

**Candidate Answer:**

I would design private connectivity with deterministic primary and controlled failover. ExpressRoute or Direct Connect should be redundant across circuits, locations, and devices where the business case justifies it, with BGP used for dynamic routing. I would prefer the private circuit for latency-sensitive enterprise traffic by route preference/local preference, while keeping VPN as backup with lower preference. Failover must be tested because uncontrolled failover can create asymmetric paths, MTU issues, or overload the VPN. I would define health checks at multiple layers: BGP session, tunnel/circuit status, packet loss, latency, and application synthetic checks. Observability should include route changes, advertised prefixes, latency by path, packet loss, and bandwidth saturation. Rollback means restoring previous route advertisements or local preference, not making ad-hoc firewall edits during an outage. I would also document which prefixes are allowed over VPN backup and whether traffic volume can fit. Security controls include encryption where required, route filtering, and least-prefix advertisement. This architecture gives predictable low-latency routing while preserving a tested degraded-mode path if the private circuit fails.

## Q38 \| BGP Route Propagation \| P3 \| Production Troubleshooting

**Interview Question:**

> A hybrid network incident causes traffic to follow an unexpected path after a route advertisement change. How would you validate BGP propagation, route priority, AS-path behavior, advertised prefixes, and blast radius before remediation?

**Candidate Answer:**

I would first freeze further route changes and identify the blast radius: which prefixes, sites, applications, and paths changed. For BGP, I would inspect the RIB and FIB on cloud route tables, transit gateways/route servers, edge routers, and firewalls. I would compare advertised prefixes before and after the change, AS path, local preference, MED, communities, and route priority rules. Unexpected paths often come from a more specific prefix, a changed local preference, or leaked advertisement. I would validate with traceroute, flow logs, and packet captures, but I would trust routing tables over assumptions. Before remediation, I would model the impact of withdrawing or changing advertisements because it can shift large traffic volumes suddenly. Mitigation could be restoring the previous prefix advertisement, applying a route filter, lowering preference, or withdrawing a leaked route. Validation must include both forward and return path. Permanent prevention includes BGP change templates, prefix allowlists, route-policy tests, route monitoring, and an approval process that explicitly shows impacted prefixes and expected path changes.

## Q39 \| MTU / Fragmentation \| P2 \| Production Troubleshooting

**Interview Question:**

> Large API requests fail intermittently over VPN or private connectivity, while small requests work. How would you investigate MTU, path MTU discovery, fragmentation blocking, tunnel overhead, TCP MSS clamping, and application retry behavior?

**Candidate Answer:**

I would suspect MTU when small requests succeed but large requests fail, especially over VPN, GRE/IPsec, service mesh, or private connectivity with encapsulation overhead. I would test path MTU using ping with DF bit or tools that vary packet size, from both sides if possible. I would check whether ICMP “fragmentation needed” is blocked, because that breaks path MTU discovery and causes black-hole behavior for large packets. Packet captures would show retransmissions, stalled large segments, or fragmentation. I would also inspect tunnel MTU, interface MTU, load balancer behavior, and whether TCP MSS clamping is configured on VPN/firewall devices. Application retries can hide the problem and amplify latency, so I would correlate request size with failures. Mitigation could be enabling MSS clamping, lowering interface/tunnel MTU, allowing required ICMP, or temporarily limiting large request size. Permanent fixes include standardized MTU for hybrid paths, automated MTU checks in network validation, clear retry/idempotency behavior for large requests, and dashboards for retransmissions and size-based failure rates.

## Q40 \| Service Discovery \| P1 \| Platform Engineering

**Interview Question:**

> A microservice platform has intermittent cross-service failures because service discovery returns stale or unhealthy endpoints. How would you debug registration, deregistration, TTLs, health checks, client-side caching, and rollout coordination?

**Candidate Answer:**

I would debug service discovery as a control-plane and client behavior problem. First I would confirm whether stale or unhealthy endpoints are present in the registry, DNS, or client cache. I would inspect registration timestamps, health check status, deregistration delay, TTLs, and whether terminating instances are removed before receiving traffic. During rollouts, stale endpoints often remain because shutdown hooks do not deregister, health checks are too weak, or clients cache DNS/registry responses longer than intended. I would compare registry state with actual pod/instance readiness and load balancer target health. On the client side, I would check DNS cache TTL, connection pooling to old endpoints, retry policy, and whether clients honor health status. Mitigation would be to manually remove bad endpoints, extend drain periods, roll back the failing version, or reduce client cache TTL where safe. Permanent platform fixes include lifecycle hooks for deregistration, readiness-based registration, bounded TTLs, endpoint freshness metrics, rollout coordination with connection draining, and tests that prove a terminated instance disappears from discovery before it stops serving.

## Q41 \| Cross-Service Latency \| P1 \| SRE

**Interview Question:**

> A checkout workflow becomes slow only when calling one downstream service in another network segment. How would you isolate DNS latency, TCP connect time, TLS handshake, proxy hops, load balancer queueing, backend latency, and dependency saturation?

**Candidate Answer:**

I would break the checkout call into network and dependency phases instead of blaming the downstream service immediately. I would measure DNS lookup time, TCP connect time, TLS handshake time, proxy hops, load balancer queueing, downstream processing time, and response transfer. Distributed tracing is the fastest path if spans include the checkout service, gateway/proxy, downstream service, and database/dependency calls. If latency appears before downstream app processing, I would inspect DNS resolver latency, route path, NAT/firewall/proxy saturation, TLS handshakes, and cross-segment packet loss. If load balancer total time is high but target response time is low, queueing or connection setup may be the issue. If downstream app time is high, I would inspect its saturation and dependencies. Mitigation could be traffic shifting to a closer/healthier segment, enabling connection reuse, scaling the proxy/downstream, or adding load shedding for noncritical checkout steps. Permanent fixes include per-hop latency dashboards, synthetic checks between segments, dependency SLOs, timeout budgets, and architecture review to reduce unnecessary network hops for critical user journeys.

## Q42 \| Packet Flow Debugging \| P1 \| Production Troubleshooting

**Interview Question:**

> A critical request path crosses client, CDN, WAF, load balancer, reverse proxy, service, database, and external dependency. During a P1 outage, how would you map packet flow and identify the failing hop without relying on assumptions?

**Candidate Answer:**

During a P1, I would create a packet/request path map before taking random actions. I would identify the exact failing transaction, user segment, request ID, source IP, destination hostname, and timestamp. Then I would trace each hop: client DNS and connect, CDN edge logs, WAF decision, load balancer listener and target, reverse proxy route and upstream status, service logs/traces, database connection, and external dependency call. At every hop I would ask: did the request arrive, was it allowed, where was it forwarded, how long did it wait, and what response or reset was produced? I would use access logs, flow logs, packet captures, trace spans, LB target metrics, WAF logs, and dependency dashboards. If a hop lacks evidence, I would add a temporary safe capture or synthetic probe from that network segment. Mitigation would target the first failing hop only: bypass a bad WAF rule, rollback proxy config, shift LB targets, or fail over dependency path. I would communicate facts and uncertainty clearly. Permanent improvement is a maintained request-path diagram, correlation IDs across layers, and runbooks that list exact logs and dashboards for each hop.

## Q43 \| Asymmetric Routing \| P2 \| Production Troubleshooting

**Interview Question:**

> Firewall logs show inbound packets allowed, but clients still time out and packet captures show incomplete sessions. How would you investigate asymmetric routing, return path filtering, NAT state, route priority, and multi-homed host behavior?

**Candidate Answer:**

I would suspect asymmetric routing when inbound packets are allowed but the TCP session never completes or captures show SYN without valid return traffic. I would capture on both client-side and server-side interfaces to see whether SYN arrives, SYN-ACK leaves, and whether the response returns through a different firewall/NAT path. Stateful firewalls and NAT gateways require both directions to pass through the same stateful device; otherwise return packets may be dropped even if inbound rules allow the initial packet. I would inspect route tables, route priority, policy-based routing, multi-homed host routes, reverse path filtering, and NAT translations. Recent route advertisements or failover changes often cause this. Mitigation would be to restore symmetric routing through the intended firewall/NAT pair, adjust route priority, or temporarily pin traffic to one path. I would be cautious with disabling reverse path filtering or stateful inspection because it can create security risk. Permanent prevention includes route symmetry checks in network design, flow-log correlation for both directions, documented multi-homed host routing, and change validation that tests full TCP handshake, not just one-way reachability.

## Q44 \| Network Observability \| P2 \| SRE

**Interview Question:**

> Your team repeatedly struggles to prove whether incidents are network, application, or dependency related. What network-level metrics, flow logs, packet capture strategy, synthetic checks, DNS monitoring, and runbook improvements would you implement?

**Candidate Answer:**

I would implement network observability that lets us prove the failure domain quickly. At minimum, I would enable VPC/VNet flow logs with accept/reject status, NAT gateway metrics, load balancer metrics, firewall logs, DNS query logs, and proxy/gateway access logs with request IDs. For host and Kubernetes nodes, I would capture TCP retransmits, connection resets, conntrack usage, socket states, interface drops, and DNS resolver latency. Packet capture should be controlled: ring-buffer captures on critical gateways or on-demand captures with approved runbooks, retention limits, and privacy controls. Synthetic checks should cover DNS resolution, TCP connect, TLS handshake, HTTP status, and critical dependency calls from each important subnet/region. Dashboards should split latency into DNS/connect/TLS/backend where possible and show 5xx/429 by route and upstream. Runbooks should map request paths and tell engineers exactly which log proves each hop. I would also add change annotations so incidents can be correlated with route, firewall, DNS, or LB changes. This reduces debate during incidents and improves MTTR by replacing opinions with evidence.

## Q45 \| Network Change Rollback \| P1 \| Leadership

**Interview Question:**

> A network change affecting route tables, firewall rules, and load balancer listeners causes a revenue-impacting outage. How would you lead containment, rollback, validation, stakeholder communication, and post-incident control improvements?

**Candidate Answer:**

I would first take incident command and move the team from discussion to containment. My immediate goal would be to stop revenue impact, not prove root cause. I would identify the impacted flows, freeze further network changes, assign owners for route tables, firewall rules, load balancer listeners, and validation, and establish a clear communication cadence. If the change was recent and rollback is known, I would prefer reverting to the last known-good IaC or config version rather than manually editing multiple layers. If rollback risk is high, I would contain by routing critical traffic through a known-good listener/path or restoring the minimum required firewall rule. Validation would include real user path synthetics, LB target health, flow logs, HTTP success rate, conversion/revenue metrics, and error-budget recovery. Stakeholders should receive impact, action, ETA uncertainty, and next update time without low-level noise. After recovery, I would lead a blameless postmortem: timeline, failed assumptions, missing prechecks, rollback gaps, and control improvements. Permanent actions would include network change canaries, automated reachability tests, mandatory blast-radius review, IaC-only changes, peer approval for listener/route/firewall changes, and dashboards annotated with network deploys.

## Q46 \| Resume Deep Dive / Network Incident Ownership \| P1 \| Resume Deep Dive

**Interview Question:**

> Describe a production networking incident you personally owned end to end. What was the user impact, exact packet or request path, investigation evidence, mitigation, rollback strategy, RCA, and long-term platform improvement?

**Candidate Answer:**

One production networking incident I would describe end to end is a private-subnet egress failure after a route/security hardening change. The user impact was that deployments and some runtime services could not reach artifact repositories and third-party APIs, causing release blockage and partial feature degradation. I first mapped the path: workload pod or VM to subnet route table, NAT/egress firewall, DNS resolver, internet endpoint, and TLS handshake. Evidence came from failed \`curl\`/artifact pull from affected subnets, flow logs showing rejected outbound connections, NAT metrics, and comparison with a healthy runner group in another subnet. The mitigation was to stop ongoing rollout, move critical jobs to a known-good runner group, and restore the previous approved egress rule/route for only required destinations. I validated recovery with image pulls, dependency synthetic checks, and application success metrics. The rollback strategy was IaC-based so we could restore the prior network state instead of making untracked console edits. RCA showed the change lacked subnet-level egress validation and did not account for DNS CNAME targets. Long term, I implemented a dependency egress inventory, pre-change connectivity tests from every subnet, flow-log dashboards, and a runbook that maps DNS, route, firewall, NAT, and TLS checks for future incidents.
