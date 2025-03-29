
# Exercice
## Démarrez la VM ubuntu depuis le répertoire atelier-01.
cd /home/ema/formation-ansible/atelier-01
vagrant up ubuntu

## Connectez-vous à cette VM.
vagrant ssh ubuntu

## Rafraîchissez les informations sur les paquets.
sudo apt update

## Recherchez le paquet ansible avec les options qui vont bien.
sudo apt search ^ansible

## Installez le paquet officiel fourni par la distribution.
sudo apt install ansible

## Vérifiez si l’installation s’est bien déroulée.
vagrant@ubuntu:~$ sudo apt list ansible
Listing... Done
ansible/jammy,now 2.10.7+merged+base+2.10.8+dfsg-1 all [installed]


## Notez la version d’Ansible.
vagrant@ubuntu:~$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]


## Déconnectez-vous et supprimez la VM.
exit
vagrant destroy -f
