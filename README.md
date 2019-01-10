# 5irc-projet-iot-2018-19-team-stbj

# Projet IoT

Ce projet a pour but de créer le système d'une montre connectée, qui servira à faire des relevés de taux de CO2 via un système de géolocalisation GPS. 
Ces données seront alors envoyées et mise en forme pour analyse et des actions pourront être réalisées pour faire baisser le taux de pollution. 

## Description du Projet

Voici l’organisation de notre projet cette semaine, pour rappel, les trois principales composantes d'un système IoT sont :
Capteurs --> Passerelles --> Cloud
Dans le rôle de capteurs, nous aurons un capteur de CO2 et une balise GPS connectés sur un Arduino. Ce dernier transmettra ces données via ZigBee sur un autre Arduino qui fera la passerelle vers notre BeagleBone. Le BBB se chargera de faire la passerelle vers Ubidots qui est la plateforme Cloud que nous avons choisit d'utiliser.

### Matériel utilisé 

Le capteur C02 utilisé pour ce projet est le MQ-135 sensor. 
La balise GPS est une GPS3 click. 
Les Arduino utilisés sont des Arduino Mega click shield. L'avantage de ces derniers est qu'ils possèdent plusieurs sérial qui permettent la connexion de trois capteurs par Arduino. 

## Procédure de mise en place de votre chaîne IoT

-----------------------------------------------Capteur --> Arduino-------------------------------------------------------

---------------------------------------------Passerelle --> BeagleBone--------------------------------------------------- 

1 - Connexion au BeagleBone 

Tout d'abord, afin que votre ordinateur puisse reconnaitre le BeagleBone, il faut installer les drivers adéquates. 

Nous vous conseillons de suivre le tutoriel d'installation sur le site http://beagleboard.org/getting-started. 
Une fois les drivers installés, nous devions flashé notre carte SD contenant l'OS de l'appareil. Pour cela, nous avons utilisé Etcher qui est disponible sur tous les OS. 

Maintenant que votre Beaglebone est bien connecté, nous vous conseillons de télécharger la version Debian 8.6 (beaglebone-debian-8.6-iot-armhf-2016-12-09-4gb.img).

Vous pouvez maintenant connecté votre Beaglebone en USB sur votre ordinateur. A ce moment la, allez dans vos périphériques réseaux et une deuxième carte réseau s'est montée. 

Activez le partage de votre connexion internet sur cette carte, et configurez la pour qu'elle soit dans le meme réseau que votre Beaglebone. 

Petit rappel du tutoriel : 

Sur MACOS X, Linux, le BeagleBone prend l'adresse IP : : 192.168.6.2. 
Sur Windows, le BBB prend l'adresse IP : 192.168.7.2.

En SSH, connectez vous sur le BBB. 
Login : Debian 
Password : temppwd 

Vous voilà à la racine du BBB. 

2 - Configuration du BBB 

A) Connexion Internet 
Pour l'instant, votre BeagleBone n'est pas connecté à Internet (test ping 8.8.8.8). Pour configurer la connexion Internet, je vous conseil de suivre ce tutoriel : https://www.digikey.com/en/maker/blogs/how-to-connect-a-beaglebone-black-to-the-internet-using-usb 

Deux choses sont importantes : 
- Il faut ajouter une route par défaut pour dire au BBB par où sortir vers Internet : 

sudo /sbin/route add default gw 192.168.7.1

- Dans ce cas là, la connexion est établie mais le DNS ne marche pas. Avec cette commande, vous mettez à jour le DNS dans resolv.conf. 

echo "nameserver 8.8.8.8" >> /etc/resolv.conf 

NB : pensez bien à configurer le DNS sur votre carte Ethernet dans votre PC (ajoutez 8.8.8.8). Pour tester, ping google.com.

B) Configuration des pins 

Pour commencer, nous avons besoin  d'activer les ports UART. 
Pour cela, éditer le fichier /boot/uEnv.txt et ajoutez la ligne suivante :

cape_enable=bone_capemgr.enable_partno=BB-UART1,BB-UART2,BB-UART4,BB-UART5 

et supprimez : 
cape_universal=enable 

et redémarrez votre BBB : ce fichier est un fichier système, le BBB ne le prendra en compte que lors du rédémarrage. 

Vérifications :

Vérifier que les slots sont identifiés:
root@beaglebone:/sys/devices/platform/bone_capemgr# cat slots
 0: PF----  -1 
 1: PF----  -1 
 2: PF----  -1 
 3: PF----  -1 
 4: P-O-L-   1 Override Board Name,00A0,Override Manuf,BB-UART1
 5: P-O-L-   0 Override Board Name,00A0,Override Manuf,BB-UART2
 6: P-O-L-   2 Override Board Name,00A0,Override Manuf,BB-UART4
 7: P-O-L-   3 Override Board Name,00A0,Override Manuf,BB-UART5
 
Vérifiez que les ttyO sont identifiés:

root@beaglebone:/dev# ls ttyO*
/dev/ttyO0  /dev/ttyO1 /dev/ttyO2 /dev/ttyO4  /dev/ttyO5

Les ports sont maintenant prêts à être utilisés, dans notre cas, nous utiliserons que les UART1 et UART2. 

C) Interconnexion physique 

Nous allons maintenant connecter physiquement les ports UART sur le BeagleBone pour vérifier qu'ils sont bien utilisables. 
Nous allons connecter :  UART1 TXD (P9_24) à UART2 RXD (P9_22) et UART2 TXD (P9-21) à UART1 RXD (P9_26). 

D) Vérification avec Minicom

Ensuite, installez minicom : 

sudo apt-get update 
sudo apt-get install minicom

En root : 
minicom -D /dev/ttyO1 -b 9600 
minicom -D /dev/ttyO2 -b 9600

Deux terminal s'ouvrent alors. Si les ports sont montés, vous pourrez écrire dans le premier et cette écriture se répercutera dans le second. 

Partie Cloud : 





