Téléinfo Broadcast
==================

Ce programme qui permet de récupérer les trames téléinfo reçue par la liaison série d'un ordinateur (ou autre périphérique) 
puis de les envoyer (broadcaster) sur un reseau local. de ce fait, il est possible de récupérer ces informations sur une 
(ou même plusieurs machines) pour en faire le traitement souhaité.

Il est aussi capable d'envoyer les données de téléinfo vers une base mySQL ou un serveur emoncms
 
Ce programme est entièrement développé pour mes besoins personnels, il serait donc illusoire de penser pouvoir l'utiliser en l'état à d'autres fin sans découvrir de bugs ou des manques de fonctionnalités. Néamoins, je pense qu'il peut servir de base et je peux aider si vous avez des questions.
 
Pour rendre à César ce qui appartient à César, je tenais à préciser que je me suis largement inspiré de l'excellent site de Domos et de son programme original ainsi que des sources de picocom.
 
J'oubliais, le programme fonctionne uniquement sous linux (testé sur Debian Lenny et wheezy) mais çà ne doit pas être sorcier de l'adapter pour Windows.
 
Le fonctionnement est très simple, et peut être distingué en deux modes :
  - Le mode serveur (ou daemon), c'est à dire qu'il ecoute les trames reçues sur le port série et les transfère immediatement via une trame UDP sur le réseau local. Petite subtilité, en fait deux trames identiques sont envoyés, une sur le port 1200 et l'autre sur le port 1201, tout simplement parce que mon PC de domotique utilise deux programmes pour traiter les données de téléinfo or deux programmes distincts ne peuvent pas se écouter (binder) simultanément sur le même port.
  - mon daemon domotique avec l'affichage en temps réél des données téléinfo reçues
  - un script executé toutes les 5 minutes pour envoyer les données de téléinfo sur la base MySQL

 
  - Le mode client, c'est à dire qu'il écoute sur le réseau local une trame UDP (donc envoyée par de daemon) puis si celle-ci est valide, fait un traitement associé. Dans mon cas, soit on affiche la trame soit on stocke les données dans une base MySQL avec les valeurs reçues (le nom des champs de la table doit être le même que les champs de la trame reçue, donc un champs ADCO, …).

Installation
------------

Récupérez les fichiers teleinfo.c et Makefile (et potentiellement le fichier teleinfo.conf si vous préférez ne pas utiliser tous les switch de la ligne de commande) et les positionner dans le même dossier puis allez dans ce dossier et tapez la commande

make
make install

Copier le fichier de config vers /etc/ et modifiez le selon vos besoins.

Vous pouvez complètement supprimer la partie mysql si vous le souhaitez, il suffit de commenter les deux lignes dans le Makefile
CFLAGSSQL=-DUSE_MYSQL
LIBSSQL=-lmysqlclient

Idem pour la partie Emoncms qui est basée sur la librairie curl
CFLAGSEMONCMS=-DUSE_EMONCMS 
LIBSEMONCMS=-lcurl 

