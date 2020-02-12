# Guestbook Kubernetes Deployment

This manifest provides a simple deployment of the Guestbook application for testing and validtion of your Kubernetes install.  It uses individual YAML files to deploy the namespace, storageclass, persistent volume claims and application components.  You then have an option to expose the Frontend application services using ingress or service type loadbalancer.  

Choose only one option to expose the Frontend service (i.e either guestbook-frontend-http-ingress.yaml, guestbook-frontend-https-ingress.yaml (requires you create guestbook-secret of time) or guestbook-frontend-svc-lb.yaml)
