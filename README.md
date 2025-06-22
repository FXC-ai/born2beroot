# Born2BeRoot

Notes en franglais pour le projet Born To Be Root de l'école 42

Change the keyboard layout :

```bash
sudo dpkg-reconfigure keyboard-configuration  
sudo service keyboard-setup restart
```

La bonne configuration pour le clavier : un clavier English (US).

# General instructions

The .vdi is at `users/fcoindre/goinfre/born2beroot/`

For check the VM : `shasum <VM_name.vdi>`

# Questions about the mandatory part

## What is the difference between Rocky and Debian ?

Tout d’abord une distribution Linux consiste à l’utilisation d’un noyau GNU/Linux autour duquel sont développés un ensemble de logiciels. Cet ensemble de logiciel fonctionne de façon cohérente et constitue un OS.

Rocky et Debian sont deux distribution linux open source qui utilisent le noyau GNU/Linux. Cependant Rocky Linux a été initialement développé par l’entreprise Red Hat. Cette distribution est compatible avec les produits de l’entreprise Red Hat. Debian est développée par une communauté. Rocky est plus développé en vu de répondre aux besoins d’une entreprise. Debian est plus généraliste et plus facile d’utilisation.

## What is the difference between aptitude and APT (Advanced Packaging Tool)?

- Aptitude is a high-level package manager while **APT is lower level** which can be used by other higher level package managers
- **Aptitude is smarter and will automatically remove unused packages or suggest installation of dependent packages**
- Apt will only do explicitly **what it is told to do in the command line**

![1 vRgjWvlyZsTHhO3_4Qsvlg.webp](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78cc8453-b2e1-41e2-a80d-7946870a3ade/1_vRgjWvlyZsTHhO3_4Qsvlg.webp)

## What is AppArmor ?

AppArmor est un module de sécurité Linux qui permet aux administrateurs de restreindre les capacités des programmes et des processus en cours d'systexécution sur un système. Il est utilisé pour protéger le système et ses utilisateurs contre les programmes malveillants ou défectueux, en les confinant à un ensemble de règles qui spécifient ce qu'ils sont autorisés à faire.

Sur les systèmes Debian, AppArmor est généralement utilisé pour protéger les composants critiques du système et les applications, tels que le noyau Linux, les bibliothèques système et les serveurs réseau. Il peut également être utilisé pour protéger les programmes et les données utilisateur, tels que les navigateurs web, les clients de messagerie et les fichiers personnels.

Pour utiliser AppArmor sur Debian, vous devez installer le paquet apparmor et configurer des profils pour les programmes et les processus que vous souhaitez protéger. Les profils spécifient les ressources auxquelles les programmes sont autorisés à accéder, telles que des fichiers spécifiques, des répertoires, des sockets de réseau et des périphériques matériels.

AppArmor est un outil utile pour améliorer la sécurité d'un système Debian, car il vous permet de affiner les autorisations des programmes et des processus, et de les empêcher d'exécuter certaines actions qui pourraient être nuisibles pour le système ou ses utilisateurs. Cependant, il peut également être complexe à configurer et nécessiter un certain effort pour être mis en place et maintenu.

# Simple Setup

## Ensure the password follows the required policy

Les fichiers modifiés sont : 

- `/etc/login.defs`: pour les la politique d’expiration des mots de passe
- `/etc/pam.d/common-password`: pour la dureté des mots de passe

Pour verifier si les règles s’applique bien à un utilisateur : 

```bash
sudo chage -l <nom de l’utilisateur>
```

Il est aussi possible de modifier la politique de mot de passe d’un utilisateur :

```bash
sudo chage <nom de l’utilisateur>
```

## Checks UFW and SSH service are started

| UFW started ? | `sudo ufw status` |
| --- | --- |
| SSH started ? | `sudo systemctl status ssh` |
| OS version ? | `cat /etc/os-release` |

# Users

| Check users in a group | `sudo getent group <group_name>` |
| --- | --- |
| Create a new user | `sudo adduser <user login>` |
| Create a new group | `sudo groupadd <new_group>` |
| Add a user to a group | `sudo usermod -aG <name_group> <name_user>` |
| Delete a user from a group  | `gpasswd --delete nom-utilisateur nom-groupe` |
| Delete a group | `delgroup <name_group>` |
| Delete a  user | `userdel <user_name>` |
| Check all the local users | `cut -d: -f1 /etc/passwd` |

# Hostname and partition

## What is a logical volume manager in a VM ?

