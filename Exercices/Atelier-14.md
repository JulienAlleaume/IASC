# Exercice

Écrivez trois playbooks pour afficher des informations sur chacun des Target Hosts :

## pkg-info.yml pour afficher le gestionnaire de paquets utilisé

```sh
- hosts: all
  tasks:
    - name: Déterminer le gestionnaire de paquets
      command: which {{ item }}
      register: package_manager
      loop:
        - apt-get
        - yum
        - dnf
        - zypper
      ignore_errors: true

    - name: Afficher le résultat
      debug:
        var: package_manager.result
```


```sh

vagrant@ansible ema]$ ansible-playbook playbooks/pkg-info.yml 

PLAY [all] *************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Déterminer le gestionnaire de paquets] ***************************************************************************************************************
changed: [debian] => (item=apt-get)
failed: [rocky] (item=apt-get) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "apt-get"], "delta": "0:00:00.003275", "end": "2025-03-26 09:41:26.097550", "item": "apt-get", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:41:26.094275", "stderr": "which: no apt-get in (/sbin:/bin:/usr/sbin:/usr/bin)", "stderr_lines": ["which: no apt-get in (/sbin:/bin:/usr/sbin:/usr/bin)"], "stdout": "", "stdout_lines": []}
failed: [suse] (item=apt-get) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "apt-get"], "delta": "0:00:00.003634", "end": "2025-03-26 09:39:52.766700", "item": "apt-get", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:39:52.763066", "stderr": "which: no apt-get in (/usr/sbin:/usr/bin:/sbin:/bin)", "stderr_lines": ["which: no apt-get in (/usr/sbin:/usr/bin:/sbin:/bin)"], "stdout": "", "stdout_lines": []}
failed: [debian] (item=yum) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "yum"], "delta": "0:00:00.002082", "end": "2025-03-26 09:39:39.633216", "item": "yum", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:39:39.631134", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
changed: [rocky] => (item=yum)
failed: [debian] (item=dnf) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "dnf"], "delta": "0:00:00.002280", "end": "2025-03-26 09:39:39.906418", "item": "dnf", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:39:39.904138", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
failed: [suse] (item=yum) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "yum"], "delta": "0:00:00.002600", "end": "2025-03-26 09:39:53.204283", "item": "yum", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:39:53.201683", "stderr": "which: no yum in (/usr/sbin:/usr/bin:/sbin:/bin)", "stderr_lines": ["which: no yum in (/usr/sbin:/usr/bin:/sbin:/bin)"], "stdout": "", "stdout_lines": []}
failed: [debian] (item=zypper) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "zypper"], "delta": "0:00:00.002437", "end": "2025-03-26 09:39:40.173109", "item": "zypper", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:39:40.170672", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
...ignoring
changed: [rocky] => (item=dnf)
failed: [suse] (item=dnf) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "dnf"], "delta": "0:00:00.003186", "end": "2025-03-26 09:39:53.615037", "item": "dnf", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:39:53.611851", "stderr": "which: no dnf in (/usr/sbin:/usr/bin:/sbin:/bin)", "stderr_lines": ["which: no dnf in (/usr/sbin:/usr/bin:/sbin:/bin)"], "stdout": "", "stdout_lines": []}
failed: [rocky] (item=zypper) => {"ansible_loop_var": "item", "changed": true, "cmd": ["which", "zypper"], "delta": "0:00:00.003140", "end": "2025-03-26 09:41:27.343305", "item": "zypper", "msg": "non-zero return code", "rc": 1, "start": "2025-03-26 09:41:27.340165", "stderr": "which: no zypper in (/sbin:/bin:/usr/sbin:/usr/bin)", "stderr_lines": ["which: no zypper in (/sbin:/bin:/usr/sbin:/usr/bin)"], "stdout": "", "stdout_lines": []}
...ignoring
changed: [suse] => (item=zypper)
...ignoring

TASK [Afficher le résultat] ********************************************************************************************************************************
ok: [rocky] => {
    "package_manager.results": [
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "apt-get"
            ],
            "delta": "0:00:00.003275",
            "end": "2025-03-26 09:41:26.097550",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which apt-get",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "apt-get",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:41:26.094275",
            "stderr": "which: no apt-get in (/sbin:/bin:/usr/sbin:/usr/bin)",
            "stderr_lines": [
                "which: no apt-get in (/sbin:/bin:/usr/sbin:/usr/bin)"
            ],
            "stdout": "",
            "stdout_lines": []
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "yum"
            ],
            "delta": "0:00:00.003145",
            "end": "2025-03-26 09:41:26.523836",
            "failed": false,
            "invocation": {
                "module_args": {
                    "_raw_params": "which yum",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "yum",
            "msg": "",
            "rc": 0,
            "start": "2025-03-26 09:41:26.520691",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "/bin/yum",
            "stdout_lines": [
                "/bin/yum"
            ]
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "dnf"
            ],
            "delta": "0:00:00.003278",
            "end": "2025-03-26 09:41:26.946426",
            "failed": false,
            "invocation": {
                "module_args": {
                    "_raw_params": "which dnf",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "dnf",
            "msg": "",
            "rc": 0,
            "start": "2025-03-26 09:41:26.943148",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "/bin/dnf",
            "stdout_lines": [
                "/bin/dnf"
            ]
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "zypper"
            ],
            "delta": "0:00:00.003140",
            "end": "2025-03-26 09:41:27.343305",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which zypper",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "zypper",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:41:27.340165",
            "stderr": "which: no zypper in (/sbin:/bin:/usr/sbin:/usr/bin)",
            "stderr_lines": [
                "which: no zypper in (/sbin:/bin:/usr/sbin:/usr/bin)"
            ],
            "stdout": "",
            "stdout_lines": []
        }
    ]
}
ok: [debian] => {
    "package_manager.results": [
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "apt-get"
            ],
            "delta": "0:00:00.002787",
            "end": "2025-03-26 09:39:39.399490",
            "failed": false,
            "invocation": {
                "module_args": {
                    "_raw_params": "which apt-get",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "apt-get",
            "msg": "",
            "rc": 0,
            "start": "2025-03-26 09:39:39.396703",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "/usr/bin/apt-get",
            "stdout_lines": [
                "/usr/bin/apt-get"
            ]
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "yum"
            ],
            "delta": "0:00:00.002082",
            "end": "2025-03-26 09:39:39.633216",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which yum",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "yum",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:39:39.631134",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "",
            "stdout_lines": []
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "dnf"
            ],
            "delta": "0:00:00.002280",
            "end": "2025-03-26 09:39:39.906418",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which dnf",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "dnf",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:39:39.904138",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "",
            "stdout_lines": []
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "zypper"
            ],
            "delta": "0:00:00.002437",
            "end": "2025-03-26 09:39:40.173109",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which zypper",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "zypper",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:39:40.170672",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "",
            "stdout_lines": []
        }
    ]
}
ok: [suse] => {
    "package_manager.results": [
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "apt-get"
            ],
            "delta": "0:00:00.003634",
            "end": "2025-03-26 09:39:52.766700",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which apt-get",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "apt-get",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:39:52.763066",
            "stderr": "which: no apt-get in (/usr/sbin:/usr/bin:/sbin:/bin)",
            "stderr_lines": [
                "which: no apt-get in (/usr/sbin:/usr/bin:/sbin:/bin)"
            ],
            "stdout": "",
            "stdout_lines": []
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "yum"
            ],
            "delta": "0:00:00.002600",
            "end": "2025-03-26 09:39:53.204283",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which yum",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "yum",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:39:53.201683",
            "stderr": "which: no yum in (/usr/sbin:/usr/bin:/sbin:/bin)",
            "stderr_lines": [
                "which: no yum in (/usr/sbin:/usr/bin:/sbin:/bin)"
            ],
            "stdout": "",
            "stdout_lines": []
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "dnf"
            ],
            "delta": "0:00:00.003186",
            "end": "2025-03-26 09:39:53.615037",
            "failed": true,
            "invocation": {
                "module_args": {
                    "_raw_params": "which dnf",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "dnf",
            "msg": "non-zero return code",
            "rc": 1,
            "start": "2025-03-26 09:39:53.611851",
            "stderr": "which: no dnf in (/usr/sbin:/usr/bin:/sbin:/bin)",
            "stderr_lines": [
                "which: no dnf in (/usr/sbin:/usr/bin:/sbin:/bin)"
            ],
            "stdout": "",
            "stdout_lines": []
        },
        {
            "ansible_loop_var": "item",
            "changed": true,
            "cmd": [
                "which",
                "zypper"
            ],
            "delta": "0:00:00.002303",
            "end": "2025-03-26 09:39:53.989977",
            "failed": false,
            "invocation": {
                "module_args": {
                    "_raw_params": "which zypper",
                    "_uses_shell": false,
                    "argv": null,
                    "chdir": null,
                    "creates": null,
                    "executable": null,
                    "removes": null,
                    "stdin": null,
                    "stdin_add_newline": true,
                    "strip_empty_ends": true
                }
            },
            "item": "zypper",
            "msg": "",
            "rc": 0,
            "start": "2025-03-26 09:39:53.987674",
            "stderr": "",
            "stderr_lines": [],
            "stdout": "/usr/bin/zypper",
            "stdout_lines": [
                "/usr/bin/zypper"
            ]
        }
    ]
}

PLAY RECAP *************************************************************************************************************************************************
debian                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1   
rocky                      : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1   
suse                       : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1
```

