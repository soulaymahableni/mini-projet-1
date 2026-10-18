# Mini-projet 1 : application web à deux niveaux

Frontend (nginx, 2 réplicas) et API (podinfo, 3 réplicas) sur k3s,
exposés par un Ingress Traefik (web.k3s.local et api.k3s.local).

## Déploiement
    k apply -f manifests/
    k get pods -o wide --show-labels
    k get svc,endpoints
    k get ingress

## Accès (remplacer <IP> par l'adresse d'un nœud)
    curl -H "Host: web.k3s.local" http://<IP>
    curl -H "Host: api.k3s.local" http://<IP>

## Contenu de manifests/
- 00-namespace.yaml : namespace du binôme
- 10-api-deployment.yaml : Deployment de l'API (3 réplicas, labels app et tier)
- 20-frontend-deployment.yaml : Deployment du frontend (2 réplicas)
- 30-services.yaml : Services ClusterIP api-svc et frontend-svc
- 40-ingress.yaml : Ingress (deux hôtes)

## Mise à jour et retour arrière
Modifier l'image dans 10-api-deployment.yaml, puis :
    k apply -f manifests/10-api-deployment.yaml
    k rollout status deployment/api
    k rollout undo deployment/api
