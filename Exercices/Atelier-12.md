# Exercice

## Écrivez un playbook myvars1.yml qui affiche respectivement votre voiture et votre moto préférée en utilisant le module debug et deux variables mycar et mybike définies en tant que play vars.


## En utilisant les extra vars, remplacez successivement l’une et l’autre marque – puis les deux à la fois – avant d’exécuter le play.


## Écrivez un playbook myvars2.yml qui fait essentiellement la même chose que myvars1.yml, mais en utilisant une tâche avec set_fact pour définir les deux variables.


## Là aussi, essayez de remplacer les deux variables en utilisant des extra vars avant l’exécution du play.


## Écrivez un playbook myvars3.yml qui affiche le contenu des deux variables mycar et mybike mais sans les définir. Avant d’exécuter le playbook, définissez VW et BMW comme valeurs par défaut pour mycar et mybike pour tous les hôtes, en utilisant l’endroit approprié.


## Effectuez le nécessaire pour remplacer VW et BMW par Mercedes et Honda sur l’hôte target02.


## Écrivez un playbook display_user.yml qui affiche un utilisateur et son mot de passe correspondant à l’aide des variables user et password. Ces deux variables devront être saisies de manière interactive pendant l’exécution du playbook. Les valeurs par défaut seront microlinux pour user et yatahongaga pour password. Le mot de passe ne devra pas s’afficher pendant la saisie.




myvars1.yml :
```sh
yaml

- hosts: all
  vars:
    mycar: Renault
    mybike: Honda
  tasks:
    - name: Afficher ma voiture et ma moto préférées
      debug:
        msg: 
          - "Ma voiture préférée est une {{ mycar }}"
          - "Ma moto préférée est une {{ mybike }}"
```
myvars2.yml :
```sh
yaml

- hosts: all
  tasks:
    - name: Définir mes variables préférées
      set_fact:
        mycar: Renault
        mybike: Honda
    - name: Afficher ma voiture et ma moto préférées
      debug:
        msg:
          - "Ma voiture préférée est une {{ mycar }}"
          - "Ma moto préférée est une {{ mybike }}"
```
myvars3.yml :
```sh
yaml

- hosts: all
  vars_files:
    - myvars3_defaults.yml
  tasks:
    - name: Afficher mes variables préférées
      debug:
        msg:
          - "Ma voiture préférée est une {{ mycar }}"
          - "Ma moto préférée est une {{ mybike }}"
```
myvars3_defaults.yml :
```sh
yaml

mycar: VW
mybike: BMW
```
display_user.yml :
```sh
yaml

- hosts: all
  vars:
    user: microlinux
    password: yatahongaga
  tasks:
    - name: Afficher l'utilisateur et le mot de passe
      debug:
        msg:
          - "Utilisateur : {{ user }}"
          - "Mot de passe : {{ password }}"
      no_log: true
  vars_prompt:
    - name: user
      prompt: "Entrez l'utilisateur"
      default: "{{ user }}"
    - name: password
      prompt: "Entrez le mot de passe"
      private: yes
      default: "{{ password }}"
```