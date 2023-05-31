# Projet Pro

Dans le cadre d'une automatisation de tâches en tant que Analyste en sécurité informatique, j'ai pu réalisé ce projet dont l'objectif principal est de rechercher et collecter des informations spécifiques pour automatiser des actions de sécurité. 

Pour cela, j'ai mis en place l'outil Ansible installé sur une machine Kali ainsi qu'un hôte cible Ubuntu afin de réaliser des tests.

Ansible est un outil d'automatisation open source largement utilisé pour la gestion de configuration, le déploiement d'applications et l'orchestration de systèmes. Il offre également des fonctionnalités pour la sécurisation des systèmes à l'aide de playbooks de sécurité.

Un playbook Ansible est un fichier YAML qui définit une série de tâches à effectuer sur des nœuds cibles. Ces tâches peuvent inclure des actions telles que la configuration de paramètres de sécurité, l'installation de correctifs, la vérification de la conformité des configurations, la gestion des autorisations, etc.

## Architecture:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/1520dfe8-e681-4e81-bfeb-72f0a2faec83)


## Installation:
 Ansible (Management Node) sera installé sur une machine Kali comme indiqué ci-dessus. 
 
 Commande pour installer Ansible: sudo apt install ansible 

 Commande pour vérifier la version de ansible: ansible --version
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/2dada4aa-1ba5-49b1-a680-c71c00f144e0)

 
 
## Configuration de Kali:
 génération de clé SSH:
 Générer une clé SSH pour la configuration d'Ansible est nécessaire pour établir une connexion sécurisée entre le contrôleur Ansible (machine d'où les commandes      
 Ansible sont exécutées) et les hôtes clients (machines sur lesquelles les actions automatisées seront effectuées).

 L'utilisation d'une clé SSH permet d'éviter d'avoir à saisir un mot de passe à chaque fois qu'une commande Ansible est exécutée. Elle permet également de garantir
 l'authenticité et la confidentialité des communications entre le contrôleur et les hôtes clients.
 
 Pour générer la clé SSH, on va taper cette commande sur Kali: ssh-keygen et on appuye sur Entrée jusqu'à ce que la clé soit générée.
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/8febe33b-bd3a-4774-93bf-2a58e7ad34fa)
 
 
## Configuration de l'hôte Ubuntu pour l'automatisation Ubuntu:

 L'hôte qu'on souhaite configurer pour l'automatisation Ansible doit avoir le package de serveur SSH préinstallé.
 Tout d'abord, il faut installer le serveur OpenSSH s'il n'est pas déjà installé avec la commande suivante: sudo apt install openssh-server -y
 Ensuite, vérifier si le service sshd est en cours d'exécusion via la commande suivante: sudo systemctl status sshd
 Maintenant, il faut créer un utilisateur ansible et lui autoriser l'accès sudo sans mot de passe.
 
 Pour créer l'utilisateur, on tape la commande suivante: sudo adduser --shell /bin/bash --gecos "" ansible
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/ca33bd74-4135-43e7-80b8-0152f118516d)
 
 pour autoriser l'accès sudo sans mot de passe à l’utilisateur ansible, modifiez-le fichier /etc/sudoers avec la commande suivante: sudo visudo
 ajoutez la ligne suivante au fichier /etc/sudoers et enregistrez: ansible ALL=(ALL) NOPASSWD:ALL
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/6ad35978-6ca3-42eb-9ef6-79d0053be906)
 
 Copier la clé publique SSH sur l'hôte Ansible (Kali):
 
 À partir de l'ordinateur sur lequel on a installé Ansible (Kali), on copie la clé publique SSH sur le client Ubuntu comme suit: ssh-copy-id ansible@192.168.10.133 L'adresse IP 192.168.10.133 étant l'IP de mon client Ubuntu.
 
![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/ce8e2b09-5aa7-413d-a744-2e04f3f15e00)

 
 On doit maintenant pouvoir accéder en SSH à l'hôte client en tant qu'utilisateur ansible sans aucun mot de passe, comme vous pouvez le voir sur la capture d'écran ci-dessous:
 
![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/faabb85a-ca84-4276-87c1-bea7d5f59e70)

Afin de pouvoir utiliser les outils installés, il faudra autoriser les droits tcpdump sur le hôte distant en tapant les commandes suivantes:

sudo groupadd pcap

sudo usermod -a -G pcap $USER

sudo chgrp pcap /usr/bin/tcpdump

sudo chmod 750 /usr/bin/tcpdump

sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
 
 ## Création du playbook:
 
 Avant la création du playbook, on créé un répertoire appelé "project" sur lequel on crée le fichier du playbook ainsi que le fichier sur lequel on va définir le groupe d'hôtes. Ce fichier est utilisé comme inventaire Ansible pour spécifier les hôtes cibles et les groupes d'hôtes.
 
 Pour créer le répertoire "project", on tape la commande suivante:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/6426c275-0c80-4301-bb84-853193c67acd)
 
 Ensuite, on crée le fichier appelé "hosts" dans le même répertoire:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/7f815b1a-c5fe-457d-aab3-956743ad435a)

Dans ce fichier, on définit l'adresse IP de l'hôte cible, ici 192.168.10.133:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/7cea9998-36f8-4b68-911d-f939c224c3aa)


Afin de tester si ansible accède à l'hôte défini dans le fichier "hosts", on peut taper cette commande: ansible -i ./hosts all -m ping -u ansible

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/283c0d38-025b-40dd-90c3-7f39d53d09c9)


