# TOS-Longhorn

Qu'est-ce que Longhorn ? Longhorn est un **CSI**, **C**ontainer **S**torage **I**nterface. Ce composant de Kubernetes permet de fournir un stockage persistant aux applications stateful. 

*Exemple : Une base de données.*

Longhorn permet aussi de faire des backups et de restorer ces backups entre autres.
De plus, le point positif de Longhorn est  qu'il dispose d'une interface graphique, ce qui facilite la gestion du stockage du cluster Kubernetes.


# Pré-requis

 - Un cluster Kubernetes fonctionnel avec une version supérieure à 1.21
 - Open-iscsi doit être installé  : [Suivre cette documentation](https://longhorn.io/docs/1.5.1/deploy/install/#installing-open-iscsi)
 - Un client NSFv4 doit être installé *(il est normalement natif sur debian)* : [Documentation](https://longhorn.io/docs/1.5.1/deploy/install/#installing-nfsv4-client)
 - Metallb si vous avez un cluster bare metal : [Documentation](https://metallb.universe.tf/installation/)
 
 En lançant cette commande : 
 
 `curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/scripts/environment_check.sh | bash`
 
 Un script de validation des pré-requis sera lancé et indiquera si besoin quels composants doivent encore être installés pour avoir un environnement fonctionnel


## Installation avec Kubectl

Si votre choix se porte sur une installation avec Kubectl, veillez à suivre ces étapes.

 1. Installation

    `kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/deploy/longhorn.yaml`

 2. Vérification de l'installation

	  Les commandes suivantes permettront de voir le déploiement des pods en temps réel.

    `kubectl get pods \
--namespace longhorn-system \--watch`

 3. Vérification de l'état des pods 

     `kubectl -n longhorn-system get pod`

	Chacun des pods devraient être en état "running", ce qui signifie que Longhorn est bien installé.

## Installation avec Helm

Si vous préférez procéder à l'installation avec Helm, suivre les instructions suivantes.

 1. Ajout du repo Helm Longhorn

    `helm repo add longhorn https://charts.longhorn.io`

 2. Recherche des dernières charts

	   `helm repo update`

 3. Installation

	`helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace --version 1.5.1`

 4. Vérification de l'état des pods

	  `kubectl get pod -n longhorn-system`

	

	Chacun des pods devraient être en état "running", ce qui signifie que Longhorn est bien installé.




## Accès à l'interface graphique

 La dernière étape consiste à accéder à l'interface graphique de Longhorn.

 1. Récupérer l'IP du service

`kubectl -n longhorn-system get svc`

Un output similaire devrait apparaître

```shell
NAME                TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)        AGE
longhorn-backend    ClusterIP      10.20.248.250   <none>           9500/TCP       58m
longhorn-frontend   ClusterIP      10.20.245.110   <none>           80/TCP         58m
```

Dans le cas de l'utilisation de Metallb, le service sera configuré pour être de type LoadBalancer au lieu de ClusterIP.
Une IP externe sera donc affichée avec un second port.

2. Accéder à l'interface

Une fois l'ip et le port récupérés, accéder à l'interface graphique de cette manière dans une machine étant dans le même réseau.

http://*EXTERNAL-IP*:*PORT*

L'interface graphique de Longhorn devrait ressembler à ceci.

![](https://longhorn.io/img/screenshots/getting-started/longhorn-ui.png)

**Félicitations, vous avec installé Longhorn avec succès**  🥳
