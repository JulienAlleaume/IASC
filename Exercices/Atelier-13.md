# Exercice


## Écrivez un playbook kernel.yml qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande uname -a et le module debug avec le paramètre msg.
## Essayez d’obtenir le même résultat en utilisant le paramètre var du module debug.
```sh
- hosts: all
  tasks:
    - name: Afficher les informations détaillées du noyau
      debug:
        msg: "{{ ansible_kernel }}"

    - name: Afficher les informations détaillées du noyau (avec le paramètre var)
      debug:
        var: ansible_kernel
```


```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/kernel.yml 

PLAY [all] *************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Afficher les informations détaillées du noyau] *******************************************************************************************************
ok: [rocky] => {
    "msg": "5.14.0-362.13.1.el9_3.x86_64"
}
ok: [debian] => {
    "msg": "6.1.0-17-amd64"
}
ok: [suse] => {
    "msg": "5.14.21-150500.55.39-default"
}

TASK [Afficher les informations détaillées du noyau (avec le paramètre var)] *******************************************************************************
ok: [rocky] => {
    "ansible_kernel": "5.14.0-362.13.1.el9_3.x86_64"
}
ok: [debian] => {
    "ansible_kernel": "6.1.0-17-amd64"
}
ok: [suse] => {
    "ansible_kernel": "5.14.21-150500.55.39-default"
}

PLAY RECAP *************************************************************************************************************************************************
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

## Écrivez un playbook packages.yml qui affiche le nombre total de paquets RPM installés sur les hôtes rocky et suse (rpm -qa | wc -l).

```sh
- hosts: 
    - rocky
    - suse
  tasks:
    - name: Afficher le nombre total de paquets RPM installés
      command: rpm -qa | wc -l
      register: rpm_count
    - debug:
        msg: "Nombre total de paquets RPM installés : {{ rpm_count.stdout }}"
```


/AUTHORS\n/usr/share/doc/haveged/COPYING\n/usr/share/doc/haveged/ChangeLog\n/usr/share/doc/haveged/README\n/usr/share/doc/haveged/havege_sample.c\n/usr/share/man/man8/haveged.8.gz"
}


```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/packages.yml

PLAY [rocky,suse] ******************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
PLAY RECAP *************************************************************************************************************************************************
rocky                      : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```