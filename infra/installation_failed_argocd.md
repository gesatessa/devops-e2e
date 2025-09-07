
```sh
╷
│ Error: installation failed
│ 
│   with helm_release.argocd,
│   on argocd.tf line 1, in resource "helm_release" "argocd":
│    1: resource "helm_release" "argocd" {
│ 
│ cannot re-use a name that is still in use
```

How to fix:
```sh
# because the release already exists in the cluster, simply import it
# format: <resource_type>.<resource_name> <release_name>/<namespace>
terraform import helm_release.argocd argocd/argocd

# Now TF should be able to reconcile the config with the actual release.
# check:
terraform plan




```
