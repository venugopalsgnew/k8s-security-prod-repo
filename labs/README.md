
# Labs Folder

This folder contains hands-on manifests used in workshops.

Files:
- 01-rbac-namespace-role.yaml : Namespace + Role + RoleBinding + ServiceAccount
- 02-networkpolicy.yaml : Deny-all with specific allow rule
- 03-podsecurity-examples.yaml : insecure vs hardened pod examples
- 04-secrets-example.yaml : Secret + consumer pod
- 05-resource-limits.yaml : LimitRange and ResourceQuota examples

Usage:
kubectl apply -f labs/01-rbac-namespace-role.yaml
kubectl apply -f labs/02-networkpolicy.yaml
...
