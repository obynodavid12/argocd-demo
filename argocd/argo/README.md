if we enable autosync will it rollback changes made by hpa ?? Like if we increase the replicas using hpa , will argocd sync it back to what it has in deployment file

As far as I know yes, it will change it back. If I'm not mistaken they (Argo people) are working on that. You can remove the number of replicas from your deployment as a workaround and let HPA deal with it