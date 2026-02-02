[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/GT-wEZl_)
# iut-stmalo-sae503
Application Python de gestion des citations du capitaine Haddock, utilisée dans le cadre de la SAÉ 5.03. Ce code est strictement destiné à des fins pédagogiques.

# Architecture du repo

```bash
├── citations-haddock-app.yml # manifeste Kubernetes
├── citations_haddock.py # application original
├── quotes # service quotes
│   ├── app.py
│   ├── Dockerfile # build l'image docker quotes
│   ├── initial_data_quotes.csv
│   └── requirements.txt
├── search # service search
│   ├── app.py
│   ├── Dockerfile # build de l'image docker search
│   └── requirements.txt
└── users # service users
    ├── app.py
    ├── Dockerfile # build de l'image docker users
    ├── initial_data_users.csv
    └── requirements.txt
```

# Shema d'architecture

```mermaid
graph TD
    %% Ingress
    USER[Client / Navigateur]
    USER -->|HTTP| TRAEFIK[Traefik IngressRoute]

    %% Routes
    TRAEFIK -->|/quotes| QUOTES_SVC[quotes-service]
    TRAEFIK -->|/search| SEARCH_SVC[search-service]
    TRAEFIK -->|/users| USERS_SVC[users-service]

    %% Services vers Pods
    QUOTES_SVC --> QUOTES_PODS[Quotes Pods x3]
    SEARCH_SVC --> SEARCH_PODS[Search Pods x3]
    USERS_SVC --> USERS_PODS[Users Pods x3]

    %% Redis
    QUOTES_PODS -->|TCP 6379| REDIS_SVC[redis Service]
    SEARCH_PODS -->|TCP 6379| REDIS_SVC
    USERS_PODS -->|TCP 6379| REDIS_SVC

    REDIS_SVC --> REDIS_POD[Redis Pod]

    %% Storage
    REDIS_POD --> PVC[PersistentVolumeClaim]
    PVC --> PV[PersistentVolume]
```

# Deploiment

## dev

```bash
kubectl create namespace citations-haddock-dev
kubectl -n citations-haddock-dev apply -f citations-haddock-app.yml
```

## prod

```bash
kubectl create namespace citations-haddock-prod
kubectl -n citations-haddock-prod apply -f citations-haddock-prod.yml
```