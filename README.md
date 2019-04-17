# Workshops
This is sample app inspired from: https://kubernetes.io/docs/tutorials/stateless-application/guestbook/
It is used to perform PKS workshops.

In order to deploy the app, perform the follwing:
git clone https://github.com/kobyshemesh/Workshops

kubectl apply -f redis-master-deployment.yaml
kubectl apply -f redis-master-service.yaml
kubectl apply -f redis-slave-deployment.yaml
kubectl apply -f redis-slave-service.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