à cette étape, on crée notre playbook dans le répertoire "project" en utilisant cette commande: nano testintrusion.yml

Le code de ce fichier est présent sur le fichier "playbook.yml"

## Readme

### Scan de vulnérabilités et d'erreurs de configuration

Ce code représente un script d'automatisation pour effectuer un scan de vulnérabilités et détecter les erreurs de configuration sur un hôte distant. Il utilise plusieurs outils de sécurité courants tels que Nmap, Nikto et Metasploit.

#### Prérequis
Avant d'exécuter ce script, assurez-vous d'avoir les éléments suivants :
- Un hôte distant accessible avec l'adresse IP spécifiée dans la variable `ansible_host`.
- Ansible installé sur la machine exécutant le script.

#### Description des tâches

1. Ajout de la clé GPG du référentiel Metasploit et ajout du référentiel Metasploit à partir de la source spécifiée.
2. Mise à jour de la liste des paquets disponibles sur l'hôte distant.
3. Installation des outils de sécurité courants (Nmap, Nikto, Metasploit, Wireshark, tshark) sur l'hôte distant.
4. Exécution d'un test de vulnérabilité avec Nmap et enregistrement du rapport dans `rapport_scan_nmap.txt`.
5. Récupération du fichier de rapport Nmap depuis l'hôte distant.
6. Recherche des mots de passe incorrects et des échecs d'authentification dans le journal d'authentification de l'hôte distant et enregistrement des résultats dans `auth_failures.log`.
7. Récupération des événements d'échec d'authentification depuis l'hôte distant.
8. Scan de vulnérabilités des applications web avec Nikto et enregistrement du rapport dans `rapport_scan_nikto.txt`.
9. Récupération du fichier de rapport Nikto depuis l'hôte distant.
10. Exécution d'un test de vulnérabilité avec Metasploit et enregistrement du rapport dans `rapport_scan_metasploit.txt`.
11. Récupération du fichier de rapport Metasploit depuis l'hôte distant.
12. Capture du trafic réseau avec Wireshark pendant 60 secondes et enregistrement dans `rapport_wireshark.pcap`.
13. Récupération du fichier de capture Wireshark depuis l'hôte distant.
14. Génération du rapport des vulnérabilités détectées via Metasploit.
15. Génération du rapport des vulnérabilités détectées via Nmap.
16. Génération du rapport des vulnérabilités détectées via Nikto.
17. Génération du rapport des erreurs d'authentification.
18. Affichage du rapport des vulnérabilités et des erreurs de configuration.

##### Utilisation
1. Assurez-vous que l'hôte distant est accessible et que vous avez les droits nécessaires pour exécuter les tâches en tant que superutilisateur (root).
2. Modifiez le fichier d'inventaire Ansible pour spécifier l'hôte distant dans la section `hosts`.
3. Exécutez le script à l'aide de la commande `ansible-playbook` en spécifiant le chemin vers le fichier contenant le code.

##### Remarques
- Ce script utilise des commandes shell pour exécuter les différentes tâches. Assurez-vous que les outils requis sont installés sur l'hôte distant.
- Les résultats des différentes analyses et des erreurs d'authentification sont enregistrés dans des fichers de rapport pour une consultation ultérieure.
- Les rapports générés sont affichés à la fin de l'exécution du script grâce à la tâche debug.



### Résultat de test du playbook:

On lance le playbook en utilisant la commande suivante: sudo ansible-playbook -i hosts testintrusion7.yml 

Les fichiers suivants se crééent automatiqument dans le répertoire "project" 
##### Fihcier Nmap:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/2d39ca04-3282-4f26-a89b-8295b748f0c6)

 L'hôte "ismahane" (192.168.10.133) est en ligne avec une latence très faible.
 
 Il y a 998 ports fermés qui n'ont pas été inclus dans le rapport.
 
 Les ports 22 (SSH) et 80 (HTTP) sont ouverts.
 
 Le service SSH utilise OpenSSH 8.9p1 sur Ubuntu Linux.
 
 Le service HTTP utilise nginx 1.18.0 sur Ubuntu.
 
 Le serveur Web prend en charge les méthodes GET et HEAD.
 
 Le titre de la page d'accueil est "Welcome to nginx!".
 
 Le système d'exploitation est Linux 2.6.X avec le noyau Linux 2.6.32.
 
 L'estimation de l'uptime est d'environ 43,501 jours.
 
 L'hôte est sur le même réseau (0 sauts de réseau).
 
 La prédiction des numéros de séquence TCP est considérée comme difficile (Difficulté=248).


##### Fichier Nikto:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/a19a0742-f9aa-4444-9b80-c91099ce979c)

 IP cible : 192.168.10.133
 
 Nom d'hôte cible : 192.168.10.133
 
 Port cible : 80
 
 Serveur : nginx/1.18.0 (Ubuntu)
 
 Vulnérabilités identifiées : Fuites d'inodes via ETags, absence de l'en-tête X-Frame-Options anti-clickjacking
 
 2 problèmes signalés sur 6544 éléments vérifiés
 
 Durée du scan : 132 secondes
    
 ##### Fichier des évènements d'échec d'authentification:
 
 Après 3 échecs d'authentification sur le hôte client, voici le résultat du test du playbook:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/790c1cc4-364d-4030-9925-f6b3407c9f00)

    




 




