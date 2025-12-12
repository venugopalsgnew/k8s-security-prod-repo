
# Kubernetes Security Cheatsheet — Production Grade

## Core Principles
- Principle of Least Privilege (PoLP)
- Defense in Depth (network, host, pod, app)
- Immutable infrastructure and reproducible builds
- Fail-safe defaults (deny-by-default)
- Audit and observability

## Authentication & Authorization
- **AuthN**: Users, service accounts, OIDC, client certs, kubeconfig.
- **AuthZ**: RBAC (Roles, ClusterRoles, RoleBindings, ClusterRoleBindings).
  - Use namespace-scoped Roles when possible.
  - Avoid granting `cluster-admin`.
- Use `kubectl auth can-i` to test permissions.

## Network Security
- CNI with NetworkPolicy support (Calico, Cilium).
- Default to `deny-all` ingress/egress per namespace.
- Use NetworkPolicies with label selectors and IPBlocks for external services.

## Workload Hardening
- Pod Security Admission (PSA) / PodSecurity Standards: Privileged | Baseline | Restricted.
- SecurityContext best practices:
  - runAsNonRoot: true
  - readOnlyRootFilesystem: true
  - capabilities: drop ALL, add only minimal
  - allowPrivilegeEscalation: false
- Avoid hostPath, hostNetwork, hostPID unless necessary.

## Images & Supply Chain
- Build immutable images; tag by digest.
- Scan images in CI (Trivy, Clair).
- Sign images (cosign) and verify in admission controller.
- Use private registries and network controls.

## Secrets Management
- Use Kubernetes Secrets encrypted at rest (encryptionConfiguration).
- Prefer external Secrets (HashiCorp Vault, AWS Secrets Manager) via CSI driver.
- Rotate secrets regularly; avoid hardcoding.

## Policy Enforcement
- Admission controllers: OPA Gatekeeper, Kyverno.
- Enforce policies: disallow hostPath, require image registry, require resource requests/limits.

## Resource Controls
- Requests and Limits for CPU/memory.
- LimitRanges and ResourceQuotas for namespaces.
- Use VerticalPodAutoscaler (carefully) and HPA.

## Node / Cluster Security
- Node isolation, limited SSH, OS hardening (CIS benchmark).
- kubelet authentication & authorization, protect kubelet APIs.
- Secure etcd: TLS, client certs, network isolation, encryption at rest.

## Observability & Auditing
- Enable Audit Logging; export to central logging (ELK, Cloud Logging).
- Use Prometheus/Grafana for metrics, and Falco/Sysdig for runtime security.
- Centralize logs and set retention policies.

## CI/CD Integration
- Gate: scan images, IaC scanning (kics, tfsec), manifest lint (kube-linter), policy checks (conftest/OPA).
- Deploy to ephemeral test cluster before prod.
- Automate signing and verification of images.

## Incident Response (Short)
- Detect -> Isolate -> Investigate -> Remediate -> Recover -> Postmortem
- Keep playbooks: image compromise, node compromise, data exfiltration.

## Useful Commands
- `kubectl auth can-i create pods --as system:serviceaccount:ns:sa`
- `kubectl get events --sort-by='.metadata.creationTimestamp'`
- `kubectl create secret generic db --from-literal=username=admin --from-literal=password=...`
