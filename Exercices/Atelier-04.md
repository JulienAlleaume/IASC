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

ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03

Et enfin le Ping ansible des trois hosts :

ansible all -i target01,target02,target03 -m ping