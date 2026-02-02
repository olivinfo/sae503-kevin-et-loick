
## Les 12 facteurs

### I. Base de code
Une base de code suivie avec un système de contrôle de version, plusieurs déploiements
- Utilisation d'un environement dev/prod avec utilisation de git

### II. Dépendances
Déclarez explicitement et isolez les dépendances
- Utilisation d'un fichier requirement.txt

### III. Configuration
Stockez la configuration dans l’environnement
- Utilisation de varible d'environement

### IV. Services externes
Traitez les services externes comme des ressources attachées

### V. Assemblez, publiez, exécutez
Séparez strictement les étapes d’assemblage et d’exécution
- Utilisation d'un Dockerfile puis build dans une pipeline et stockage des images dans github. Pull puis déploiment de ces images dans Kubernetes.

VI. Processus
Exécutez l’application comme un ou plusieurs processus sans état
- 

### VII. Associations de ports
Exportez les services via des associations de ports
- Utilisation de Traefik

### VIII. Concurrence
Grossissez à l’aide du modèle de processus
- Grossissement vertical grâce à un modèle sans état

### IX. Jetable
Maximisez la robustesse avec des démarrages rapides et des arrêts gracieux
- Utilisation de gunicorn pour gérer le lifecycle de l'application python.

### X. Parité dev/prod
Gardez le développement, la validation et la production aussi proches que possible
- Utilisation de différent namespace

### XI. Logs
Traitez les logs comme des flux d’évènements
- Utilisation des logs des différents conteneurs

### XII. Processus d’administration
Lancez les processus d’administration et de maintenance comme des one-off-processes
- Présence d'un token admin pour administrer l'application