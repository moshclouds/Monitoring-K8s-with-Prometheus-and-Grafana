# 🚀 Monitoring Kubernetes with Prometheus and Grafana on Minikube

## 🔥 Introduction

Kubernetes is a powerful orchestration tool, but monitoring its performance is crucial for ensuring smooth operations. This is where **Prometheus** and **Grafana** come into play. Prometheus is a leading open-source monitoring solution, while Grafana is a visualization tool that helps make sense of the collected data.

In this guide, we’ll walk through setting up **Prometheus and Grafana on Minikube**, a local Kubernetes cluster, to monitor and visualize cluster metrics. This tutorial is **beginner-friendly** and will help you get hands-on experience with Kubernetes monitoring.

---

## ✅ Prerequisites

Before we start, ensure you have the following installed:

- **Minikube** ([Install Guide](https://minikube.sigs.k8s.io/docs/start/))
- **kubectl** ([Install Guide](https://kubernetes.io/docs/tasks/tools/))
- **Helm** ([Install Guide](https://helm.sh/docs/intro/install/))

To verify Minikube is installed, run:
```sh
minikube version
```

---

## ⚙️ Step 1: Start Minikube

First, start your Minikube cluster:
```sh
minikube start
```
Check the status:
```sh
minikube status
```

---

## 📦 Step 2: Create a Namespace for Monitoring

It's a good practice to keep monitoring components in a separate namespace.
```sh
kubectl create namespace monitoring
```

---

## 📡 Step 3: Deploy Prometheus

1️⃣ Add the **Prometheus Helm repository**:
```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

2️⃣ Install **Prometheus**:
```sh
helm install prometheus prometheus-community/prometheus -n monitoring
```

3️⃣ Verify the installation:
```sh
kubectl get pods -n monitoring
```

4️⃣ Check the running ports of the Prometheus pod:
```sh
kubectl describe pod <your-prometheus-pod-name> -n monitoring
```

5️⃣ Expose the Prometheus service:
```sh
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-exp -n monitoring
```

6️⃣ Verify the exposed service:
```sh
kubectl get svc -n monitoring
```

7️⃣ Access Prometheus in the browser:
```sh
minikube service prometheus-server-exp -n monitoring
```
<img width="1918" alt="Image" src="https://github.com/user-attachments/assets/422a4d43-10eb-4fcd-88e9-0134c71259a1" />

---

## 📊 Step 4: Deploy Grafana

1️⃣ Add the **Grafana Helm repository**:
```sh
helm repo add grafana https://grafana.github.io/helm-charts
```

2️⃣ Install **Grafana**:
```sh
helm install grafana grafana/grafana -n monitoring
```

3️⃣ Get the default Grafana admin password:
```sh
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}"
```

🔑 Decode the base64-encoded password using a third-party tool or command-line utility.


<img width="902" alt="Image" src="https://github.com/user-attachments/assets/82eef060-83f6-4789-bad0-536f7e12ae95" />

<br>

4️⃣ Check the running ports of the Grafana pod:
```sh
kubectl describe pod <your-grafana-pod-name> -n monitoring
```

5️⃣ Expose the Grafana service:
```sh
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-exp -n monitoring
```

6️⃣ Access Grafana in the browser:
```sh
minikube service grafana-exp -n monitoring
```

<img width="1921" alt="Image" src="https://github.com/user-attachments/assets/91df4e79-4d39-435e-bdae-c7d59910b573" />

<br>

---

## 🛠️ Step 5: Add Prometheus as a Data Source in Grafana

1️⃣ In Grafana, navigate to **Settings → Data Sources → Add Data Source**. 

<img width="1901" alt="Image" src="https://github.com/user-attachments/assets/cd93115a-556b-4897-9c3d-e27ad5599baa" />

<br>
2️⃣ Select **Prometheus**. 

<img width="1904" alt="Image" src="https://github.com/user-attachments/assets/3214410d-3b16-4799-9bdb-95881860128d" />

<br>
3️⃣ Use the following URL:

   ```
   http://<your-minikube-ip>:<grafana-port-address>
   ```
<img width="1240" alt="Image" src="https://github.com/user-attachments/assets/6b26ec27-cb47-4eae-b1f2-27bf57e2ae29" />

<br>

4️⃣ Click **Save & Test** to confirm the connection.

<img width="1035" alt="Image" src="https://github.com/user-attachments/assets/93a257f6-1f52-4df7-862a-33a1e2f285e0" />

<br>

---

## 📈 Step 6: Import Kubernetes Dashboards in Grafana

Grafana provides pre-built dashboards for Kubernetes monitoring:

1️⃣ Go to **Dashboards → Import**. 

<img width="1917" alt="Image" src="https://github.com/user-attachments/assets/c4e8f36b-f692-4a4e-b912-9fc802cd5a33" />

<br>

2️⃣ Enter **Dashboard ID:** `3662`.

<img width="879" alt="Image" src="https://github.com/user-attachments/assets/c0660269-945f-4a11-adeb-0d2111838085" /> <br>

3️⃣ Select the Prometheus data source and click **Import**. 

<img width="1126" alt="Image" src="https://github.com/user-attachments/assets/ef9ae90d-0633-4b52-8a60-bcf3475e3079" />

<br>

You should now see real-time metrics from your Minikube cluster! As shown below🎉

<img width="1902" alt="Image" src="https://github.com/user-attachments/assets/100b1f3b-a6b3-43d9-a487-42cdb71deed7" />

<br>

---



## 🎯 Conclusion

Congratulations! You have successfully set up **Prometheus and Grafana on Minikube**. You can now:

- Monitor **CPU, memory, and network usage**.
- Visualize real-time metrics with **Grafana dashboards**.
- Set up **alerts** using Alertmanager.

To further improve your monitoring setup, consider configuring **Alertmanager** for notifications and exploring **custom dashboards**.

🚀 **Next Steps**:
- Learn more about **Prometheus Query Language (PromQL)**.
- Customize Grafana dashboards to fit your needs.
- Set up monitoring for real-world Kubernetes clusters using **kube-prometheus**.

---

🔗 Useful Links:
- [Prometheus Docs](https://prometheus.io/docs/)
- [Grafana Docs](https://grafana.com/docs/)
- [Helm Charts](https://github.com/prometheus-community/helm-charts)

