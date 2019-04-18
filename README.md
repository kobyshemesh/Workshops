# Workshops
This is sample app inspired from: https://kubernetes.io/docs/tutorials/stateless-application/guestbook/
It is used to perform PKS workshops.

**Login to linux machine via ssh.**
make sure you have pksapi, kubectl and docker installed.

**vi env.sh**

PKS_API_URL=https://api.pks.livingston.cf-app.com
GCP_REGION=europe-west1
PKS_CLI_USERNAME=kobi
GCP_PROJECT=pa-kshamama
PKS_CLI_PASSWORD=TBD
CLUSTER_NAME=workshop
PREFIX=livingston
source env.sh

pks login -k -a ${PKS_API_URL} -u ${PKS_CLI_USERNAME} -p ${PKS_CLI_PASSWORD} --skip-ssl-validation

pks plans
pks clusters
pks cluster ${CLUSTER_NAME}

**Dont do:
pks create-cluster ${CLUSTER_NAME} -e ${MASTER_EXTERNAL_IP} -p small

#Creating K8S from scratch
https://kubernetes.io/docs/setup/scratch/

pks get-credentials ${CLUSTER_NAME}
KUBECONFIG=myconfig.config_workshop pks get-credentials ${CLUSTER_NAME}
kubectl cluster-info
kubectl get nodes
**kill one node before resize**
pks resize ${CLUSTER_NAME} --num-nodes 4
bosh -e pks vms
bosh -e pks tasks
bosh -e pks  task
bosh -e pks ssh -d service-instance_bd8ce083-9b4d-41e5-b76c-627f07b49443 master/8b65ee5a-7b59-4de9-b886-f9025fcebeb5
etcdctl cluster-health
etcdctl member list
**from the master node:**
  monit summary

**pull docker images from remote repo**
mkdir /etc/docker/certs.d/harbor.livingston.cf-app.com
cd /etc/docker/certs.d/harbor.livingston.cf-app.com
vim ca.crt
**copy paste**
-----BEGIN CERTIFICATE-----
MIIDUTCCAjmgAwIBAgIVAMeZe40FBlLJovyOrrG2JVhSCPW8MA0GCSqGSIb3DQEB
CwUAMB8xCzAJBgNVBAYTAlVTMRAwDgYDVQQKDAdQaXZvdGFsMB4XDTE5MDQwNTE4
MTQzMVoXDTIzMDQwNjE4MTQzMVowHzELMAkGA1UEBhMCVVMxEDAOBgNVBAoMB1Bp
dm90YWwwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDMZtC9OdVnSlPa
FmlT0J6WIfGUG+cCgjehWbY6Azm7d/3Q36HhtcWAxsR8+R1cBFSxM75AyyCQeIKm
2WFiLKVrie+X2299r0boxOhCqOfxQPrEb5p4CaV0bHwqG9v7FVbYChuYPOnqp649
kb9d7TIkzYsvXCzjIh0Nn8gFEUYIuWsxz0/1YkZv87ha8CP8dei0pMyECdTBpX4/
r31QCD7YDRL9Wbmq5rZQIjrZ+4RYw/wpgbQkLx437R+ZkBNgiEqMbniUllLjk3ib
2sh4AvOp0TqkJGSq/9PfmfIyF2IIVsT/+xyGrhw/xJqs6xVvYInQ83iIUtrqFX99
z1+oAHsVAgMBAAGjgYMwgYAwHQYDVR0OBBYEFItd2xGLuWsfKZa7bG0MWfxuCuFJ
MB8GA1UdIwQYMBaAFItd2xGLuWsfKZa7bG0MWfxuCuFJMB0GA1UdJQQWMBQGCCsG
AQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MA4GA1UdDwEB/wQEAwIB
BjANBgkqhkiG9w0BAQsFAAOCAQEAgZoyplix47WBmTYTiqv7ztS2DpuX8lGTGojY
Q7MqkZw1a1IYwU2kYClDhaZWrAMamPPJEaEwG/0/t4Ix7bM54C8yLmzBXET4tgB8
l+8wIWpqRcRYZLdq0IxtDcNkmpgr57wMkVw8mWxzupO8btjrx4LG0nxJEJtFzRSQ
WxNjOAtJt4jNYhSmi97RX4wgO3fUz3mrFak3RLrGBn9Q0w3ieAAcTOj9wDP+7ayT
qSn1jZ1OvPG3Lzi/68TFeZufG+fVMP0exP/xq5s5mz/Uk6hIUPFWcWrded6+Oe2u
4xGKG2OLGmA7pAwx78VtzhL1khXZtgI170p4k+dNdanL8qzugQ==
-----END CERTIFICATE-----




docker pull k8s.gcr.io/redis:e2e
docker pull gcr.io/google_samples/gb-redisslave:v1
docker pull gcr.io/google-samples/gb-frontend:v4
docker images
docker login harbor.livingston.cf-app.com
docker tag gcr.io/google-samples/gb-frontend:v4 harbor.livingston.cf-app.com/workshop/gb-frontend:**XX**
docker tag gcr.io/google_samples/gb-redisslave:v1 harbor.livingston.cf-app.com/workshop/gb-redisslave:**XX**
docker tag k8s.gcr.io/redis:e2e harbor.livingston.cf-app.com/workshop/redis:**XX**

docker push harbor.livingston.cf-app.com/workshop/gb-frontend:**XX**
docker push harbor.livingston.cf-app.com/workshop/gb-redisslave:**XX**
docker push harbor.livingston.cf-app.com/workshop/redis:**XX**

In order to deploy the app, perform the follwing:
git clone https://github.com/kobyshemesh/Workshops

create ns:
kubectl create ns workshop

kubectl apply -f redis-master-deployment.yaml --namespace=workshop
kubectl get pods
kubectl apply -f redis-master-service.yaml --namespace=workshop
kubectl get service
kubectl apply -f redis-slave-deployment.yaml --namespace=workshop
kubectl get pods
kubectl apply -f redis-slave-service.yaml --namespace=workshop
kubectl get services
kubectl apply -f frontend-deployment.yaml --namespace=workshop
kubectl get pods -l app=guestbook -l tier=frontend
kubectl apply -f frontend-service.yaml --namespace=workshop
kubectl get services
kubectl get service frontend

or 
kubectl apply -f .

kubectl expose deployment frontend --type=LoadBalancer --namespace=workshop
kubectl get service frontend
kubectl scale deployment frontend --replicas=5
kubectl get pods


**working with storage**
https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/#visit-your-new-wordpress-blog

kubectl get StorageClass

kubectl create secret generic mysql-pass --from-literal=password=password
kubectl get secrets

The following manifest describes a single-instance MySQL Deployment. The MySQL container mounts the PersistentVolume at /var/lib/mysql. The MYSQL_ROOT_PASSWORD environment variable sets the database password from the Secret.

kubectl create -f mysql-deployment.yaml
kubectl get pods
kubectl create -f wordpress-deployment.yaml
kubectl get pvc



