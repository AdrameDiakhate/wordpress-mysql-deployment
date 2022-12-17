# wordpress-mysql-deployment
Ce projet consiste à déployer Wordpress et Mysql avec des Persistent Volume Claim (PVC) sur minikube.

Pour utiliser ce projet, il faut déja installer minikube sur sa machine et suivre les étapes suivantes:  

1) Cloner le projet

2) Créer un fichier kustomization.yml qui contient toutes les ressources pour déployer un site Wordpress et une bdd Mysql

   Voici le contenu de ce fichier kustomization.yml:

            secretGenerator:
            - name: mysql-pass
            literals:
            - password=abcd
            resources:
            - mysql-deployment.yml
            - wordpress-deployment.yml

Mettre votre vrai mot de passe à la place de : abcd

3) Vérifier le context de Kubernetes avec: kubectl config current-context

4)S'il s'agit de minikube continuez; Sinon faire: kubectl config use-context minikube pour switcher vers minikube

5) Faire : kubectl apply -k ./ pour lancer le déploiement

6) Vous pouvez faire les vérifications ci-dessous pour voir si tout c'est bien passé:

    a)Vérifiez que le secret existe en exécutant la commande suivante : kubectl get secrets
    
    b)Vérifiez qu'un PersistentVolume a été provisionné dynamiquement: kubectl get pvc
    
    c)Vérifiez que le pod est en cours d'exécution en exécutant la commande suivante : kubectl get pods

    d)Vérifiez que le service est en cours d'exécution en exécutant la commande suivante : kubectl get services wordpress

    e)Exécutez la commande suivante pour obtenir l'adresse IP du service WordPress : minikube service wordpress --url


NB:
    *Minikube ne peut exposer les services que via NodePort. L'EXTERNAL-IP est toujours en attente.