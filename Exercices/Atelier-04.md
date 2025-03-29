# Exercice
# Faites le nécessaire pour réussir un ping Ansible

## Demarrage des VM
cd ~/formation-ansible/atelier-03
vagrant up

## SSH du controler et ajout dans le etc host des ip/nom des autres VM

`vagrant ssh control`

Voici les quatre machines virtuelles Ubuntu 22.04 de cet atelier :
``` sh
Machine virtuelle 	Adresse IP
control 	192.168.56.10
target01 	192.168.56.20
target02 	192.168.56.30
target03 	192.168.56.40
```

vi /etc/host
```sh
127.0.0.1 localhost
127.0.1.1 vagrant
192.168.56.10 control
192.168.56.20 target01
192.168.56.30 target02
192.168.56.40 target03
[...]
```

Test ping des hosts ajouté  :

for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```sh
PING target01 (192.168.56.20) 56(84) bytes of data.

--- target01 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.438/0.438/0.438/0.000 ms
PING target02 (192.168.56.30) 56(84) bytes of data.

--- target02 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.395/0.395/0.395/0.000 ms
PING target03 (192.168.56.40) 56(84) bytes of data.

--- target03 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.294/0.294/0.294/0.000 ms
```

ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
```sh
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
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

Et enfin le Ping ansible des trois hosts :
```sh
ansible all -i target01,target02,target03 -m ping
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
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
```