## python-info.yml pour afficher la version de Python installée
```sh
- hosts: all
  tasks:
    - name: Afficher la version de Python installée
      command: python3 --version
      register: python_version
      changed_when: false

    - name: Afficher le résultat
      debug:
        var: python_version.stdout

```
```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/python-info.yml 

PLAY [all] *************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Afficher la version de Python installée] *************************************************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Afficher le résultat] ********************************************************************************************************************************
ok: [rocky] => {
    "python_version.stdout": "Python 3.9.18"
}
ok: [debian] => {
    "python_version.stdout": "Python 3.11.2"
}
ok: [suse] => {
    "python_version.stdout": "Python 3.6.15"
}

PLAY RECAP *************************************************************************************************************************************************
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```


## dns-info.yml pour afficher le(s) serveur(s) DNS utilisé(s)

```sh
- hosts: all
  tasks:
    - name: Afficher les serveurs DNS utilisés
      command: cat /etc/resolv.conf
      register: dns_servers
      changed_when: false

    - name: Afficher le résultat
      debug:
        var: dns_servers.stdout_lines

```

```sh
[vagrant@ansible ema]$ ansible-playbook playbooks/dns-info.yml 

PLAY [all] *************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Afficher les serveurs DNS utilisés] ******************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Afficher le résultat] ********************************************************************************************************************************
ok: [rocky] => {
    "dns_servers.stdout_lines": [
        "# Generated by NetworkManager",
        "search mines-ales.fr",
        "nameserver 10.0.2.3",
        "options single-request-reopen",
        "options single-request-reopen",
        "options single-request-reopen",
        "options single-request-reopen",
        "options single-request-reopen"
    ]
}
ok: [debian] => {
    "dns_servers.stdout_lines": [
        "# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)",
        "#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN",
        "# 127.0.0.53 is the systemd-resolved stub resolver.",
        "# run \"resolvectl status\" to see details about the actual nameservers.",
        "",
        "nameserver 4.2.2.1",
        "nameserver 4.2.2.2",
        "nameserver 208.67.220.220",
        "search mines-ales.fr"
    ]
}
ok: [suse] => {
    "dns_servers.stdout_lines": [
        "### /etc/resolv.conf is a symlink to /run/netconfig/resolv.conf",
        "### autogenerated by netconfig!",
        "#",
        "# Before you change this file manually, consider to define the",
        "# static DNS configuration using the following variables in the",
        "# /etc/sysconfig/network/config file:",
        "#     NETCONFIG_DNS_STATIC_SEARCHLIST",
        "#     NETCONFIG_DNS_STATIC_SERVERS",
        "#     NETCONFIG_DNS_FORWARDER",
        "# or disable DNS configuration updates via netconfig by setting:",
        "#     NETCONFIG_DNS_POLICY=''",
        "#",
        "# See also the netconfig(8) manual page and other documentation.",
        "#",
        "### Call \"netconfig update -f\" to force adjusting of /etc/resolv.conf.",
        "search mines-ales.fr",
        "nameserver 10.0.2.3"
    ]
}

PLAY RECAP *************************************************************************************************************************************************
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```


