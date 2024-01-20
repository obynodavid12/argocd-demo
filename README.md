# https://github.com/lerndevops/educka/tree/master/ArgoCD  --Updated in the repository
# https://github.com/lerndevops/kubernetes/blob/master/advanced/git-ops/ArgoCD/README.md
# https://argo-cd.readthedocs.io/en/stable/getting_started/
# https://github.com/argoproj/argo-cd/releases/tag/v2.8.9

ARGOCD UI

Argo CD is a tool which will read your environment configuration (written either as a helm chart, kustomize files, jsonnet or plain yaml files) from your git 
repository and apply it to your Kubernetes namespaces.

ubuntu@devops101:~/argocd-demo# kubectl create namespace argocd 

ubuntu@devops101:~/argocd-demo/argocd/argo# kubectl apply -n argocd -f install.yaml

ubuntu@devops101:~/argocd-demo/argocd/argo# kubectl apply -n argocd -f argocd-app.yaml
application.argoproj.io/lab-app configured

ubuntu@devops101:~/argocd-demo/argocd# kubectl get svc -n argocd

#Execute the Port Forward on another terminal
kubectl port-forward svc/argocd-server -n argocd 8080:443 --(didnt even need to change loadbalancer to NodePort)-See document. #https://argo-cd.readthedocs.io/en/stable/getting_started/

To access the Argo CD UI, Go to browser and open localhost:8080 then click on Advanced and proceed 
Enter Username: admin and Password: fetch password with this command "kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d"
See all applications running and even from the terminal
