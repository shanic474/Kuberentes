# Kuberentes
Argo CD installation instructions:
# Step 1: Install Argo CD in Kubernetes
kubectl create namespace argocd && \
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Step 2: Expose Argo CD Server as NodePort
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

# Step 3: Get the Argo CD Admin Password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Step 4: Get the NodePort (Replace <NODE_PORT> with the actual port number)
kubectl get svc argocd-server -n argocd
echo "Access Argo CD UI at https://<VM_IP>:<NODE_PORT>"

# Step 5: Login to Argo CD CLI
argocd login <VM_IP>:<NODE_PORT> --username admin --password <password> --insecure


Create a deployment in Argo CD using the command line
# Step 1: Create an Application in Argo CD
argocd app create nginx-app \
  --repo https://github.com/YOUR_USERNAME/YOUR_REPO.git \
  --path . \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default \
  --project default

# Step 2: Sync the Application to Deploy it
argocd app sync nginx-app
