
## Utilitaires

| commande                                 | effet                                                                |
| ---------------------------------------- | -------------------------------------------------------------------- |
| **sudo** EDITOR=vi *commande/fichier*    | permet de definir l'editeur à vi pour ouvrir le fichier fourni       |
| sudo update-alternatives --config editor | permet de choisir l'éditeur que l'on souhaite, il doit être installé |
| ssh *login*@*hostname* -p 4242           | se connecte au hostname avec le login sur le port 4242               |
| ssh *login*@*ip* -p 4242                 | se connecte a l'adresse ip avec le login sur le port 4242            |
| getent *group* *nom_du_groupe*           | récupère tous les membres du groupe nommé                            |


## SUDO

| commande                                                             | effet                                                                               | contexte                                    |
| -------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------- |
| su -                                                                 | passer en mode admin                                                                | à utiliser si sudo n'existe pas encore      |
| apt update and apt install sudo -y                                   | installer sudo                                                                      | needs to be roots                           |
| usermod -aG sudo *login*                                             | ajoute l'utilisateur désigné au groupe admin                                        | needs to be roots && a restart session / vm |
| Defaults        passwd_tries=3                                       | permet de set le nombre de try de connection en root à 3                            | doit être rajouté à visudo                  |
| Defaults        badpass_message="Mot de passe incorrect, réessayez." | Défini un message d'erreur personalisé                                              | doit être rajouté à visudo                  |
| Defaults        logfile="/var/log/sudo/sudo.log"                     | permet d'activer les log: qui a utilisé sudo, où, quand et quelle commande          | doit être rajouté à visudo                  |
| Defaults        iolog_dir="/var/log/sudo"                            | comme au dessus mais enregistre les input ouput de tous ce qui a été fait avec sudo | doit être rajouté à visudo                  |
| Defaults        requiretty                                           | Enable the use of a true terminal                                                   | doit être rajouté à visudo                  |

## groups / users

| commande                           | effet                                                               | contexte |
| ---------------------------------- | ------------------------------------------------------------------- | -------- |
| id *login*                         | permet de voir les infos de l'utilisateur désigné, uid, groupes etc |          |
| cat /etc/passwd                    | permet de voir les utilisateurs                                     |          |
| cat /etc/group                     | permet de voir les groupes                                          |          |
| sudo deluser --remove-home *login* | remove un user et son home                                          |          |
| sudo deluser *login* *groupe*      | permet de suppr un user                                             |          |
| sudo adduser *login*               | permet de rajouter un user de manière interactive                   |          |
| sudo usermod -aG *login* *group*   | ajoute un user à un groupe sans écraser ses groupes actuels         |          |
| sudo addgroup                      | Créer un nouveau groupe vide                                        |          |

## Mot de passe


| commande                                                                                                                                                                                                                   | effet                                                                                                                                               | contexte                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| sudo chage -l *login*                                                                                                                                                                                                      | permet de voir l'expiration, la validité d'un mot de passe                                                                                          |                                                                                                                |
| sudo chage -d 0 *login*                                                                                                                                                                                                    | force le changement de mot de passe au prochain login de l'utilisateur                                                                              |                                                                                                                |
| sudo chage -M 30 -m 2 -W 7 login                                                                                                                                                                                           | change le nombre de jour max à 30 jours, le nombre min à 2 jours et set l'avertissement à 7 jours                                                   | commande pour changer la validité, et la fréquence de changement d'un mot de passe pour un utilisateur existan |
| PASS_MAX_DAYS   30      # Le mot de passe expire après 30 jours<br>PASS_MIN_DAYS   2       # Délai minimal avant de pouvoir changer à nouveau<br>PASS_WARN_AGE   7       # Alerte l'utilisateur 7 jours avant l'expiration | changer ces paramètres dans le /etc/login.defs pour servir de template aux nouveaux utilisateurs.<br>pour ceux existant, voir la commande ci-dessus |                                                                                                                |
|                                                                                                                                                                                                                            |                                                                                                                                                     |                                                                                                                |
## SSH

| Commande                                 | effet                             | contexte                                |
| ---------------------------------------- | --------------------------------- | --------------------------------------- |
| sudo apt upgrade && sudo apt install ssh | install le service ssh            |                                         |
| sudo systemctl status ssh                | vérifie l'état du service ssh     |                                         |
| sudo vim /etc/ssh/sshd_config            | permet de changer la config ssh   |                                         |
| Port 4242              <br>              | # Changer le port par défaut (22) | à modifier dans le /etc/ssh/sshd_config |
| PermitRootLogin no                       | # Interdire la connexion en root  | à modifier dans le /etc/ssh/sshd_config |
| sudo systemctl restart ssh               | restart le service                |                                         |

## Firewall

| commande                           | effet                                         | contexte |
| ---------------------------------- | --------------------------------------------- | -------- |
| sudo apt install ufw               | installer le firewall ufw                     |          |
| sudo ufw default deny incoming<br> | setup le firewall pour  fermer tout les input |          |
| sudo ufw default allow outgoing    | setup le firewall pour ouvrir tout les ouput  |          |
| sudo ufw allow 4242                | Ouvrir le port 4242                           |          |
| sudo ufw enable                    | enable the firewall                           |          |
| sudo ufw status verbose            | pour checker le status du firewall            |          |

## Virt - manager
périphérique : e1000e