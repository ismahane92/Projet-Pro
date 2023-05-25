Projet Pro

Dans le cadre d'une automatisation de tâches en tant que Analyste en sécurité informatique, j'ai pu réalisé ce projet dont l'objectif principal est de rechercher et collecter des informations spécifiques pour automatiser des actions de sécurité. 

Pour cela, j'ai mis en place l'outil Ansible installé sur une machine Kali ainsi qu'un hôte client Ubuntu afin de réaliser des tests.

Ansible est un outil d'automatisation open source largement utilisé pour la gestion de configuration, le déploiement d'applications et l'orchestration de systèmes. Il offre également des fonctionnalités pour la sécurisation des systèmes à l'aide de playbooks de sécurité.

Un playbook Ansible est un fichier YAML qui définit une série de tâches à effectuer sur des nœuds cibles. Ces tâches peuvent inclure des actions telles que la configuration de paramètres de sécurité, l'installation de correctifs, la vérification de la conformité des configurations, la gestion des autorisations, etc.

Architecture:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/a6a7420d-4bf0-4533-bcde-7a5d6945b3fe)

Installation:
 Ansible (Management Node) sera installé sur une machine KALI comme indiqué ci-dessus. 
 Commande pour installer Ansible: sudo apt install ansible 
 Commande pour vérifier la version de ansible: ansible --version
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/ef9039fd-2549-4bb2-9734-948e7ebc91fc)
 
Configuration de Kali:
 génération de clé SSH:
 Générer une clé SSH pour la configuration d'Ansible est nécessaire pour établir une connexion sécurisée entre le contrôleur Ansible (machine d'où les commandes Ansible 
 sont exécutées) et les hôtes clients (machines sur lesquelles les actions automatisées seront effectuées).

 L'utilisation d'une clé SSH permet d'éviter d'avoir à saisir un mot de passe à chaque fois qu'une commande Ansible est exécutée. Elle permet également de garantir
 l'authenticité et la confidentialité des communications entre le contrôleur et les hôtes clients.
 
 Pour générer la clé SSH, on va taper cette commande sur Kali: ssh-keygen et on appuye sur Entrée jusqu'à ce que la clé soit générée.
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/9f23f1da-3683-4f34-89b9-97fe9db8e908)
 
 
Configuration de l'hôte Ubuntu pour l'automatisation Ubuntu:

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
 À partir de l'ordinateur sur lequel on a installé Ansible (Kali), on copie la clé publique SSH sur le client Ubuntu comme suit: ssh-copy-id ansible@192.168.10.133
 L'adresse IP 192.168.10.133 étant l'IP de mon client Ubuntu.
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/89df2a4e-e9f5-40cc-9613-51ac0b724c0f)
 
 On doit maintenant pouvoir accéder en SSH à l'hôte client en tant qu'utilisateur ansible sans aucun mot de passe, comme vous pouvez le voir sur la capture d'écran ci-   
 dessous:
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/874b5a05-4b88-4605-b114-a50706f500a8)
 
 


 




