1\. Contexte et objectifs
=========================

*   Contexte fonctionnel : appli Haddock, API /users, /quotes, /search, /apidocs.
    
*   Objectifs techniques :
    
*   Conteneurisation + orchestration Kubernetes.
    
*   Deux plateformes cloisonnées : dev et prod.
    
*   Scalabilité, isolation, sécurité, persistance.
    

2\. Architecture globale du système
===================================

*   Schéma d’ensemble :
    
*   Client → Traefik → Services (users/quotes/search) → Base de données unique.
    
*   Deux namespaces : dev, prod.
    
*   Namespaces :
    
*   namespace dev : environnement de qualification.
    
*   namespace prod : environnement de production.
    
*   Justification : isolation logique, politiques de ressources, secrets séparés.
    

3\. Découpage applicatif en microservices
=========================================

*   Microservice Users :
    
*   Gère /users (GET/POST).
    
*   Accès base de données (table users).
    
*   Microservice Quotes :
    
*   Gère /quotes (GET/POST/DELETE).
    
*   Microservice Search :
    
*   Gère /search (GET, recherche par mot-clé).
    
*   Justification :
    
*   Alignement avec les bonnes pratiques 12-Factor.
    
*   Scalabilité indépendante (ex : search plus sollicité).
    

Inclure un diagramme de composants montrant les 3 services + DB + Traefik.

4\. Architecture Kubernetes
===========================

*   Cluster :
    
*   Mono-nœud (VM unique) mais architecture pensée “cluster-ready”.
    
*   Objets principaux :
    
*   Deployments pour chaque microservice.
    
*   Services (ClusterIP) pour l’interne.
    
*   Ingress (Traefik) pour l’externe.
    
*   PersistentVolumeClaim pour la base de données.
    
*   Traefik comme reverse proxy / Ingress Controller :
    
*   Routage par FQDN nip.io :
    
*   haddock-dev..nip.io → namespace dev.
    
*   haddock-prod..nip.io → namespace prod.
    
*   Rate limiting (10 req/min) via Middleware Traefik.
    

5\. Gestion des ressources et isolation dev/prod
================================================

*   ResourceQuotas :
    
*   dev : quotas CPU/RAM/pods plus faibles.
    
*   prod : quotas plus larges.
    
*   LimitRanges :
    
*   Limites par container dans dev pour éviter les pods gloutons.
    
*   (Optionnel à mentionner) PriorityClass :
    
*   prod prioritaire en cas de saturation (même si mono-nœud, tu peux l’expliquer).
    

Tu peux ajouter un tableau comparatif dev/prod (ressources, replicas, FQDN, priorité).

############################################################################

Non complété

############################################################################

6\. Sécurité et secrets
=======================

*   Secrets Kubernetes :
    
*   ADMIN\_KEY.
    
*   Credentials base de données.
    
*   Variables sensibles (JWT secret éventuel, etc.).
    
*   Justification :
    
*   Pas de secrets en clair dans les manifests ou images.
    
*   Autres points sécurité :
    
*   Images minimales/distroless (distro alpine).
    
*   Non-root via securityContext.
    

############################################################################

7\. Stockage et persistance
===========================

*   Base de données :
    
*   Redis.
    
*   Déploiement dans un pods
    
*   Persistance :
    
*   PVC
    
*   Justification : ne pas perdre les utilisateurs/citations.
    

Inclure un diagramme de déploiement montrant pods + volumes.

8\. Choix techniques et justification
=====================================

Pour chaque choix, tu expliques pourquoi :

*   Kubernetes (orchestration, montée en charge, isolation).
    
*   Traefik (Ingress Controller moderne, CRD, middlewares, rate limiting).
    
*   Namespaces dev/prod (cloisonnement, gouvernance).
    
*   ResourceQuotas/LimitRanges (protection de prod).
    
*   Secrets (sécurité).
    
*   Microservices (scalabilité, maintenabilité).