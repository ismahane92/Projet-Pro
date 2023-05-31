# Projet Pro

Dans le cadre d'une automatisation de tâches en tant que Analyste en sécurité informatique, j'ai pu réalisé ce projet dont l'objectif principal est de rechercher et collecter des informations spécifiques pour automatiser des actions de sécurité. 

Pour cela, j'ai mis en place l'outil Ansible installé sur une machine Kali ainsi qu'un hôte cible Ubuntu afin de réaliser des tests.

Ansible est un outil d'automatisation open source largement utilisé pour la gestion de configuration, le déploiement d'applications et l'orchestration de systèmes. Il offre également des fonctionnalités pour la sécurisation des systèmes à l'aide de playbooks de sécurité.

Un playbook Ansible est un fichier YAML qui définit une série de tâches à effectuer sur des nœuds cibles. Ces tâches peuvent inclure des actions telles que la configuration de paramètres de sécurité, l'installation de correctifs, la vérification de la conformité des configurations, la gestion des autorisations, etc.

## Architecture:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/84085634-77e3-40d2-8d79-a9dfe7f4c904)

## Installation:
 Ansible (Management Node) sera installé sur une machine Kali comme indiqué ci-dessus. 
 
 Commande pour installer Ansible: sudo apt install ansible 

 Commande pour vérifier la version de ansible: ansible --version
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/ef9039fd-2549-4bb2-9734-948e7ebc91fc)
 
 
## Configuration de Kali:
 génération de clé SSH:
 Générer une clé SSH pour la configuration d'Ansible est nécessaire pour établir une connexion sécurisée entre le contrôleur Ansible (machine d'où les commandes      
 Ansible sont exécutées) et les hôtes clients (machines sur lesquelles les actions automatisées seront effectuées).

 L'utilisation d'une clé SSH permet d'éviter d'avoir à saisir un mot de passe à chaque fois qu'une commande Ansible est exécutée. Elle permet également de garantir
 l'authenticité et la confidentialité des communications entre le contrôleur et les hôtes clients.
 
 Pour générer la clé SSH, on va taper cette commande sur Kali: ssh-keygen et on appuye sur Entrée jusqu'à ce que la clé soit générée.
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/9f23f1da-3683-4f34-89b9-97fe9db8e908)
 
 
## Configuration de l'hôte Ubuntu pour l'automatisation Ubuntu:

 L'hôte qu'on souhaite configurer pour l'automatisation Ansible doit avoir le package de serveur SSH préinstallé.
 Tout d'abord, il faut installer le serveur OpenSSH s'il n'est pas déjà installé avec la commande suivante: sudo apt install openssh-server -y
 Ensuite, vérifier si le service sshd est en cours d'exécusion via la commande suivante: sudo systemctl status sshd
 Maintenant, il faut créer un utilisateur ansible et lui autoriser l'accès sudo sans mot de passe.
 
 Pour créer l'utilisateur, on tape la commande suivante: sudo adduser --shell /bin/bash --gecos "" ansible
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/2220fe1d-6948-4ec1-a38b-5d53a19bcb02)
 
 pour autoriser l'accès sudo sans mot de passe à l’utilisateur ansible, modifiez-le fichier /etc/sudoers avec la commande suivante: sudo visudo
 ajoutez la ligne suivante au fichier /etc/sudoers et enregistrez: ansible ALL=(ALL) NOPASSWD:ALL
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/76b4b42f-470c-4b1d-b690-193bb48f23b7)
 
 Copier la clé publique SSH sur l'hôte Ansible (Kali):
 À partir de l'ordinateur sur lequel on a installé Ansible (Kali), on copie la clé publique SSH sur le client Ubuntu comme suit: ssh-copy-id ansible@192.168.10.133 L'adresse IP 192.168.10.133 étant l'IP de mon client Ubuntu.
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/89df2a4e-e9f5-40cc-9613-51ac0b724c0f)
 
 On doit maintenant pouvoir accéder en SSH à l'hôte client en tant qu'utilisateur ansible sans aucun mot de passe, comme vous pouvez le voir sur la capture d'écran ci-dessous:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/874b5a05-4b88-4605-b114-a50706f500a8)
 
 ## Création du playbook:
 
 Avant la création du playbook, on créé un répertoire appelé "project" sur lequel on crée le fichier du playbook ainsi que le fichier sur lequel on va définir le groupe d'hôtes. Ce fichier est utilisé comme inventaire Ansible pour spécifier les hôtes cibles et les groupes d'hôtes.
 
 Pour créer le répertoire "project", on tape la commande suivante:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/7a1223e7-27b4-418c-9d0f-781ff7e786d4)
 
 Ensuite, on crée le fichier appelé "hosts" dans le même répertoire:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/3e182454-518a-499f-b536-6720b2b1f51f)

Dans ce fichier, on définit l'adresse IP de l'hôte cible, ici 192.168.10.133:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/65e768d3-5a28-47e2-abc1-fbc94a5e07b2)


Afin de tester si ansible accède à l'hôte défini dans le fichier "hosts", on peut taper cette commande: ansible -i ./hosts all -m ping -u ansible

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/ef7b7a27-7164-47c8-ae25-83412b7a32da)


à cette étape, on crée notre playbook dans le répertoire "project" en utilisant cette commande: nano testintrusion.yml

Le code de ce fichier est présent sur le fichier "playbook.yml"

## Readme

### Scan de vulnérabilités

Ce script automatisé est conçu pour effectuer un scan de vulnérabilités sur un hôte appelé "Client1". Il utilise plusieurs outils de sécurité courants tels que Nmap, Nikto, Metasploit Framework, Wireshark et Tshark pour effectuer différentes tâches de scan.

#### Configuration

