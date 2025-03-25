# Exercice

Écrivez trois playbooks :

## Un premier playbook apache-debian.yml qui installe
### Apache sur l’hôte debian avec une page personnalisée
### Apache web server running on Debian Linux.

```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/apache-debian.yml -v
Using /home/vagrant/ansible/projets/ema/ansible.cfg as config file

PLAY [Install Apache on Debian] ************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [debian]

TASK [Install Apache package] **************************************************************************************
ok: [debian] => {"cache_update_time": 1742923017, "cache_updated": false, "changed": false}

TASK [Install custom web page] *************************************************************************************
changed: [debian] => {"changed": true, "checksum": "1d6ce6940435153fb8023520fc4a79bf38e82fc0", "dest": "/var/www/html/index.html", "gid": 0, "group": "root", "md5sum": "ab91621250c5389b60710bf715c248b3", "mode": "0644", "owner": "root", "size": 176, "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1742923083.435696-6788-29961703357911/source", "state": "file", "uid": 0}

TASK [Start and enable Apache service] *****************************************************************************
ok: [debian] => {"changed": false, "enabled": true, "name": "apache2", "state": "started", "status": {"ActiveEnterTimestamp": "Tue 2025-03-25 17:17:22 UTC", "ActiveEnterTimestampMonotonic": "1958739940", "ActiveExitTimestampMonotonic": "0", "ActiveState": "active", "After": "tmp.mount nss-lookup.target basic.target systemd-journald.socket remote-fs.target systemd-tmpfiles-setup.service network.target sysinit.target -.mount system.slice", "AllowIsolate": "no", "AssertResult": "yes", "AssertTimestamp": "Tue 2025-03-25 17:17:22 UTC", "AssertTimestampMonotonic": "1958703043", "Before": "shut": "no", "Type": "forking", "UID": "[not set]", "UMask": "0022", "UnitFilePreset": "enabled", "UnitFileState": "enabled", "UtmpMode": "init", "WantedBy": "multi-user.target", "Wants": "tmp.mount", "WatchdogSignal": "6", "WatchdogTimestampMonotonic": "0", "WatchdogUSec": "0"}}

PLAY RECAP *********************************************************************************************************
debian                     : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```
![image debian apache](./serveur_apache_debian.png)


## Un deuxième playbook apache-rocky.yml qui installe
### Apache sur l’hôte rocky avec une page personnalisée
### Apache web server running on Rocky Linux.

```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/apache-rocky.yml -v
Using /home/vagrant/ansible/projets/ema/ansible.cfg as config file

PLAY [Installer Apache sur Rocky Linux] ****************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [rocky]

TASK [Installer le paquet Apache] **********************************************************************************
ok: [rocky] => {"changed": false, "msg": "Nothing to do", "rc": 0, "results": []}

TASK [Install custom web page] *************************************************************************************
changed: [rocky] => {"changed": true, "checksum": "0c5271c6564b364f5d714558dada403f9df8ddcf", "dest": "/var/www/html/index.html", "gid": 0, "group": "root", "md5sum": "9a2bdb3cf3491d6dc37b4c9e2b3e2cb2", "mode": "0644", "owner": "root", "secontext": "system_u:object_r:httpd_sys_content_t:s0", "size": 175, "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1742922345.5985744-6340-25716457788299/source", "state": "file", "uid": 0}

TASK [Start and enable Apache service] *****************************************************************************
ok: [rocky] => {"changed": false, "enabled": true, "name": "httpd", "state": "started", "status": {"AccessSELinuxContext": "system_u:object_r:httpd_unit_file_t:s0", "ActiveEnterTimestamp": "Tue 2025-03-25 17:00:43 UTC", "ActiveEnterTimestampMonotonic": "1017070281", "ActiveExitTimestampMonotonic": "0", "ActiveState": "active", "After": "nss-lookup.target basic.target tmp.mount system.slice remote-fs.target -.mount network.target systemd-journald.socket sysinit.target systemd-tmpfiles-setup.service httpd-init.service", "AllowIsolate": "no", "AssertResult": "yes", "AssertTimestamp": "Tue 2025-03-25 17:00:43 UTC", "AssertTimestampMonotonic": "1016993338", "Before": "multi-user.target shutdown.target", "BlockIOAccounting": "no", "BlockIOWeight": "[not set]", "CPUAccounting": "yes", "CPUAffinityFromNUMA": "no", "CPUQuotaPerSecUSec": "infinity", "CPUQuotaPeriodUSec": "infinity", 

PLAY RECAP *********************************************************************************************************
rocky                      : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
![image rocky apache](./serveur_apache_rocky.png)

## Un troisième playbook apache-suse.yml qui installe
### Apache sur l’hôte suse avec une page personnalisée
### Apache web server running on SUSE Linux.

```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/apache-suse.yml -v
Using /home/vagrant/ansible/projets/ema/ansible.cfg as config file

PLAY [Install Apache on SUSE Linux] ********************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [suse]

TASK [Install Apache package] **************************************************************************************
changed: [suse] => {"changed": true, "cmd": ["/usr/bin/zypper", "--quiet", "--non-interactive", "--xmlout", "install", "--type", "package", "--auto-agree-with-licenses", "--no-recommends", "--", "+apache2"], "name": ["apache2"], "rc": 0, "state": 

TASK [Install custom web page] *************************************************************************************
changed: [suse] => {"changed": true, "checksum": "24ff385add59e840be719f1a608c20b0f88c6912", "dest": "/srv/www/htdocs/index.html", "gid": 0, "group": "root", "md5sum": "a5c7d31de7307e600fe99cd5f6bf527e", "mode": "0644", "owner": "root", "size": 174, "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1742922406.2091-6396-200275233817124/source", "state": "file", "uid": 0}

TASK [Start and enable Apache service] *****************************************************************************
changed: [suse] => {"changed": true, "enabled": true, "name": "apache2", "state": "started", "status": {"ActiveEnterTimestamp": "n/a", "ActiveEnterTimestampMonotonic": "0", "ActiveExitTimestamp": "n/a", "ActiveExitTimestampMonotonic": "0", "ActiveState": "inactive", "After": "remote-fs.target network.target -.mount nss-lookup.target basic.target system.slice systemd-journald.socket systemd-tmpfiles-setup.service tmp.mount time-sync.target sysinit.target", "AllowIsolate": "no", "AssertResult": "no", "AssertTimestamp": "n/a", "AssertTimestampMonotonic": "0", "Before": "plymouth-quit.service shutdown.target xdm.service getty@tty1.service", "BlockIOAccounting": "no", "BlockIOWeight": "[not set]", "CPUAccounting": "no", "CPUAffinityFromNUMA": "no", "CPUQuotaPerSecUSec": "infinity", "CPUQuotaPeriodUSec": "infinity"}}

PLAY RECAP *********************************************************************************************************
suse                       : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
![image suse apache](./serveur_apache_suse.png)