<<<<<<< HEAD
# observability-zero-to-hero

https://github.com/iam-veeramalla/observability-zero-to-hero
https://www.youtube.com/watch?v=otY2_M_pTmU&list=PLdpzxOOAlwvJUIfwmmVDoPYqXXUNbdBmb&index=2


## INSTALALTION K8S - PROMETHEUS, GRAFANA, ALERTMANAGER
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
```bash
kubectl create ns monitoring
```
```bash
vim custom_kube_prometheus_stack.yml
```
```bash
alertmanager:
  alertmanagerSpec:
    # Selects Alertmanager configuration based on these labels. Ensure that the Alertmanager configuration has matching labels.
    # ✅ Solves error: Misconfigured Alertmanager selectors can lead to missing alert configurations.
    # ✅ Solves error: Alertmanager wasn't able to findout the applied CRD (kind: Alertmanagerconfig)
    alertmanagerConfigSelector:
      matchLabels:
        release: monitoring

    # Sets the number of Alertmanager replicas to 3 for high availability.
    # ✅ Solves error: Single replica can cause alerting issues during pod failures.
    # ✅ Solves error: Alertmanager Cluster Status is Disabled (GitHub issue)
    replicas: 2

    # Sets the strategy for matching Alertmanager configurations. 'None' means no specific matching strategy.
    # ✅ Solves error: Incorrect matcher strategy can lead to unhandled alert configurations.
    # ✅ Solves error: Get rid of namespace matchers when creating AlertManagerConfig (GitHub issue)
    alertmanagerConfigMatcherStrategy:
      type: None
```
```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
-n monitoring \
-f ./custom_kube_prometheus_stack.yml
```
```bash
kubectl get all -n monitoring
kubectl apply -f prometheus-ingress.yaml
kubectl apply -f grafana-ingress.yaml
kubectl apply -f alertmanager-ingress.yaml
```
```bash
sudo vim /etc/hosts
```
```bash
<k8s-ip> prometheus.local
<k8s-ip> grafana.local
<k8s-ip> alertmanager.local
```
- http://prometheus.local:32437
- http://grafana.local:32437
- http://alertmanager.local:32437
```bash
vim /etc/hosts - on the local computer, not on the k8s-master
```

Grafana Login:
        - kubectl get secret -n monitoring monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 --decode && echo
                - admin
                - password
=======
# prometheus-Grafana-Zero-to-Hero
[WIP]: Repo for learning how monitor your kubernetes clusters using prometheus and visualize using grafana
>>>>>>> 4bc459a (Initial commit)
