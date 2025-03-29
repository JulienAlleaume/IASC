# Exercice
## Voici mes fichers de configuration jinga, template et vars :
Templates:
chrony.conf.j2
```conf
{# templates/chrony.conf.j2 #}
# {{ chrony_confdir }}
server 1.fr.pool.ntp.org iburst
server 0.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

Vars:
```sh
---  # vars/chrony02_debian.yml

chrony_package: "chrony"
chrony_service: "chrony"
chrony_confdir: "/etc/chrony/chrony.conf"
```


```sh
---  # vars/chrony02_opensuse-leap.yml

chrony_package: "chrony"
chrony_service: "chronyd"
chrony_confdir: "/etc/chrony.conf"
```

```sh
---  # vars/chrony02_rocky.yml

chrony_package: "chrony"
chrony_service: "chronyd"
chrony_confdir: "/etc/chrony.conf"
```

```sh
---  # vars/chrony02_ubuntu.yml

chrony_package: "chrony"
chrony_service: "chrony"
chrony_confdir: "/etc/chrony/chrony.conf"
```

```sh
--- # chrony.yml

- name: Install Chrony with package manager of hosts
  hosts: all 
  gather_facts: true
  tasks:

    - name: Include vars
      ansible.builtin.include_vars: >
        chrony02_{{ansible_distribution|lower|replace(" ", "-")}}.yml

    - name: Install chrony on Dall hosts
      ansible.builtin.package:
        name: "{{ chrony_package }}"
        state: present
        update_cache: true
    
    - name: Configure chrony
      ansible.builtin.template:
        src: chrony.conf.j2
        dest: "{{ chrony_confdir }}"
        mode: '0644'
      notify: restart chrony
      
  handlers:
    - name: restart chrony
      ansible.builtin.systemd:
        name: "{{ chrony_service }}"
        state: restarted
        enabled: yes
```



## Test de la configuration 
``` sh
[vagrant@ansible ema]$ ansible-playbook chrony.yml
PLAY [Install Chrony with package manager of hosts] *************************************************************************************************************************************************

TASK [Gathering Facts]*************************************************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [ubuntu]
ok: [rocky]
TASK [Include vars]*************************************************************************************************************************************************
ok: [rocky]
ok: [debian]
ok: [suse]
ok: [ubuntu]
TASK [Install chrony on Dall hosts] ************************************************************************************************************************************************
ok: [debian]
ok: [ubuntu]
ok: [suse]
ok: [rocky]
TASK [Configure chrony]*************************************************************************************************************************************************
changed: [ubuntu]
changed: [debian]
changed: [rocky]
changed: [suse]
RUNNING HANDLER [restart chrony] ************************************************************************************************************************************************
changed: [ubuntu]
changed: [debian]
changed: [rocky] 
changed: [suse]
PLAY RECAP *************************************************************************************************************************************************
debian
rocky
suse
ubuntu
[vagrant@ansible ema] $
: ok=5 changed=2 : ok=5 changed=2 : ok=5 changed=2 : ok=5 changed=2
unreachable=0 failed=0 skipped=0 unreachable=0 failed=0 skipped=0 unreachable=0 failed=0 unreachable=0 failed=0
rescued=0 ignored=0 rescued=0 skipped=0 rescued=0 skipped=0 rescued=0
ignored=0
ignored=0
ignored=0
```

## Verifiaction du bon fonctionnement du playbook sur les VM

```sh
[ema@sandbox-mpu: atelier-18] $ vagrant ssh rocky Last login: Sat Mar 29 12:58:45 2025 from 192.168.56.10 [vagrant@rocky ~]$ cat /etc/chrony.conf
# /etc/chrony.conf
server 1.fr.pool.ntp.org iburst
server 0.fr.pool.ntp.org iburst
server 2. fr.pool.ntp.org iburst server 3.fr.pool.ntp.org iburst driftfile
/var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony

[ema@sandbox-mpu: atelier-18] $ vagrant ssh debian
Last login: Sat Mar 29 12:59:55 2025 from 192.168.56.10 vagrant@debian:~$ cat /etc/chrony/chrony.conf
# /etc/chrony/chrony.conf
server 1.fr.pool.ntp.org iburst server 0. fr.pool.ntp.org iburst server 2. fr.pool.ntp.org iburst server 3.fr.pool.ntp.org iburst driftfile /var/lib/chrony/drift makestep 1.0 3 rtcsync
logdir /var/log/chrony
```