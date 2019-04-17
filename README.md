# Workshops
This is sample app inspired from: https://kubernetes.io/docs/tutorials/stateless-application/guestbook/
It is used to perform PKS workshops.

In order to deploy the app, perform the follwing:
git clone ....

kubectl apply -f redis-master-deployment.yaml
kubectl apply -f redis-master-service.yaml
kubectl apply -f https://k8s.io/examples/application/guestbook/redis-slave-deployment.yaml
kubectl apply -f https://k8s.io/examples/application/guestbook/redis-slave-service.yaml
kubectl apply -f https://k8s.io/examples/application/guestbook/frontend-deployment.yaml
kubectl apply -f https://k8s.io/examples/application/guestbook/frontend-service.yaml
