### === PI BACKEND SECTION === 

RKE INSTALL HAS BEEN DONE ON Raspbian 11:::
INSTALL DOCKER:::
> DONT SNAP INSTALL DOCKER

(((https://stackoverflow.com/questions/52526219/docker-mkdir-read-only-file-system))) \
sudo usermod -aG docker pi (((THEN RESTART PI))) \
chmod +x rke_linux-arm64 (((COPY FILE ACROSS FIRST USING WINSCP WITH SIMPLE CLUSTER.YML TOO))) \
"./rke_linux-arm64 up" but prerequisite with ssh login without pass by running::: \
ssh-keygen \
ssh-copy-id -i /home/pi/.ssh/id_rsa -p 22 pi@192.168.50.89

INSTALL KUBECTL::: \
mkdir .kube \
cp kube_config_cluster.yml .kube/config \

INSTALL HELM PLUS CERT MANAGER AND THEN RANCHER BOTH THROUGH HELM USE SNAP::: \
(((https://helm.sh/docs/intro/install/))) \
helm repo add jetstack https://charts.jetstack.io \
helm repo update \
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.10.1/cert-manager.crds.yaml

helm install cert-manager jetstack/cert-manager \ \
  --namespace cert-manager \ \
  --create-namespace \ \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \ \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \ \
  --set ingressShim.defaultIssuerGroup=cert-manager.io \ \
  --version v1.10.1

RANCHER INSTALL::: \
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable \
helm repo update \
helm install rancher rancher-stable/rancher \ \
  --namespace cattle-system \ \
  --create-namespace \ \
  --set hostname=rancher.junder.app \ \
  --set replicas=1 \ \
  --set ingress.tls.source=letsEncrypt \ \
  --set letsEncrypt.email=emailgoeshere \ \
  --set letsEncrypt.ingress.class=nginx

kubectl -n cattle-system rollout status deploy/rancher

UNINSTALL:::

./rke remove \
docker stop $(docker ps -aq) \
docker rm $(docker ps -aq) \
docker system prune \
docker volume prune

### === PI USEFUL COMMANDS ===

kubectl apply -f paintapp || kubectl delete -f paintapp \
kubectl logs deployment/paintapp # logs of deployment \
kubectl logs -f deployment/zlurby # follow logs \
kubectl rollout restart deployments/paintapp \
kubectl exec -it deployment/paintapp -- /bin/sh \
kubectl get service/nginx-service -o jsonpath='{.spec.clusterIP}' \
curl clusterip || ingressip \
kubectl describe ing nginx-junder \
kubectl get ingress \
kubectl cluster-info \
curl -vLk https://127.0.0.1 \
kubectl get endpoints

### === DNS === 

(((look into nagios))) \
(((https://www.suse.com/support/kb/doc/?id=000020174))) \
kubectl run -i --tty test --image=alpine:3.8 --restart=Never -- sh \
cat /etc/resolv.conf

### === SOME INGRESS ANOTATIONS ===

kubernetes.io/ingress.class: haproxy \
cert-manager.io/cluster-issuer: letsencrypt-prod \
ingress.kubernetes.io/whitelist-source-range: "0.0.0.0" \
kubernetes.io/ingress.allow-http: "false" \
ingress.kubernetes.io/ssl-passthrough: "true"

### === RANCHER CLI ===

(((https://github.com/rancher/cli/releases))) \
chmod +x rancher \
./rancher login https://rancher.junder.ddns.net --token <BEARER_TOKEN>
