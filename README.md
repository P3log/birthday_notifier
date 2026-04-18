# README

```
  ___   __   __ _  ____  __    ____    ____  __  __ _  ____  ____  ____ 
 / __) / _\ (  ( \(    \(  )  (  __)  (  __)(  )(  ( \(    \(  __)(  _ \
( (__ /    \/    / ) D (/ (_/\ ) _)    ) _)  )( /    / ) D ( ) _)  )   / 
 \___)\_/\_/\_)__)(____/\____/(____)  (__)  (__)\_)__)(____/(____)(__\_) 
 ```
Auteur: P3log<br/>
Date : 12/2024


# Guide d'utilisation de Candle finder

<!-- TOC -->

- [README](#readme)
- [Guide d'utilisation de Candle finder](#guide-dutilisation-de-candle-finder)
    - [Descriptif](#descriptif)
    - [Installation](#installation)
    - [Lancement](#lancement)
    - [Commandes](#commandes)
        - [Prénom](#pr%C3%A9nom)
        - [Nom](#nom)
        - [Date de naissance](#date-de-naissance)
        - [Ajout](#ajout)
        - [Suppression](#suppression)
    - [Paramétrer l'application avec crontab](#param%C3%A9trer-lapplication-avec-crontab)
        - [Variable DISPLAY](#variable-display)
        - [Variable XAUTHORITY](#variable-xauthority)
        - [dbus-launch](#dbus-launch)
        - [Paramétrer le lancement de l'application avec crontab](#param%C3%A9trer-le-lancement-de-lapplication-avec-crontab)

<!-- /TOC -->



## Descriptif
**Candle finder** est une application de référencement et de signalement d'anniversaires, un grand plus pour ne plus passer à côté des bougies! Cette application est implémentée en bash.


## Installation
- cloner le projet
- Se déplacer dans le répertoire du projet
- Ouvrir un terminal dans le répertoire du projet
- Rendre exécutable le fichier `config`
```bash
chmod 777 config
```
Exécuter le script
```
./config
```
Le script va paramétrer l'environnement de base et créer une base de données vide. <br>
Si vous disposez déjà d'une base de données csv au format `PRENOM;NOM;DATE_DE_NAISSANCE`.


## Lancement
- Ouvrir un terminal
- Exécuter le fichier *candleFinder*:
```bash
./candleFinder $(cat env)
```
Une IHM se lance avec des informations:
- Présence d'un anniversaire à fêter ce jour ou non
- Une liste de choix à réaliser regroupée sous le titre *Chercher*

Si un/des anniversaire(s) survient/nent le jour en question, le nom de la/des personne(s) concernée(s) est/sont affichée(s) à l'écran ainsi que son/leur âge. Sinon un message signale l'absence d'anniversaire.

**Tous les choix de sélection dans le menu se font en entrant le numéro de l'index associé à un titre**

Par exemple pour sélectionner une recherche par prénom, il faudra entrer 1 dans le terminal puis valider avec la touche *\<Entrée>*.

## Commandes

### Prénom
Recherche un prénom dans la base de données. Les **recherches** ne sont pas sensibles à la casse mais sont **sensibles aux caractères spéciaux** (tels é, è, ê, ô, ...). Par exemple, une recherche de "Beatrice" se soldera par un échec si cette personne est enregistrée sous "B**é**atrice".

### Nom
Recherche un individu par son nom de famille. Les critères sont les mêmes que ceux du prénom concernant la casse et les caractères spéciaux.

### Date de naissance
Recherche un individu par sa date de naissance

Si au moins un individu est trouvé lors d'une recherche, il sera affiché à l'écran sous le format: <br/>
<Prénom> \<Nom>\<Date de naissance>

Si plusieurs individus correspondent au paramètre de recherche, ils seront affichés au nombre d'une personne par ligne.

### Ajout
Ajoute un individu à la base de données en suivant la procédure décrite.
1. Entrer le nom de famille de la personne à ajouter
2. Entrer le prénom de la personne à ajouter
3. Entrer sa date de naissance

Il est également possible de remplir la base de données manuellement.
Veiller scrupuleusement à respecter le format décrit ci-dessous:
- Une personne par ligne
- format <Prénom;\<Nom>;\<Date de naissance>
- Le format de la date de naissance doit être jj/mm/aaaa.
- Aucun champ ne doit être vide
- L'utilisateur reste libre de choisir la casse au sein de sa BDD.

**ex:**<br/>
```
Jean;DE LA FONTAINE;8/07/1621
```

### Suppression
Il n'existe pas à ce jour de fonction de suppression de personne de la base de données. Pour ce faire, ouvrir le fichier *dates.csv* et supprimer manuellement l'intégralité de la ligne où se trouve l'individu.
- Ne pas laisser de lignes vides au sein de la base de données
- Si la dernière ligne est effacée, s'assurer de l'existence d'un renvoi de ligne après la dernière personne référencée.

## Paramétrer l'application avec crontab
Pour une expérience-utilisateur améliorée il est possible de lancer **Candle finder** de façon automatisée!

Prérequis :
- Vérifier la variable DISPLAY
- Vérifier la variable XAUTHORITY
- S'assurer que dbus-launch est bien installé

### Variable DISPLAY
la commande
```bash
echo $DISPLAY
```
Devrait renvoyer `:0`.
Si c'est le cas, première étape validée!

### Variable XAUTHORITY
Même vérification que précédemment
```bash
echo $XAUTHORITY
```
Si un chemin s'affiche, le **copier** dans le fichier **launcher** pour remplacer la destination assignée à XAUTHORITY.

### dbus-launch
Entrer la commande suivante dans le terminal:
```bash
which dbus-launch
```
Cette commande permet de vérifier si dbus-x11 est installé sur la machine.

Si cette commande ne prodigue aucun résultat, **installer** dbus-x11

```bash
sudo apt-get install dbus-x11
```

### Paramétrer le lancement de l'application avec crontab
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
## m h        user	command
MINUTES HEURE * * * VOTRE_NOM_D_UTILISATEUR	CHEMIN/launcher CHEMIN
```
- **MINUTES**: minutes auxquelles exécuter le script
- **HEURE**: heure à laquelle exécuter le script
- **NOM_D_UTILISATEUR**: le nom récupéré via la commande `whoami`
- **CHEMIN**: chemin absolu vers le fichier *launcher*. Copier-coller le chemin se trouvant dans le fichier **env** et y concatener `/launcher`.

Illustration:
```
## m h        user	command
05 11 * * * toto	/home/toto/launcher /home/toto/
@reboot toto /bin/sh -c 'sleep 120 && /home/toto/launcher /home/toto </dev/null'
```
*Script cron qui lance l'application **CandleFinder** quotidiennement à 11h05*.<br/>
*Script cron qui lance l'application **CandleFinder** quotidiennement à chaque allumage/redémarrage du PC après un délai de 2 minutes*

nb: il est possible de paramétrer le cron différemment bien entendu, plus d'informations se trouvent sur le net à ce sujet!

Une fois ceci paramétré, le script est prêt à être lancé quotidiennement.

Ne pas hésiter à faire quelques tests en amont car une erreur de chemin est vite arrivée ou bien des soucis de configuration peuvent obstruer le chemin le temps d'être résolus.

Petit conseil d'usage: penser à paramétrer les rappels à une heure où la machine est allumée...

À présent, impossible de manquer un anniversaire!
