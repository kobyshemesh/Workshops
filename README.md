# Workshops
This is sample app inspired from: https://kubernetes.io/docs/tutorials/stateless-application/guestbook/
It is used to perform PKS workshops.

**Login to linux machine via ssh.**
make sure you have pksapi, kubectl and docker installed.

**vi env.sh**
**i have used GCP, you can condigure it what ever env you want**
PKS_API_URL=https://api.pks.XXX.com
GCP_REGION=europe-west1
PKS_CLI_USERNAME=XXX
GCP_PROJECT=XXX
PKS_CLI_PASSWORD=XXX
CLUSTER_NAME=XXX
PREFIX=XXX
**Keep the file**
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
bosh -e pks ssh -d service-instance_XXX master/XXX
etcdctl cluster-health
etcdctl member list
**from the master node:**
  monit summary

**pull docker images from remote repo**
mkdir /etc/docker/certs.d/harbor.XXX.com
cd /etc/docker/certs.d/harbor.XXX.com
vim ca.crt


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



