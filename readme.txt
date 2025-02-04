//start minkube
minikube start

//create namespace for monitoring
kubectl create namespace monitoring

//add and install prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus -n monitoring

//verify installaion by 
kubectl get pods

//for check the ports pod running
kubectl describe pod <your-promethues-pod-name> -n monitoring

//expose the prometheus service
kubectl expose service promethues-server --type=NodePort --target-port=9090 --name=prometheus-server-exp -n monitoring

//verify exposing of the service
kubectl get svc -n monitoring

//run exposed service using minikube service
minikube service prometheus-server-exp -n monitoring

//grafana installation
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana -n monitoring

// get grafana default password
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}"

//then decode encoded password using any 3rd party or something

//check the ports the grafana pod listening
kubectl describe pod <your-grafana-pod-name> -n monitoring
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-exp -n monitoring

//run exposed service using minikube service
minikube service grafana-exp -n monitoring