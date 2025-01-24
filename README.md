# K8s-Dashboard
Created this dashboard to monitor real time cost and individual Kubernetes resources (cpu-memory)
 Kubecost Dashboard Installation
Overview
Kubecost provides real-time cost monitoring and optimization for Kubernetes clusters, enabling granular cost allocation and better management of infrastructure expenses.
Installation Steps
1. Add the Helm Repository:
helm repo add kubecost https://kubecost.github.io/cost-analyzer/
helm repo update
2. Create a Custom values.yaml File: Customize the values.yaml file based on your requirements. For example:
global:
  prometheus:
    enabled: true
kubecostProductConfigs:
  clusterName: "devworld"
3. Install Kubecost Using Helm:
helm upgrade --install kubecost kubecost/cost-analyzer \
  --namespace kubecost --create-namespace \
  -f values.yaml
4. Configure Ingress: Ensure your ingress resource is configured for Kubecost. Example ingress:
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: kubecost-ingress
  namespace: kubecost
  annotations:
    kubernetes.io/ingress.class: "ALB"
spec:
  rules:
    - host: kubecost.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: kubecost-cost-analyzer
                port:
                  number: 9003
5. Verify Installation:
 Run the following command to verify that the pods are running:
 kubectl get pods -n kubecost
 Access the dashboard via the ingress URL.