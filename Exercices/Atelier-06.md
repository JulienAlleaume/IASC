# Exercice

## Préparatio net allumage des VM
cd ~/formation-ansible/atelier-06
vagrant up
vagrant ssh control

## Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.
vi /etc/hosts
```sh
127.0.0.1 localhost
127.0.1.1 vagrant
192.168.56.10 control
192.168.56.20 target01
192.168.56.30 target02
192.168.56.40 target03
[...]
```

## Configurez l’authentification par clé SSH avec les trois Target Hosts.
ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
```sh
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
```
Genere une clé ssh :
```sh
ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:5XzOOFEvPsgrOR0wxpHCZ5M29PkoqY4AFBORxvAoYjU vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|+=+E . ..o       |
| *+ . o X. .     |
|=o.    * ++ .    |
|=       == + .   |
|.      .So= + .  |
| .     . o.O .   |
|  .   .  o=.=    |
|   . o  + .o .   |
|    . .  o.      |
+----[SHA256]-----+
```

On copy la clé ssh sur les trois machines, il nous demande le mots de passe :
```sh
vagrant@control:~$ ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target02'"
and check to make sure that only the key(s) you wanted were added.

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target03'"
and check to make sure that only the key(s) you wanted were added.
```

## Installez Ansible.
sudo apt update
sudo apt install ansible

## Envoyez un premier ping Ansible sans configuration.
ansible all -i target01,target02,target03 -m ping
```sh
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## Créez un répertoire de projet ~/monprojet.
mkdir -pv ~/ansible/projets/monprojet
```
mkdir: created directory '/home/vagrant/ansible'
mkdir: created directory '/home/vagrant/ansible/projets'
mkdir: created directory '/home/vagrant/ansible/projets/monprojet'
```

## Créez un fichier vide ansible.cfg dans ce répertoire.
touch ~/ansible/projets/ansible.cfg

## Vérifiez si ce fichier est bien pris en compte par Ansible.
```sh
vagrant@control:~/ansible/projets/monprojet$ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/ansible/projets/monprojet/ansible.cfg
```

## Spécifiez un inventaire nommé hosts.
## Activez la journalisation dans ~/journal/ansible.log.

vagrant@control:~/ansible/projets/monprojet$ cat host 
```sh
[all]
target01
target02
target03
```

mkdir -v ~/logs

vagrant@control:~/ansible/projets/monprojet$ cat ansible.cfg 
```sh
[defaults]
inventory = ./host
log_path = ~/logs/ansible.log
```
## Testez la journalisation.
```sh
vagrant@control:~/ansible/projets/monprojet$ cat ~/logs/ansible.log 
2025-03-24 10:10:49,540 p=2903 u=vagrant n=ansible | [WARNING]: Unable to parse /home/vagrant/monprojet/hosts as an inventory source

2025-03-24 10:10:49,541 p=2903 u=vagrant n=ansible | [WARNING]: No inventory was parsed, only implicit localhost is available

2025-03-24 10:10:49,543 p=2903 u=vagrant n=ansible | [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

[...]
```

## Créez un groupe [testlab] avec vos trois Target Hosts.
## Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
## Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.

```sh
vagrant@control:~/ansible/projets/monprojet$ cat host 
[all]
target01
target02
target03

[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
ansible_become=true
```

## Envoyez un ping Ansible vers le groupe de machines [all].
```sh
vagrant@control:~/ansible/projets/monprojet$ ansible all -m ping
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.
```sh
vagrant@control:~/ansible/projets/monprojet$ ansible all -a "head -n 1 /etc/shadow"
target03 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target01 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target02 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
```

## Quittez le Control Host et supprimez toutes les VM de l’atelier.
exit
vagrant destroy -f

