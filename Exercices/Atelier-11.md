# Exercice

Écrivez un playbook chrony.yml qui assure la synchronisation NTP de tous vos Target Hosts :

## Installez le paquet chrony.
## Activez et démarrez le service chronyd correspondant.
```sh
---
- name: Installer et configurer Chrony
  hosts: all
  become: true
  tasks:
    - name: Installer le paquet chrony
      ansible.builtin.dnf:
        name: chrony
        state: present

    - name: Activer et démarrer le service chronyd
      ansible.builtin.systemd:
        name: chronyd
        state: started
        enabled: true
...
```

## Jetez un œil sur le fichier de configuration /etc/chrony.conf fourni par défaut.
Le fichier ce base sur le serveur public de temps de pool 2.rocky.pool.ntp.org iburst.

## Installez une configuration personnalisée (cf. ci-dessous).
## Prenez en compte cette nouvelle configuration.
```sh
# /etc/chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```
Je rajoute a mon playbook :
```sh
[...]
 - name: Déployer la configuration chrony personnalisée
      ansible.builtin.copy:
        dest: /etc/chrony.conf
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        mode: '0644'
        owner: root
        group: root
      notify: Redémarrer le service chronyd pour appliquer la nouvelle configuration

  handlers:

    - name: Redémarrer le service chronyd pour appliquer la nouvelle configuration
      ansible.builtin.systemd:
        name: chronyd
        state: restarted
[...]
```

## Vérifiez la syntaxe correcte de votre playbook chrony.yml.
[vagrant@control ema]$ yamllint playbooks/chrony.yml 
playbooks/chrony.yml
  32:81     error    line too long (84 > 80 characters)  (line-length)
  36:81     error    line too long (82 > 80 characters)  (line-length)

Erreur inutile.

## Vérifiez l’idempotence de votre playbook.

```sh
[vagrant@control ema]$ ansible-playbook playbooks/chrony.yml -v
Using /home/vagrant/ansible/projets/ema/ansible.cfg as config file

PLAY [Installer et configurer Chrony] **********************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [target02]
ok: [target01]
ok: [target03]

TASK [Installer le paquet chrony] **************************************************************************************************************************
ok: [target03] => {"changed": false, "msg": "Nothing to do", "rc": 0, "results": []}
ok: [target02] => {"changed": false, "msg": "Nothing to do", "rc": 0, "results": []}
ok: [target01] => {"changed": false, "msg": "Nothing to do", "rc": 0, "results": []}

TASK [Activer et démarrer le service chronyd] **************************************************************************************************************
ok: [target02] => {"changed": false, "enabled": true, "name": "chronyd", "state": "started", "status": {"AccessSELinuxContext": "system_u:object_r:chronyd_unit_file_t:s0", "ActiveEnterTimestamp": "Wed 2025-03-26 07:20:05 UTC", "ActiveEnterTimestampMonotonic": "910355655", "ActiveExitTimestamp": "Wed 2025-03-26 07:20:05 UTC", "ActiveExitTimestampMonotonic": "910307957", "ActiveState": "active", "After": "basic.target ntpd.service sysinit.target ntpdate.service -.mount sntp.service system.slice systemd-tmpfiles-setup.service tmp.mount systemd-journald.socket", "AllowIsolate": "no", "AssertResult": "yes", "AssertTimestamp": "Wed 2025-03-26 07:20:05 UTC", "AssertTimestampMonotonic": "910310516", "Before": "multi-user.target shutdown.target", ""0"}}

TASK [Déployer la configuration chrony personnalisée] ******************************************************************************************************
ok: [target02] => {"changed": false, "checksum": "a83aabba5a98e45e3e950eb08fc8b13b89483e5d", "dest": "/etc/chrony.conf", "gid": 0, "group": "root", "mode": "0644", "owner": "root", "path": "/etc/chrony.conf", "secontext": "system_u:object_r:etc_t:s0", "size": 206, "state": "file", "uid": 0}
ok: [target01] => {"changed": false, "checksum": "a83aabba5a98e45e3e950eb08fc8b13b89483e5d", "dest": "/etc/chrony.conf", "gid": 0, "group": "root", "mode": "0644", "owner": "root", "path": "/etc/chrony.conf", "secontext": "system_u:object_r:etc_t:s0", "size": 206, "state": "file", "uid": 0}
ok: [target03] => {"changed": false, "checksum": "a83aabba5a98e45e3e950eb08fc8b13b89483e5d", "dest": "/etc/chrony.conf", "gid": 0, "group": "root", "mode": "0644", "owner": "root", "path": "/etc/chrony.conf", "secontext": "system_u:object_r:etc_t:s0", "size": 206, "state": "file", "uid": 0}

PLAY RECAP *************************************************************************************************************************************************
target01                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
