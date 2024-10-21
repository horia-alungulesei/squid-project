# squid-project

A proof-of-concept for ArgoCD deployments.

## Initial ArgoCD installation

`helm install argocd argocd --values argocd/values.yaml -n argocd`

## Update ArgoCD

1. Bump version in `Chart.yaml` file.
2. Run `helm dep update argocd/`.
3. Commit `Chart.lock` file.
4. Run `helm upgrade argocd argocd --values argocd/values.yaml -n argocd`.

TODO: [Manage Argo CD Using Argo CD](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#manage-argo-cd-using-argo-cd)

## Deploy example app

### Github Authentication

For the PoC: using a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic).

### Authorization to read Helm charts from S3

Manually added `s3:GetObject` permission to role https://us-east-1.console.aws.amazon.com/iam/home?region=eu-west-1#/roles/details/tools-staging2023111014034783450000000b?section=permissions.

### ArgoCD configuration for Helmfile

Out-of-the-box, ArgoCD does not know about Helmfile. This functionality can be added using a side-car for the Helmfile binary. Details here: https://christianhuth.de/deploying-helm-charts-using-argocd-and-helmfile/

These changes have been applied in the ArgoCD values.yaml file.

### Repository connection

Added `ebury-manifests` as a new repository connection.

### Application deployment

Configured the `example` service to be deployed in the `tools-staging` cluster using Argo, from this path: https://github.com/Ebury/ebury-manifests/blob/master/k8s-environments/emp-devel/staging/example/helmfile.yaml. 

For the deployment to be successful additional cluster configuration is needed to apply the workload module and to add support for External Secrets.
