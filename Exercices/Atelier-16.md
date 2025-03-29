# Exercice

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