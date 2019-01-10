# 5irc-projet-iot-2018-19-team-stbj

# Projet IoT

Ce projet a pour but de créer le système d'une montre connectée, qui servira à faire des relevés de taux de CO2 via un système de géolocalisation GPS. 
Ces données seront alors envoyées et mise en forme pour analyse et des actions pourront être réalisées pour faire baisser le taux de pollution. 

## Description du Projet

Voici l’organisation de notre projet cette semaine, pour rappel, les trois principales composantes d'un système IoT sont :
Capteurs --> Passerelles --> Cloud
Dans le rôle de capteurs, nous aurons un capteur de CO2 et une balise GPS connectés sur un Arduino. Ce dernier transmettra ces données via ZigBee sur un autre Arduino qui fera la passerelle vers notre BigleBone. Le BBB se chargera de faire la passerelle vers Ubidots qui est la plateforme Cloud que nous avons choisit d'utiliser.

### Matériel utilisé 

Le capteur C02 utilisé pour ce projet est le MQ-135 sensor. 
La balise GPS est une GPS3 click. 
Les Arduino utilisés sont des Arduino Mega click shield. L'avantage de ces derniers est qu'ils possèdent plusieurs sérial qui permettent la connexion de trois capteurs par Arduino. 

## Procédure de mise en place de votre chaîne IoT

Partie capteur : 

Partie BeagleBone :



Partie Cloud : 





