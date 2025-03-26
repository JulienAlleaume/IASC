# Exercice

---
- hosts: all

  tasks:

    - name: Install Chrony package (Debian)
      apt:
        name: chrony
        state: present
      when: ansible_distribution == 'Debian'

    - name: Install Chrony package (Rocky Linux)
      dnf:
        name: chrony
        state: present
      when: ansible_distribution == 'Rocky'

    - name: Install Chrony package (SUSE Linux)
      zypper:
        name: chrony
        state: present
      when: ansible_distribution == 'openSUSE Leap'

    - name: Install Chrony package (Ubuntu)
      apt:
        name: chrony
        state: present
      when: ansible_distribution == 'Ubuntu'

    - name: Create Chrony configuration file (Debian/Ubuntu)
      file:
        path: /etc/chrony.conf
        state: touch
      when: ansible_distribution in ['Debian', 'Ubuntu']

    - name: Configure Chrony (Debian/Ubuntu)
      copy:
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
      when: ansible_distribution in ['Debian', 'Ubuntu']
      notify: Restart Chrony service

    - name: Configure Chrony (Rocky Linux/SUSE)
      copy:
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
      when: ansible_distribution in ['Rocky', 'openSUSE Leap']
      notify: Restart Chrony service


    - name: Start and enable Chrony service
      systemd:
        name: "{{ chrony_service }}"
        state: started
        enabled: yes
      vars:
        chrony_service:
          Debian: chrony
          Rocky: chronyd
          'openSUSE Leap': chronyd

  handlers:
    - name: Restart Chrony service
      systemd:
        name: "{{ chrony_service }}"
        state: restarted