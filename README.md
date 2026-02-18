

# Ajouter le repo helm pour ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# Installer NGINX ingress controller
helm install nginx-ingress ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace \
  --set controller.publishService.enabled=true
Vérifie que le IngressClass est créé :

bash
Copier le code
kubectl get ingressclass
2️⃣ Installer Cilium (CNI & Service Mesh optionnel)
bash
Copier le code
# Installer Cilium CLI si pas déjà
curl -L --remote-name https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz
tar xzvf cilium-linux-amd64.tar.gz
sudo mv cilium /usr/local/bin

# Installer Cilium sur le cluster
cilium install

# Vérifier que tous les pods Cilium sont running
kubectl -n kube-system get pods -l k8s-app=cilium
Vérifier CRDs installées par Cilium :

bash
Copier le code
kubectl get crds | grep cilium
3️⃣ Installer Gateway API (CRDs modernes)
bash
Copier le code
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.9.1/standard-install.yaml
kubectl get crds | grep gateway

# crds for gateway api 
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.4.1/standard-install.yaml

# canary-deployment-cilium-prometheus

2️⃣ Accès au service pour tests
Test interne cluster (pas besoin d’Ingress)
kubectl run -i --tty curlpod --image=radial/busyboxplus:curl --restart=Never -- sh
Puis dans le pod :

for i in $(seq 1 20); do curl -s http://myapp; echo; done


resultat souhaité 90 et 10%
> compter la répartition :

for i in $(seq 1 50); do curl -s http://myapp; done | sort | uniq -c
