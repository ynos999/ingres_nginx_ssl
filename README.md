# Nginx ingress controller and free ssl.

1. Apply Nginx ingress:
### kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.1/deploy/static/provider/cloud/deploy.yaml
### 
### kubectl get pods -n ingress-nginx
### kubectl get svc -n ingress-nginx
### kubectl -n ingress-nginx get pods
### 
2. Deploy app whoami:
### 
### kubectl apply -f whoami.yaml
### 
3. Apply Cert-Manager:
### 
### kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.17.0/cert-manager.yaml
### 
4. Create a ClusterIssuer for Let’s Encrypt.
### 
### nano cluster-issuer.yaml and edit email: your_mail@your_domain.com
### 
### kubectl apply -f cluster-issuer.yaml
### 
### kubectl get clusterissuer
### kubectl describe clusterissuer letsencrypt-prod
### 
5. Create A record for your domain name whoami.your_domain.com
### 
### 
6. If use k3s, You need delete traefik. If don't use, skip.
### 
### 
### kubectl delete -f /var/lib/rancher/k3s/server/manifests/traefik.yaml
### 
7. Apply ingress.yaml
### 
### Edit hosts:
### 
###   tls:
###   - hosts:
###     - whoami.your_domain.com
###     secretName: whoami-tls
###   rules:
###   - host: whoami.your_domain.com
### 
### kubectl apply -f ingress.yaml
### kubectl get svc -n ingress-nginx
### 
8. Verify Certificate Creation
### 
### kubectl get certificates
### kubectl describe certificate whoami-tls
### kubectl describe order
### kubectl get secrets
### kubectl get secret whoami-tls
### kubectl -n default get secret
### kubectl -n default describe secret whoami-tls
### kubectl get svc whoami
### kubectl get pods -A
### kubectl describe challenge -A
### kubectl get pods -n cert-manager
### kubectl logs -n cert-manager deploy/cert-manager
### curl -v https://whoami.your_domain.com