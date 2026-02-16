# canary-deployment-cilium-prometheus

2️⃣ Accès au service pour tests
Test interne cluster (pas besoin d’Ingress)
kubectl run -i --tty curlpod --image=radial/busyboxplus:curl --restart=Never -- sh
Puis dans le pod :

for i in $(seq 1 20); do curl -s http://myapp; echo; done


resultat souhaité 90 et 10%
> compter la répartition :

for i in $(seq 1 50); do curl -s http://myapp; done | sort | uniq -c
