# https://github.com/lerndevops/educka/tree/master/ArgoCD  --Updated in the repository
# https://github.com/lerndevops/kubernetes/blob/master/advanced/git-ops/ArgoCD/README.md
# https://argo-cd.readthedocs.io/en/stable/getting_started/
# https://github.com/argoproj/argo-cd/releases/tag/v2.8.9
# https://argo-cd.readthedocs.io/en/stable/faq/

ArgoCD polls Git repository every 3 minutes for changes

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

# If using the above port-forward command then patch it to either use NodePort or LoadBalancer -https://stackoverflow.com/questions/67262769/access-argocd-server?rq=3
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}' 
https://<hosted-node-ip>:<NodePort>
OR
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
argoPass=$(kubectl -n argocd get secret argocd-initial-admin-secret \
    -o jsonpath="{.data.password}" | base64 -d)

argocd login --insecure --grpc-web kubemaster:32761 --username admin \
    --password $argoPass


# DELETING ARGOCD APP SETUP STUCK IN DELETION WHEN I TRIED DELETING IT ON AGOCD UI
kubectl -n argocd get application -o jsonpath='{range.items[?(@.status.sync.status=="Unknown")]}{.metadata.name}{"\n"}{end}' | xargs -I {} kubectl -n argocd patch application {} --type=json -p='[{"op":"remove", "path": "/metadata/finalizers"}]'
application.argoproj.io/lab-app patched

OR

kubectl -n argocd get applications -o=jsonpath='{range .items[?(@.status.health.status=="Unknown")]}{.metadata.name}{"\n"}'| xargs -I {} kubectl patch application {} --type=json -p='[{"op":"remove", "path": "/metadata/finalizers"}]'

This code will remove finalizers field for all argocd applications in Unknown status.

Of course you can modify the filter.

# Log in to kind-control-plane node
==>docker exec -it kind-control-plane bash

# Listing the servers created
grep server ~/.kube/config
server: https://0.0.0.0:38247
server: https://127.0.0.1:43481

kubectl config get-contexts

kubectl get nodes --context kind-kind
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   61m   v1.30.0
kind-worker          Ready    <none>          60m   v1.30.0
kind-worker2         Ready    <none>          60m   v1.30.0
kind-worker3         Ready    <none>          60m   v1.30.0

# USE CONTEXT
kubectl config use-context kind-kind
Switched to context "kind-kind".

# GET CLUSTER
kind get clusters
kind

# DELETE CLUSTER
kind delete cluster --name kind
kind delete cluster --name mycluster

docker pull kindest/haproxy
Using default tag: latest
Error response from daemon: manifest for kindest/haproxy:latest not found: manifest unknown: manifest unknown
devops_cloud@obinayah:~/argocd-demo/argocd/argo$ docker pull kindest/haproxy:v20230606-42a2262b
v20230606-42a2262b: Pulling from kindest/haproxy
a7ca0d9ba68f: Pull complete 
fe5ca62666f0: Pull complete 
b02a7525f878: Pull complete 
fcb6f6d2c998: Pull complete 
e8c73c638ae9: Pull complete 
1e3d9b7d1452: Pull complete 
4aa0ea1413d3: Pull complete 
7c881f9ab25e: Pull complete 
5627a970d25e: Pull complete 
b9e44556168d: Pull complete 
3ec8085a1234: Pull complete 
Digest: sha256:001a06433666046dea44567c7d7c6adfc2ac0edb556576f6da507ff0b0f063d3
Status: Downloaded newer image for kindest/haproxy:v20230606-42a2262b
docker.io/kindest/haproxy:v20230606-42a2262b

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview kindest/haproxy:v20230606-42a2262b
devops_cloud@obinayah:~/argocd-demo/argocd/argo$ docker scout quickview kindest/haproxy:v20230606-42a2262b
    i New version 1.9.3 available (installed version is 1.8.0) at https://github.com/docker/scout-cli
    ✓ SBOM of image already cached, 38 packages indexed

  Target     │  kindest/haproxy:v20230606-42a2262b  │    0C     0H     0M     0L   
    digest   │  914852332446                        │                              
  Base image │  distroless/static:nonroot           │    0C     0H     0M     0L   

What's next:
    Include policy results in your quickview by supplying an organization → docker scout quickview kindest/haproxy:v20230606-42a2262b --org <organization>



# NANA DEMO 
https://gitlab.com/nanuchi/argocd-app-config
https://gitlab.com/nanuchi/argocd-app-config/-/tree/main/dev?ref_type=heads
https://gitlab.com/nanuchi/argocd-app-config/-/blob/main/README.md?ref_type=heads
# install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl -n argocd get pods,svc
kubectl port-forward svc/argocd-server -n argocd 8080:443

# access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

# login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# you can change and delete init password
