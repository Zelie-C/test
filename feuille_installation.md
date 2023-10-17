#Feuille Installation
## Installation environnement UNIX sous Windows

Fiche d'installation sur un PC sous Windows 10

### Installation WSL 2 avec une distribution Ubuntu sous Windows

1. Ouvrir **PowerShell**
2. Dans **PowerShell**, installer **WSL** avec la commande :
```PowerShell
wsl --install
```

3. Pour installer une version d'Ubuntu, vérifier la liste des distributions installable :
```PowerShell
wsl --list --online
```

Puis installer la distribution avec la commande :
```PowerShell
wsl --install -d <Distribution Name>
```

4. Lors du lancement d'Ubuntu, si la virtualisation n'est pas autorisée, éteindre l'ordi et accéder au **BIOS**. Dans les **Options Avancées** (**Advanced** ou **Advanced Options**), autoriser la virtualisation. Sauvegarder les changements et sortir du BIOS.
5. Lancer **Ubuntu** depuis le menu Démarrer, la distribution va finir de s'installer et demander un nom d'utilisateur et un mot de passe (qui n'a pas à être le même que la session Windows).

## Installation Git

1. Git devrait déjà être installé sous Ubuntu, contrôler que c'est le cas en vérifiant la version installée de Git :
```Shell
git --version
```

3. Si la commande n'est pas reconnue, installer Git :
```Shell
sudo apt-get install git
```

3. Configurer git :
```Shell
git config --global user.name "[firstname lastname]"
git config --global user.email "[valid-email]"
```

## Génération clef SSH

1. Taper la ligne de commande :
```Shell
ssh-keygen -t ed25519 -C "valid-email"
```

2. Par défaut, la commande crée la clef dans un dossier dont le chemin est /home/user/.ssh. Le chemin est modifiable.
3. Il faut définir ensuite définir une passphrase.
4. Ajouter la nouvelle clef SSH à ssh-agent, ouvrir ssh-agent :
```Shell
eval "$(ssh-agent -s)"
```
Le terminal renvoie un Agent pid suivi d'un chiffre
5. Ajouter la clef à ssh-agent :
```Shell
ssh-add ~/.ssh/nom_de_la_clef
```

6. En cas d'erreur de communication avec ssh-agent : (https://stackoverflow.com/questions/17846529/could-not-open-a-connection-to-your-authentication-agent)

## Ajout de la clef à GitHub

1. Ouvrir la clef et copier le contenu dans le Presse-papier
2. Dans GitHub, accéder aux *Settings*. Dans la section accès, cliquer sur *SSH and GPG keys* et ajouter une nouvelle clef SSH.
3. Donner un nom à la clef et copier le contenu du Presse-papier dans la section *Key*.

## Installation de l'extension WSL VS Code

1. Dans **Visual Studio Code**, installer l'extension *Remote Development* qui contient l'extension *WSL*.
2. Mettre à jour la distribution Linux :
```Shell
sudo apt-get update
```
3. Ajouter wget (récupère du contenu de serveurs web) et les certificats ca : 
```Shell
sudo apt-get install wget ca-certificates
```

4. Pour connecter WSL sur VSC, sur Ubuntu :
```Shell
code .
```

## Connexion origin (git/GitHub)

1. Dans GitHub, créer un nouveau repository :
2. Dans ce nouveau repository, GitHub indique la façon de push un dossier existant par ligne de commande en utilisant les clefs SSH : 
```Shell
git remote add origin git@github.com:nom_utilisateur/nom_repo.git
git branch -M main
git push -u origin main
```

3. Une erreur peut apparaître après la commande push :
```Shell
~$ git push -u origin main
ssh: Could not resolve hostname github: Name or service not known
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Dans ce cas, il convient de vérifier la connexion SSH à GitHub et de vérifier que la clef SSH de GitHub est la bonne.
```Shell
$ ssh -T git@github.com
> The authenticity of host 'github.com (IP ADDRESS)' can\'t be established.
> ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> Are you sure you want to continue connecting (yes/no)?
```

- Une fois l'empreinte vérifiée, le terminal devrait afficher :
```Shell
Hi user.name! You\'ve successfully authenticated, but GitHub does not provide shell access.
```
- Si le problème au moment du push persiste, l'erreur peut venir de l'initialisation de l'origin du repo. Dans ce cas :

1. Utiliser la commande : 
```
git remote -v
origin  git@github:user.name/nom_repo.git (fetch)
origin  git@github:user.name/nom_repo.git (push)
```

2. L'origin est dans notre cas mal définie, pour la réassigner on utilise la commande :
```Shell
git remote set-url origin git@github.com:user.name/nom_repo.git
```


## Installation nvm

1. A partir de la documentation, disponible sur ce [repository GitHub](https://github.com/nvm-sh/nvm), copier dans un terminal la commande :
```Shell
 curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```
2. Une fois l'installation terminée, relancer le terminal.
3. Vérifier l'installation en passant une commande nvm, comme nvm -v
4. Pour trouver les versions installables de Node en passant par nvm, passer la commande :
```Shell
nvm ls-remote
```
5. Installer une version avec la commande :
```Shell
nvm install xx.xx.x
```
