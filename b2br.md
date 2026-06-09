
## Utilitaires

| commande                                 | effet                                                                |
| ---------------------------------------- | -------------------------------------------------------------------- |
| **sudo** EDITOR=vi *commande/fichier*    | permet de definir l'editeur à vi pour ouvrir le fichier fourni       |
| sudo update-alternatives --config editor | permet de choisir l'éditeur que l'on souhaite, il doit être installé |
| ssh *login*@*hostname* -p 4242           | se connecte au hostname avec le login sur le port 4242               |
| ssh *login*@*ip* -p 4242                 | se connecte a l'adresse ip avec le login sur le port 4242            |
| getent *group* *nom_du_groupe*           | récupère tous les membres du groupe nommé                            |


## SUDO

| commande                                                                                                                                    | effet                                                                               | contexte                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------- |
| su -                                                                                                                                        | passer en mode admin                                                                | à utiliser si sudo n'existe pas encore      |
| apt update and apt install sudo -y                                                                                                          | installer sudo                                                                      | needs to be roots                           |
| usermod -aG sudo *login*                                                                                                                    | ajoute l'utilisateur désigné au groupe admin                                        | needs to be roots && a restart session / vm |
| Defaults        passwd_tries=3                                                                                                              | permet de set le nombre de try de connection en root à 3                            | doit être rajouté à visudo                  |
| Defaults        badpass_message="Mot de passe incorrect, réessayez."                                                                        | Défini un message d'erreur personalisé                                              | doit être rajouté à visudo                  |
| Defaults        logfile="/var/log/sudo/sudo.log"                                                                                            | permet d'activer les log: qui a utilisé sudo, où, quand et quelle commande          | doit être rajouté à visudo                  |
| Defaults        iolog_dir="/var/log/sudo"                                                                                                   | comme au dessus mais enregistre les input ouput de tous ce qui a été fait avec sudo | doit être rajouté à visudo                  |
| Defaults        requiretty                                                                                                                  | Enable the use of a true terminal                                                   | doit être rajouté à visudo                  |
| remplacer: Defaults secure_path<br>par Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" |                                                                                     | doit être rajouté / modifié dans visudo     |


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
| sudo apt update && sudo apt install libpam-pwquality -y                                                                                                                                                                    | install le module pour vérifier la qualitée du password                                                                                             |                                                                                                                |
| password        requisite                       pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root                                                   | remplacer: password        requisite                       pam_pwquality.so retry=3                                                                 | à faire /etc/pam.d/common-password                                                                             |

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


## Hostname


| commande                           | effet                                         | contexte |
| ---------------------------------- | --------------------------------------------- | -------- |
| hostnamectl                               | Affiche les informations importantes sur la vm|          |
| sudo hostnamectl set-hostname nouveau-nom | Change le hostname                            |          |
| sudo vim /etc/hosts                       | ouvre le fichier de config des hosts          |          |

## AppArmor


| commande                                                                       | effet                            | contexte                   |
| ------------------------------------------------------------------------------ | -------------------------------- | -------------------------- |
| GRUB_CMDLINE_LINUX_DEFAULT="quiet splash apparmor=1 lsm=apparmor"              | enable AppArmor au démarrage     | sudo vim /etc/default/grub |
| sudo systemctl enable apparmor<br>sudo systemctl start apparmor<br>sudo reboot | Activer le service et redémarrer |                            |
| sudo aa-status                                                                 | Vérifier le status du service    |                            |

