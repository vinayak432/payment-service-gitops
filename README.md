# payment-service-gitops

GitOps repository for the payment-service deployment. This repo contains 
only the Helm chart and values — no application code.

ArgoCD watches this repo and automatically syncs the cluster when anything changes.

## How it works

1. Jenkins builds a new Docker image and pushes it to ECR
2. Jenkins updates `apps/payment-service/values.yaml` with the new image tag
3. ArgoCD detects the commit and syncs the change to EKS
4. New pods roll out automatically

No manual kubectl commands. Git is the source of truth for cluster state.

## Structure

apps/payment-service/

├── Chart.yaml          # Helm chart metadata

├── values.yaml         # image tag, replicas, resources, HPA config

└── templates/

├── deployment.yaml

├── service.yaml

└── hpa.yaml        # scales 2-5 pods based on CPU (target: 30%)


## ArgoCD app config

| Setting | Value |
|---------|-------|
| Sync policy | Automated |
| Self-heal | Enabled |
| Auto-prune | Enabled |
| Namespace | payment-service |

## Related repos

- [payment-service](https://github.com/vinayak432/payment-service) — application code and Jenkinsfile
- [terraform-eks-setup](https://github.com/vinayak432/terraform-eks-setup-) — EKS cluster infrastructure