In a virtual machine (VM), a logical volume manager (LVM) partition is a type of virtual disk partition that is managed by LVM. LVM is a system for managing logical volumes, or virtual disks, on a computer. It allows you to create, resize, and delete logical volumes without having to worry about the underlying physical layout of the disks.

In a VM, an LVM partition is created and managed in much the same way as it is on a physical machine. You can create an LVM partition by allocating space from one or more virtual disks and then creating logical volumes within that space. LVM allows you to resize logical volumes on-the-fly, so you can easily increase or decrease the size of your LVM partition as needed.

Using LVM in a VM can be useful for a number of reasons. For example, it allows you to easily resize your virtual disks as your storage needs change, without having to recreate the entire virtual machine. It can also make it easier to manage multiple virtual disks, as you can create and delete logical volumes as needed, rather than having to work with multiple physical disks.

## Commands

| Check the hostname of the machine | `hostnamectl` |
| --- | --- |
| Modify the hostname of the machine | `sudo hostnamectl set-hostname <new_hostname>` |
| Check the partition of the VM | `lsblk` (List BLocK device) |

# SUDO

The sudo log file is at : /var/log/sudo/sudo.log

La ligne `Defaults requiretty` dans le fichier `/etc/sudoers` spécifie que la commande `sudo` ne devrait être autorisée à s'exécuter que lorsqu'elle est lancée depuis un terminal ("tty"). Cela signifie que `sudo` ne sera pas autorisé à s'exécuter depuis des scripts ou d'autres sources non interactives.

## Explanation about authorization in sudoers