Le script utilise Ansible pour gérer la configuration et l'exécution des tâches. La section "hosts" spécifie l'hôte cible sur lequel les scans seront effectués. La propriété "become" est définie sur "true" pour exécuter les tâches en tant que superutilisateur (root) afin d'accéder aux ressources système nécessaires.

#### Tâches

##### Ajout de la clé GPG du référentiel Metasploit : 
Cette tâche ajoute la clé GPG du référentiel Metasploit au système pour permettre la vérification des paquets provenant de ce référentiel.

##### Ajout du référentiel Metasploit :
Cette tâche ajoute le référentiel Metasploit à la liste des sources de paquets du système. Il spécifie le lien vers le référentiel et son état "présent" pour s'assurer qu'il est activé.

##### Mise à jour de la liste des paquets disponibles :
Cette tâche met à jour la liste des paquets disponibles sur l'hôte en exécutant la commande "apt update" pour récupérer les dernières informations des paquets depuis les référentiels.

##### Installation des outils de sécurité courants :
Cette tâche utilise le module "apt" pour installer plusieurs outils de sécurité, notamment Nmap, Nikto, Metasploit Framework, Wireshark et Tshark, sur l'hôte cible.

##### Exécution d'un test de vulnérabilité avec Nmap :
Cette tâche exécute la commande shell "nmap" avec des options spécifiques pour effectuer un test de vulnérabilité sur l'hôte cible. Les résultats du test sont enregistrés dans le fichier "rapport_scan_nmap.txt".

##### Recherche des mots de passe incorrects dans le journal d'authentification :
Cette tâche utilise la commande shell "grep" pour rechercher les lignes contenant le motif "authentication failure" dans le fichier de journal d'authentification "/var/log/auth.log". Les résultats sont enregistrés dans le fichier "auth_failures.log".

##### Récupération des événements d'échec d'authentification :
Cette tâche utilise le module "fetch" pour récupérer le fichier "auth_failures.log" depuis l'hôte distant vers le répertoire local.

##### Scan de vulnérabilité des applications web avec Nikto : 
Cette tâche exécute la commande shell "nikto" avec des options spécifiques pour effectuer un scan de vulnérabilité des applications web sur l'adresse IP spécifiée. Les résultats du scan sont enregistrés dans le fichier "rapport_scan_nikto.txt".

##### Récupération du fichier de journal Nmap :
Cette tâche utilise le module "fetch" pour récupérer le fichier "rapport_scan_nmap.txt" depuis l'hôte distant vers le répertoire local.

##### Capture du trafic réseau avec Wireshark : 
Cette tâche utilise la commande shell "tcpdump" avec des options spécifiques pour capturer le trafic réseau pendant une minute. Les résultats sont enregistrés dans le fichier "rapport_wireshark.pcap".

##### Récupération du fichier de capture Wireshark :
Cette tâche utilise le module "fetch" pour récupérer le fichier de capture "rapport_wireshark.pcap" depuis l'hôte distant vers le répertoire local.

##### Génération du rapport des vulnérabilités détectées via Wireshark :
Cette tâche utilise la commande shell "tcpdump" avec des options spécifiques pour analyser le fichier de capture Wireshark et extraire les informations sur les vulnérabilités détectées. Les résultats sont enregistrés dans le fichier "rapport_vulns_wireshark.txt".

##### Récupération du fichier de journal Nikto : 
Cette tâche utilise le module "fetch" pour récupérer le fichier "rapport_scan_nikto.txt" depuis l'hôte distant vers le répertoire local.

##### Affichage du nombre de tentatives de mots de passe échoués : 
Cette tâche utilise la commande shell "grep" pour compter le nombre de tentatives de mots de passe échoués en recherchant le motif "authentication failure" dans le fichier de journal d'authentification. Le résultat est enregistré dans la variable "auth_failures_count".

##### Affichage du nombre de tentatives de mots de passe échoués : 
Cette tâche utilise le module "debug" pour afficher le nombre de tentatives de mots de passe échoués en utilisant la valeur de la variable "auth_failures_count.stdout".

##### Génération d'un rapport des événements d'échec d'authentification :
 Cette tâche utilise la commande shell "cat" pour ajouter le contenu du fichier "auth_failures.log" au fichier "rapport_auth_failures.txt".

### Utilisation

Pour utiliser ce script, vous devez disposer d'Ansible installé sur votre machine. Vous pouvez exécuter le script en spécifiant l'hôte cible dans la section "hosts" et en exécutant la commande "ansible-playbook" suivi du nom du fichier contenant le script.

Assurez-vous d'adapter les adresses IP et les chemins des fichiers de sortie selon vos besoins.

Note : Ce script nécessite des privilèges d'administrateur (superutilisateur) sur l'hôte cible, car il effectue des opérations système et accède à des journaux système sensibles.

### Résultat de test du playbook:

On lance le playbook en utilisant la commande suivante: sudo ansible-playbook -i hosts testintrusion7.yml 












##### Fihcier Nmap:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/b19b7ac1-0f84-4b95-aded-416e6112fd29)

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

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/6b90031d-a2ed-4806-ba09-a1c8af183a4b)

 IP cible : 192.168.10.133
 
 Nom d'hôte cible : 192.168.10.133
 
 Port cible : 80
 
 Serveur : nginx/1.18.0 (Ubuntu)
 
 Vulnérabilités identifiées : Fuites d'inodes via ETags, absence de l'en-tête X-Frame-Options anti-clickjacking
 
 2 problèmes signalés sur 6544 éléments vérifiés
 
 Durée du scan : 132 secondes
    
 ##### Fichier des évènements d'échec d'authentification:
 
 Après 2 échecs d'authentification sur le hôte client, voici le résultat du test du playbook:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/382b016b-859e-49ea-9862-8a3c79c89223)
 
 
    
    




 




