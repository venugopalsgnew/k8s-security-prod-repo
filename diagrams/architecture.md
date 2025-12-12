
# Architecture Diagram (Mermaid)

```mermaid
flowchart LR
  CI[CI/CD Pipeline] --> Registry[Image Registry (scanned & signed)]
  Registry --> APIServer[Kubernetes API Server (Admission Controllers)]
  APIServer --> |Schedules| Nodes[Worker Nodes]
  Nodes --> Pods[Application Pods]
  subgraph Addons
    NetworkPolicyEngine((CNI: Calico/Cilium))
    ServiceMesh((mTLS: Istio/Linkerd))
    SecretsStore((Vault/KMS))
    RuntimeSec((Falco/Sysdig))
  end
  APIServer --> Addons
  Nodes --> Addons
```