![explications-commandes-sudo-linux.jpg](https://github.com/FXC-ai/born2beroot/blob/main/explications-commandes-sudo-linux.jpg)

## Commands

| Check the sudo package is installed | `sudo dpkg -l | grep sudo` |
| --- | --- |
| Assign a new user to sudo group | `sudo usermod -aG sudo <user_name>` |
| Check the rules of sudo group | `sudo visudo` |

# UFW

## What is UFW ?

UFW (Uncomplicated Firewall) is a frontend for managing firewall rules in Debian and other Linux distributions. It is designed to be easy to use, and it allows users to manage the firewall rules on their system without having to know the details of the `iptables` command-line utility that is used to implement the firewall.

UFW provides a set of commands that can be used to enable or disable the firewall, allow or block incoming or outgoing connections, and view the current firewall rules. It is a good choice for users who need to set up a basic firewall on their system, but it may not be suitable for more advanced firewall configurations.

To use UFW, you must install the `ufw` package on your Debian system. Once it is installed, you can use the `ufw` command to manage the firewall. For example, to enable the firewall, you can use the command `ufw enable`. To allow incoming connections on a specific port, you can use a command like `ufw allow 22/tcp` to allow incoming connections on the SSH port (port 22).

## Commands

| Check the ufw rules | `sudo ufw status numbered` |
| --- | --- |
| Add a rule to open a port | `sudo ufw allow <port_number>` |
| Delete a rule | `sudo ufw delete <line_number>` |

# SSH

## What is SSH ?

SSH (Secure Shell) is a network protocol that allows you to securely connect to a remote computer over an unsecured network. It provides a secure channel between two devices, allowing you to remotely execute commands, transfer files, and establish secure tunnels for other network connections.

One of the main values of using SSH is that it provides a secure way to remotely access and manage devices over a network. It encrypts all data that is transmitted over the network, including passwords and other sensitive information, making it difficult for attackers to intercept and steal this data.

SSH is commonly used to remotely access and manage servers, as it allows administrators to securely log in to the servers and execute commands as if they were physically present at the console. It is also used to securely transfer files between devices, and to establish secure tunnels for other network connections, such as FTP, Telnet, and X11.

In addition to its security features, SSH is also convenient to use, as it does not require any special software or configuration on the client side. Most operating systems, including Linux, macOS, and Windows, include an SSH client as part of the default installation.

Overall, SSH is an important tool for securely accessing and managing devices over a network, and is widely used in a variety of settings, including system administration, network engineering, and software development.

## Commands

| Check SSH is properly installed | `sudo service ssh status` |
| --- | --- |
| Use SSH to connect to the VM | `ssh new_user@127.0.0.1 -p 4242` |

# CRON

## What is cron ?

On Debian systems, cron is a daemon that runs in the background and executes scheduled tasks at specified intervals. It reads a configuration file called the crontab, which specifies the commands to be run and the schedule on which they are to be executed.

Cron is commonly used to automate system maintenance tasks, such as deleting temporary files, rotating logs, and updating package repositories. It is also used to run custom scripts and programs at regular intervals.

To use cron on Debian, you can either create a crontab file for a specific user or create a script in the `/etc/cron.d` directory. The crontab file or script should specify the schedule and command to be run. For example, the following entry in a crontab file would run the `/usr/local/bin/backup.sh` script at 2:00 AM every day:

```bash
0 2 * * * /usr/local/bin/backup.sh
```

|Cron uses a special syntax for specifying the schedule, which consists of five fields: minute, hour, day of month, month, and day of week. The `*` character is a wildcard that specifies all possible values for the field.

Overall, cron is a useful tool for automating tasks on a Debian system, and can save time and effort by allowing you to schedule tasks to run automatically in the background.

## Commands

The script [monitoring.sh](http://monitoring.sh) is at `/usr/local/bin/`

Pour parametrer les scripts qui se lancent automatiquement : `sudo crontab -u root -e`

Pour arreter et relancer crontab : `sudo service cron stop` ******`sudo service cron start`

## monitoring.sh

```bash
arc=$(uname -a)
pcpu=$(grep "physical id" /proc/cpuinfo | cd | uniq | wc -l) 
vcpu=$(grep "^processor" /proc/cpuinfo | wc -l)
fram=$(free -m | awk '$1 == "Mem:" {print $2}')
uram=$(free -m | awk '$1 == "Mem:" {print $3}')
pram=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')
fdisk=$(df -BG | grep '^/dev/' | grep -v '/boot$' | awk '{ft += $2} END {print ft}')
udisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} END {print ut}')
pdisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} {ft+= $2} END {printf("%d"), ut/ft*100}')
cpul=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')
lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -eq 0 ]; then echo no; else echo yes; fi)
ctcp=$(ss -neopt state established | wc -l)
ulog=$(users | wc -w)
ip=$(hostname -I)
mac=$(ip link show | grep "ether" | awk '{print $2}')
cmds=$(journalctl _COMM=sudo | grep COMMAND | wc -l)
wall "	#Architecture: $arc
	#CPU physical: $pcpu
	#vCPU: $vcpu
	#Memory Usage: $uram/${fram}MB ($pram%)
	#Disk Usage: $udisk/${fdisk}Gb ($pdisk%)
	#CPU load: $cpul
	#Last boot: $lb
	#LVM use: $lvmu
	#Connections TCP: $ctcp ESTABLISHED
	#User log: $ulog
	#Network: IP $ip ($mac)
	#Sudo: $cmds cmd"
```

| uname -a | affiche toutes les informations disponibles sur le système, y compris le nom de l'OS, le nom du noyau, le nom de l'hôte, etc. |
| --- | --- |
| wc -l | La commande `wc`(word countd) est un programme utilisé en ligne de commande pour compter le nombre de lignes, de mots et de caractères dans un fichier ou une entrée standard. L'option `-l` permet de n'afficher que le nombre de lignes. |
| uniq | La commande `uniq` est un programme utilisé en ligne de commande pour afficher les lignes uniques d'un fichier ou d'une entrée standard. Elle ignore les lignes en double et n'affiche qu'une seule occurrence de chaque ligne unique. |
| sort | The `sort` command is a utility that is used to sort lines of text in a file or standard input in a particular order. It is available on most Unix-like systems, including Debian. |
| free -m | The `free` command is a utility that is used to display information about the amount of free and used memory on a system. It is available on most Unix-like systems, including Debian. This will show you the total amount of physical memory, the amount of used memory, the amount of free memory, and the amount of memory that is being used for buffers and cache. The `-m` option is for display in mgabyte. |
| df -BG | The `df` command is a utility that is used to display information about the available space on a file system. The `-B` option specifies that the values should be displayed in blocks, and the `-G`
 option specifies that the values should be displayed in gigabytes. |
| journalctl | `journalctl` is a command-line utility that is used to display the contents of the system journal on Linux systems. The system journal is a system log that stores messages from the kernel and from 
various system processes and services. |
| who -b | Renvoie la date et l’heure du dernier démarage |
| top -bn1 | `top` is a command-line utility that is used to display information about the **processes that are currently running** on a system. The `-b` option tells `top` to run in batch mode, and the `-n` option specifies the number of iterations to run. In this case, `top` is being run for one iteration only. |

# Sources

## Tutoriels Bor 2 Be Root

[Tutoriel pour l’installation avec les bonus](https://www.youtube.com/watch?v=OQEdjt38ZJA)

Tutoriel pour l’installation avec les bonus

[Born2BeRoot-Guide/README.md at main · pasqualerossi/Born2BeRoot-Guide](https://github.com/pasqualerossi/Born2BeRoot-Guide/blob/main/README.md)

[42-born2beroot/README.md at main · hanshazairi/42-born2beroot](https://github.com/hanshazairi/42-born2beroot/blob/main/README.md)

[Born2beroot](https://baigal.medium.com/born2beroot-e6e26dfb50ac)

[Born2beRoot Evaluation Form w/commands (without bonus)](https://docs.google.com/document/d/1-BwCO0udUP7MhRh81Y681zz0BalXtKFtte_FHJc6G4s/edit)

## Liens utiles

[How to Start, Stop, and Restart Cron Jobs](https://www.airplane.dev/blog/how-to-start-stop-and-restart-cron-jobs)

[1.10. Snapshots](https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/snapshots.html)

[fr/Keyboard - Debian Wiki](https://wiki.debian.org/fr/Keyboard)

[Commande sudo : comment configurer sudoers sous Linux ? | IT-Connect](https://www.it-connect.fr/commande-sudo-comment-configurer-sudoers-sous-linux/#D_Creer_une_nouvelle_regle_dans_sudoers)

# Quelques notions pour mieux comprendre

## What is a daemon ?

A daemon is a background process that runs on a Unix-like operating system. The word "daemon" is a shortened form of the word "demon," which was originally used to refer to background processes that were thought to be evil or malicious. Today, the word "daemon" is used more broadly to refer to any background process that runs on a Unix-like system, whether or not it is considered to be malevolent.

In the context of Debian, a daemon is a background process that is started at boot time and runs continuously until the system is shut down. Some common examples of daemons include `sshd`, the OpenSSH server daemon, and `httpd`, the Apache HTTP server daemon. Daemons usually run with a low level of privileges and are designed to perform specific tasks or services for the system or for users of the system.

## What is the purpose of the cut function ?

On Debian, the `cut` command is a command-line utility that is used to extract specific fields from a file or input. It can be used to extract fields from a delimited file (such as a tab-separated or comma-separated file) or from a file that uses a fixed-width format. By default, `cut` will extract fields based on the position of 
the field within the input, but it can also extract fields based on the delimiter that separates the fields.

## What is the purpose of the secure_path variable in /etc/sudoers ?

In the `sudoers` file on Debian (and other systems that use `sudo`), the `secure_path` variable specifies the search path that is used when running commands with `sudo`. It is used to restrict the directories that are searched for commands, in order to prevent unauthorized users from running malicious executables that are stored in directories that are writable by non-privileged users.

By default, the `secure_path` variable is set to a list of directories that are commonly used to store system executables, such as `/usr/local/sbin`, `/usr/local/bin`, `/usr/sbin`, `/usr/bin`, and `/sbin`. This means that when a user runs a command with `sudo`, `sudo` will only search for executables in these directories.

For example, if a user tries to run the `ls` command with `sudo`, `sudo` will search for the `ls` executable in the directories specified in the `secure_path` variable. If it is found in one of these directories, `sudo` will execute the command with the user's privileges. If it is not found, `sudo` will display an error message indicating that the command could not be found.

The `secure_path` variable can be modified in the `sudoers` file to add or remove directories from the search path. However, it is important to be cautious when modifying this variable, as changing it can potentially affect the security of the system.

## What is the difference between a physical disk and a virtual disk ?

A virtual disk is a file or set of files that resides on a physical disk, such as a hard drive or solid-state drive, and behaves like a physical disk. It is used to store data in a virtual machine (VM) or other virtualized environment.

A physical disk, on the other hand, is a real, physical storage device, such as a hard drive or solid-state drive, that is installed in a computer or other device. It is used to store data on the device itself.

There are several key differences between virtual disks and physical disks:

- Physicality: Virtual disks are purely virtual and exist as files on a physical disk, while physical disks are actual physical devices.
- Accessibility: Virtual disks can only be accessed through the virtualized environment in which they are used, while physical disks can be accessed directly by the computer or device in which they are installed.
- Modification: Virtual disks can be easily modified, such as by resizing or deleting them, while physical disks usually require more complex procedures to be modified.
- Performance: Virtual disks may have different performance characteristics than physical disks, depending on the underlying hardware and the virtualization software being used.
- Cost: Virtual disks may be less expensive than physical disks, as they do not require the purchase of additional hardware.
