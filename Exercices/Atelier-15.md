# Exercice

## Le premier playbook chrony-01.yml utilisera les modules de gestion de paquets natifs apt, dnf et zypper et s’inspirera de la méthode « gros sabots » utilisée plus haut dans cet article.
```yml
---

- name: Install Chrony with package manager of hosts
  hosts: all
  gather_facts: true
  vars:
    chrony_service_debian_ubuntu: "chrony"
    chrony_service_redhat_suse: "chronyd"
    chrony_conf_path_debian_ubuntu: "/etc/chrony/chrony.conf"
    chrony_conf_path_redhat_suse: "/etc/chrony.conf"
  tasks:
    - name: Install chrony on Debian/Ubuntu
      ansible.builtin.package:
        name: chrony
        state: present
        update_cache: true
      when: ansible_pkg_mgr == 'apt'
    
    - name: Install chrony on RHEL based systems
      ansible.builtin.dnf:
        name: chrony
        state: present
        update_cache: true
      when: ansible_pkg_mgr == 'dnf'
    
    - name: Install chrony on SUSE based systems
      ansible.builtin.zypper:
        name: chrony
        state: present
        update_cache: true
      when: ansible_pkg_mgr == 'zypper'
    
    - name: Configure chrony on Debian/Ubuntu
      ansible.builtin.copy:
        content: |-
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: "{{ chrony_conf_path_debian_ubuntu }}"
        mode: '0644'
      notify: restart chrony on Debian/Ubuntu
      when: ansible_os_family == 'Debian'
    
    - name: Configure chrony on RedHat/SUSE systems
      ansible.builtin.copy:
        content: |-
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: "{{ chrony_conf_path_redhat_suse }}"
        mode: '0644'
      notify: restart chrony on RedHat/SUSE
      when: ansible_os_family == 'RedHat' or ansible_os_family == 'Suse'
      
  handlers:
    - name: restart chrony on Debian/Ubuntu
      ansible.builtin.systemd:
        name: "{{ chrony_service_debian_ubuntu }}"
        state: restarted
        enabled: yes
      when: ansible_os_family == 'Debian'
      
    - name: restart chrony on RedHat/SUSE
      ansible.builtin.systemd:
        name: "{{ chrony_service_redhat_suse }}"
        state: restarted
        enabled: yes
      when: ansible_os_family == 'RedHat' or ansible_os_family == 'Suse'
```

## Test du playbook :
```sh
[vagrant@ansible ema]$ ansible-playbook chrony-01.yml
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
TASK [Install chrony on Debian/Ubuntu] ************************************************************************************************************************************************
ok: [debian]
ok: [ubuntu]
skipping: [suse]
skipping: [rocky]
TASK [Install chrony on RHEL based systems] ************************************************************************************************************************************************
skipping: [debian]
skipping: [ubuntu]
skipping: [suse]
ok: [rocky]
TASK [Install chrony on SUSE based systems] ************************************************************************************************************************************************
skipping: [debian]
skipping: [ubuntu]
ok: [suse]
skipping: [rocky]
TASK [Configure chrony on Debian/Ubuntu]*************************************************************************************************************************************************
changed: [ubuntu]
changed: [debian]
skipping: [rocky]
skipping: [suse]
TASK [Configure chrony on RedHat/SUSE systems]*************************************************************************************************************************************************
skipping: [ubuntu]
skipping: [debian]
changed: [rocky]
changed: [suse]
RUNNING HANDLER [restart chrony] ************************************************************************************************************************************************
changed: [ubuntu]
changed: [debian]
changed: [rocky] 
changed: [suse]
```

Tout c'est bien déroulé.


## Le deuxième playbook chrony-02.yml définira trois variables chrony_package, chrony_service et chrony_confdir et utilisera le module de gestion de paquets générique package.
```yml
---

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
      ansible.builtin.copy:
        content: |-
          server 1.fr.pool.ntp.org iburst
          server 0.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
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

## Test du playbook :
```sh
[vagrant@ansible ema]$ ansible-playbook chrony-02.yml
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