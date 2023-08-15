# Ingress configuration of frontend-backend web application Cluster with correct HTTP header rewrites
Configure cluster for web application with frontend and backend on difrerent addresses: 
- frontend on `/`,
- backend on `/backend`.

 Using such configuration ensures `Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html"` won't appear since we rewrite all requests correctly.
 
 This problem is discussed in Ticket **Ingress changes original content type #5265** [^1].
 
`ingress-config.yaml`
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: application-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - host: test.application.com
    http:
      paths:
      - path: /?(.*)
        pathType: Prefix
        backend:
          service:
            name: application-frontend-service
            port:
              number: 80   
      - path: /backend/?(.*)
        pathType: Prefix
        backend:
          service:
            name: application-backend-service
            port:
              number: 80
```

[^1]: https://github.com/kubernetes/ingress-nginx/issues/5265