# EFREI_ST2VAS_Project
A vagrant script to build a pair of Ubuntu based VMs (master and worker) with MicroK8s and Docker installed in order to experiment with Kubernetes

# Prerequisites
 - Vagrant
 - Virtualbox

# Execution steps

## Start and create the K8s cluster
```bash
vagrant up
```
## Open ports for the k8s dashboard
```bash
vagrant ssh master
setsid /vagrant/dashboard.sh >/dev/null 2>&1 < /dev/null &
```
## Dashboards
- k8s dashboard: [](10.0.0.100:10443)
- Prometheus: [](10.0.0.100:30090)
- Grafana: [](10.0.0.100:30000)
