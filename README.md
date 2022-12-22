# Argo

# Installation of ArgoCD through helm.
values.yaml
```shell
configs:
  secret:
    # -- Bcrypt hashed admin password
    ## Argo expects the password in the secret to be bcrypt hashed. You can create this hash with
    ## `htpasswd -nbBC 10 "" $ARGO_PWD | tr -d ':\n' | sed 's/$2y/$2a/'`
    argocdServerAdminPassword: "$2a$10$oAF16VyqCGi6Hk5fzIPvVOBAgdXf388h257FokIdwiEEVe2n1dH4S"
server:
  service:
    # -- Server service annotations
    annotations: {}
    # -- Server service labels
    labels: {}
    # -- Server service type
    type: LoadBalancer
```
```
helm install ic argo/argo-cd -f my-argo-cd-values.yaml
```
Username: admin        Password: admin

Configure argocd cli
```shell
chmod +x argocd
mv argocd /usr/local/bin/
argocd login 192.168.56.50
argocd app list
argocd cluster list
argocd app create app \
--repo https://github.com/abhi-aws15/Argo.git \
--path app \
--dest-namespace app \
--dest-server https://kubernetes.default.svc
```
yaml file will look like
```yaml
apiVersion: argoproj.io/vlalpha1 
kind: Application 
metadata: 
  name: app 
  namespace: default
spec: 
  project: default 
  source: 
    repoURL: https://github.com/abhi-aws15/Argo.git 
    targetRevision: HEAD 
    path: app 
  destination: 
    server: https://kubernetes.default.svc 
    namespace: app 
  syncPolicy: 
    automated: 
      selfHeal: true 
    syncoptions: 
      CreateNamespace=true
```

