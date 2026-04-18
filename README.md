# README

```
  ___   __   __ _  ____  __    ____    ____  __  __ _  ____  ____  ____ 
 / __) / _\ (  ( \(    \(  )  (  __)  (  __)(  )(  ( \(    \(  __)(  _ \
( (__ /    \/    / ) D (/ (_/\ ) _)    ) _)  )( /    / ) D ( ) _)  )   / 
 \___)\_/\_/\_)__)(____/\____/(____)  (__)  (__)\_)__)(____/(____)(__\_) 
 ```
Auteur: Goulven Le Pennec<br/>
12/2024


### Guide d'utilisation de Candle finder
Bienvenue dans le guide d'installation de **Candle finder**!

- [README](#readme)
    - [Guide d'utilisation de Candle finder](#guide-dutilisation-de-candle-finder)
    - [1 - Descriptif](#1---descriptif)
    - [2 - Contenu du package](#2---contenu-du-package)
    - [3 - Paramétrage](#3---paramétrage)
    - [4 - Lancement](#4---lancement)
      - [4.1 Prénom](#41-prénom)
      - [4.2 - Nom](#42---nom)
      - [4.3 - Date de naissance](#43---date-de-naissance)
    - [5 - Ajout](#5---ajout)
    - [6 - Suppression](#6---suppression)
    - [7 - Paramétrer l'application avec crontab](#7---paramétrer-lapplication-avec-crontab)
      - [7.1 - Variable DISPLAY](#71---variable-display)
      - [7.2 - Variable XAUTHORITY](#72---variable-xauthority)
      - [7.3 - dbus-launch](#73---dbus-launch)
      - [7.4 - Paramétrer le lancement de l'application avec crontab](#74---paramétrer-le-lancement-de-lapplication-avec-crontab)




### 1 - Descriptif
**Candle finder** est une application de référencement et de signalement d'anniversaires, un grand plus pour ne plus passer à côté des bougies!


### 2 - Contenu du package
Le package contient ces fichiers:
- config
- launcher
- dailyCheck
- main
- ajoute
- calc
- cherche

### 3 - Paramétrage
- Se déplacer dans le répertoire.<br/>
- Ouvrir un terminal.<br/>
- Rendre exécutable le fichier `config`
```bash
chmod 777 config
```
Exécuter le script
```
./config
```
Le script va paramétrer l'environnement de base et créer la base de données

### 4 - Lancement
- Ouvrir un terminal
- Exécuter le fichier *candleFinder*:
```bash
./main
```
Une IHM se lance avec des informations:
- Présence d'un anniversaire à fêter ce jour ou non
- Une liste de choix à réaliser regroupée sous le titre *Chercher*

Si un/des anniversaire(s) survient/nent le jour en question, le nom de la/des personne(s) concernée(s) est/sont affichée(s) à l'écran ainsi que son/leur âge. Sinon un message signale l'absence d'anniversaire.

**Tous les choix de sélection dans le menu se font en entrant le numéro de l'index associé à un titre**

Par exemple pour sélectionner une recherche par prénom, il faudra entrer 1 dans le terminal puis valider avec la touche *\<Entrée>*.

Concernant la liste de choix possibles:

#### 4.1 Prénom
Permet de rechercher quelqu'un dans la base de données par son prénom. Les **recherches** ne sont pas sensibles à la casse mais sont **sensibles aux caractères spéciaux** (tels é, è, ê, ô, ...). Par exemple, une recherche de "Beatrice" se soldera par un échec si cette personne est enregistrée sous "B**é**atrice".

#### 4.2 - Nom
Permet de rechercher un individu par son nom de famille. Les critères sont les mêmes que ceux du prénom concernant la casse et les caractères spéciaux.

#### 4.3 - Date de naissance
Permet de rechercher un individu par sa date de naissance

Si au moins un individu est trouvé lors d'une recherche, il sera affiché à l'écran sous le format: <br/>
<Prénom> \<Nom>\<Date de naissance>

Si plusieurs individus correspondent au paramètre de recherche, ils seront affichés au nombre d'une personne par ligne.

### 5 - Ajout
Il est possible d'ajouter des individus à la base de données en suivant la procédure décrite.
1. Entrer le nom de famille de la personne à ajouter
2. Entrer le prénom de la personne à ajouter
3. Entrer sa date de naissance

Si l'ajout vous paraît trop fastidieux sous cet angle, il est également possible de remplir la base de données manuellement.
Veiller scrupuleusement à respecter le format décrit ci-dessous:
- Une personne par ligne
- format <Prénom>;\<Nom>;\<Date de naissance>
- Le format de la date de naissance doit être jj/mm/aaaa.
- Aucun champ ne doit être vide
- L'utilisateur reste libre de choisir la casse au sein de sa BDD.

**ex:**<br/>
```
Jean;DE LA FONTAINE;8/07/1621
```
### 6 - Suppression

Il n'existe pas à ce jour de fonction de suppression de personne de la base de données. Pour ce faire, ouvrir le fichier *dates.csv* et supprimer manuellement l'intégralité de la ligne où se trouve l'individu.
- Ne pas laisser de lignes vides au sein de la base de données
- Si la dernière ligne est effacée, s'assurer de l'existence d'un renvoi de ligne après la dernière personne référencée.


### 7 - Paramétrer l'application avec crontab
Pour une expérience-utilisateur améliorée il est possible de lancer **Candle finder** quotidiennement et ce de façon automatisée!

3 prérequis à ceci:
- Vérifier la variable DISPLAY
- Vérifier la variable XAUTHORITY
- S'assurer que dbus-launch est bien installé

#### 7.1 - Variable DISPLAY
la commande
```bash
echo $DISPLAY
```
Devrait renvoyer `:0`.
Si c'est le cas, première étape validée!

#### 7.2 - Variable XAUTHORITY
Même vérification que précédemment
```bash
echo $XAUTHORITY
```
Si un chemin s'affiche, le **copier** dans le fichier **launcher** pour remplacer la destination assignée à XAUTHORITY.

#### 7.3 - dbus-launch
Entrer la commande suivante dans le terminal:
```bash
which dbus-launch
```
Cette commande permet de vérifier si dbus-x11 est installé sur la machine.

Si cette commande ne prodigue aucun résultat, **installer** dbus-x11

```bash
sudo apt-get install dbus-x11
```

#### 7.4 - Paramétrer le lancement de l'application avec crontab
Aller dans le répertoire des crontab:
```bash 
cd /etc/cron.d
```

Créer un fichier cron par exemple **candleFinder**

```bash
sudo touch candleFinder
```
Récupérez votre nom d'utilisateur:
```
whoami
```

Copier ce contenu dans le fichier créé **candleFinder**:
```
# m h        user	command
MINUTES HEURE * * * VOTRE_NOM_D_UTILISATEUR	CHEMIN/launcher CHEMIN
```
- **MINUTES**: minutes auxquelles exécuter le script
- **HEURE**: heure à laquelle exécuter le script
- **NOM_D_UTILISATEUR**: le nom récupéré via la commande `whoami`
- **CHEMIN**: chemin absolu vers le fichier *launcher*. Copier-coller le chemin se trouvant dans le fichier **env** et y concatener `/launcher`.

Illustration:
```
# m h        user	command
05 11 * * * toto	/home/toto/launcher /home/toto/
@reboot toto /bin/sh -c 'sleep 120 && /home/toto/launcher /home/toto </dev/null'
```
*Script cron qui lance l'application **CandleFinder** quotidiennement à 11h05*.
*Script cron qui lance l'application **CandleFinder** quotidiennement à chaque allumage/redémarrage du PC après un délai de 2 minutes*

nb: il est possible de paramétrer le cron différemment bien entendu, plus d'informations se trouvent sur le net à ce sujet!

Une fois ceci paramétré, le script est prêt à être lancé quotidiennement.

Ne pas hésiter à faire quelques tests en amont car une erreur de chemin est vite arrivée ou bien des soucis de configuration peuvent obstruer le chemin le temps d'être résolus.

Petit conseil d'usage: penser à paramétrer les rappels à une heure où la machine est allumée...

À présent, impossible de manquer un anniversaire!
