# Kubernetes NGINX Ingress Demo

NGINX Ingress Controller demo with HPA on Minikube.

> **Legal Notice:** This project was developed for [DataBird](https://data-bird.co).
> Copyright © 2025 DataBird. All rights reserved.

## Prerequisites

- Minikube installed
- `kubectl` installed

## Quick Start

```bash
minikube start

kubectl apply -f demo-namespace.yaml
kubectl apply -f ingress-namespace.yaml

kubectl apply -f ingress-controller/

kubectl apply -f app/
```

## What's Included

- **Ingress Controller**: NGINX Ingress Controller with RBAC
- **Sample App**: Google's hello-app (3 replicas)
- **HPA**: Horizontal Pod Autoscaler for auto-scaling
- **Ingress**: HTTP routing configuration

## Verify Deployment

```bash
kubectl get pods -n ingress-nginx
kubectl get pods -n demo
kubectl get ingress -n demo
```

## Test HPA Autoscaling

Generate load to trigger HPA:

```bash
kubectl run -it --rm load --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://hello-service.demo.svc.cluster.local; done"
```

Watch HPA scale the pods:

```bash
kubectl get hpa -n demo --watch
```

## Cleanup

```bash
kubectl delete -f app/
kubectl delete -f ingress-controller/
kubectl delete -f ingress-namespace.yaml
kubectl delete -f demo-namespace.yaml

minikube stop
```

## License

See [LICENSE](LICENSE)
