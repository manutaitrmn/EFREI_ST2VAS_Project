# EFREI_ST2VAS_Project
A vagrant script to build a pair of Ubuntu based VMs (master and worker) with MicroK8s and Docker installed in order to experiment with Kubernetes

### Check status
![Check status](img/1.png)


### Add worker node
![](img/2.png)
![](img/3.png)

### Check nodes from master
![Check nodes](img/4.png)

### Enable dashboard and dns
![dashboard](img/5.png)
![dashboard](img/6.png)

### Get the access token for the k8s dashboard
![dashboard](img/7.png)
![dashboard](img/8.png)

# Prometheus
Prometheus is enabled during the installation of the K8s cluster.
Then this manifests is applied to expose the solution.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-np
  namespace: monitoring
spec:
  ports:
  - nodePort: 30090
    port: 9090
    protocol: TCP
    targetPort: 9090
  selector:
    app.kubernetes.io/component: prometheus
    app.kubernetes.io/name: prometheus
    app.kubernetes.io/part-of: kube-prometheus
  type: NodePort
```
![dashboard](img/prometheus.png)

# Grafana
Grafana is enabled during the installation of the K8s cluster.
Then this manifests is applied to expose the solution.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-np
  namespace: monitoring
spec:
  ports:
  - nodePort: 30090
    port: 9090
    protocol: TCP
    targetPort: 9090
  selector:
    app.kubernetes.io/component: prometheus
    app.kubernetes.io/name: prometheus
    app.kubernetes.io/part-of: kube-prometheus
  type: NodePort
```
![dashboard](img/grafana.png)