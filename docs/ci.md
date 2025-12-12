
# CI/CD Security Workflows

## Trivy Image Scan
- Builds image on GH runner
- Scans image for vulnerabilities
- Fails the workflow on detection (exit-code: 1) — configure policy as needed

## Kubernetes Manifest Lint
- Runs kube-linter on `labs/` folder
- Catches anti-patterns and security issues (hostPath, runAsRoot, etc.)

## Notes
- For production pipelines, run scans at multiple stages: build, merge, pre-deploy.
- Use cosign to sign images and have admission controller verify signature before allowing deployment.