## Script pour infos basiques
```
#!/bin/bash

ARCH=$(uname -a)
CPU_PHYS=$(grep "physical id" /proc/cpuinfo | sort -u | wc -l)
vCPU=$(grep "processor" /proc/cpuinfo | wc -l)
RAM_TOTAL=$(free -m | awk '$1 == "Mem:" {print $2}')
RAM_USE=$(free -m | awk '$1 == "Mem:" {print $3}')
RAM_PCT=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')
DISK_TOTAL=$(df -Bg | grep '^/dev/' | grep -v '/boot$' | awk '{fd += $2} END {print fd}')
DISK_USE=$(df -Bm | grep '^/dev/' | grep -v '/boot$' | awk '{ud += $3} END {print ud}')
DISK_PCT=$(df -Bm | grep '^/dev/' | grep -v '/boot$' | awk '{ud += $3; fd += $2} END {printf("%d"), ud/fd*100}')
CPU_LOAD=$(vmstat 1 2 | tail -1 | awk '{print 100 - $15}')
LAST_BOOT=$(who -b | awk '{print $3 " " $4}')
LVM_COUNT=$(lsblk | grep "lvm" | wc -l)
LVM_STATUS=$(if [ $LVM_COUNT -gt 0 ]; then echo "yes"; else echo "no"; fi)
TCP_CONN=$(ss -t | grep -i "estab" | wc -l)
USER_LOG=$(users | wc -w)
IP_ADDR=$(hostname -I | awk '{print $1}')
MAC_ADDR=$(ip link show | grep "link/ether" | awk '{print $2}')
SUDO_COUNT=$(journalctl _COMM=sudo 2>/dev/null | grep "COMMAND=" | wc -l)

wall << EOF
	#Architecture: $ARCH
	#CPU physical : $CPU_PHYS
	#vCPU : $vCPU
	#Memory Usage: $RAM_USE/${RAM_TOTAL}MB ($RAM_PCT%)
	#Disk Usage: $DISK_USE/${DISK_TOTAL}Gb ($DISK_PCT%)
	#CPU load : $CPU_LOAD%
	#Last boot : $LAST_BOOT
	#LVM use : $LVM_STATUS
	#Connections TCP : $TCP_CONN ESTABLISHED
	#User log : $USER_LOG
	#Network : IP $IP_ADDR ($MAC_ADDR)
	#Sudo : $SUDO_COUNT cmd
EOF
```

`chmod +x monitoring.sh`


| commandes               | infos                                                                                                                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `awk`<br>               | Permet de découper une ligne en colonnes. Par exemple, `$2` signifie "deuxième colonne".                                                                                                         |
| `wc -l`                 | Compte le nombre de lignes (Word Count `-l`ines). Très utile pour compter les processeurs, connexions, ou utilisateurs.                                                                          |
| `grep -v`               | Le drapeau `-v` permet d'**exclure** un mot. (Exemple : `grep -v '/boot$'` exclut la partition de boot pour ne pas fausser le calcul du stockage).                                               |
| `journalctl _COMM=sudo` | C'est la méthode moderne et propre sous Debian pour compter les exécutions de `sudo` à partir des journaux système, évitant ainsi les erreurs si les fichiers de logs classiques n'existent pas. |
### Cron


| commandes                                                                                     | infos                                                                           |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| sudo crontab -e                                                                               | ouvre le fichier de conf de crontab                                             |
| @reboot /chemin/complet/vers/monitoring.sh<br>*/10 * * * * /chemin/complet/vers/monitoring.sh | ajout de ces deux lignes pour le lancer au démarrage et toutes les deux minutes |
| sudo service cron stop                                                                        | stop le script sans le modifier                                                 |
| sudo service cron start                                                                       | lance le script sans le modifier                                                |




## Virt - manager
périphérique : e1000e

`<seclabel type='none'/>` doit etre rajouter a l'avant derniere ligne entre: `</devices>` et `</domain>`
such as :
```
  </devices>
  <seclabel type='none'/>
</domain>
```
doit etre rajouter entre `<mac adresse=` et `model type='virtio'`
ceci sert a creer un pont entre l'hote et la machine'

```
<portForward proto="tcp">
    <range start="4242" to="4242"/>
</portForward>
```

a mettre apres le `<model type ="virtio"/>`
```<backend type="passt"/>```


