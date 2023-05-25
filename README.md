Projet Pro
Dans le cadre d'une automatisation de tâches en tant que Analyste en sécurité informatique, j'ai pu réalisé ce projet dont l'objectif principal est de rechercher et collecter des informations spécifiques pour automatiser des actions de sécurité. 

Pour cela, j'ai mis en place l'outil Ansible installé sur une machine Kali ainsi qu'un hôte client Debian afin de réaliser des tests.

Ansible est un outil d'automatisation open source largement utilisé pour la gestion de configuration, le déploiement d'applications et l'orchestration de systèmes. Il offre également des fonctionnalités pour la sécurisation des systèmes à l'aide de playbooks de sécurité.

Un playbook Ansible est un fichier YAML qui définit une série de tâches à effectuer sur des nœuds cibles. Ces tâches peuvent inclure des actions telles que la configuration de paramètres de sécurité, l'installation de correctifs, la vérification de la conformité des configurations, la gestion des autorisations, etc.

Architecture:

![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/a6a7420d-4bf0-4533-bcde-7a5d6945b3fe)

Installation:
 Ansible (Management Node) sera installé sur une machine KALI comme indiqué ci-dessus. 
 Commande pour installer Ansible: sudo apt install ansible 
 Commande pour vérifier la version de ansible: ansible --version
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/ef9039fd-2549-4bb2-9734-948e7ebc91fc)

Configuration:
 génération de clé SSH:
 Générer une clé SSH pour la configuration d'Ansible est nécessaire pour établir une connexion sécurisée entre le contrôleur Ansible (machine d'où les commandes Ansible 
 sont exécutées) et les hôtes clients (machines sur lesquelles les actions automatisées seront effectuées).

 L'utilisation d'une clé SSH permet d'éviter d'avoir à saisir un mot de passe à chaque fois qu'une commande Ansible est exécutée. Elle permet également de garantir
 l'authenticité et la confidentialité des communications entre le contrôleur et les hôtes clients.
 
 Pour générer la clé SSH, on va taper cette commande sur Kali: ssh-keygen et on appuye sur Entrée jusqu'à ce que la clé soit générée.
 
 ![image](https://github.com/ismahane92/Projet-Pro/assets/134289075/9f23f1da-3683-4f34-89b9-97fe9db8e908)


 



