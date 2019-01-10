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

---------------------------------------------Passerelle --> BeagleBone--------------------------------------------------- 

1 - Connexion au BeagleBone :

Tout d'abord, afin que votre ordinateur puisse reconaitre le BeagleBone, il faut installer les drivers adéquates. 

Nous vous conseillons de suivre le tutoriel d'installation sur le site http://beagleboard.org/getting-started. 
Une fois les drivers installés, nous devions flashé notre carte SD contenant l'OS de l'appareil. Pour cela, nous avons utilisé BalenaEtcher qui est disponible sur tous les OS. 

Maintenant que votre Beaglebone est bien configuré, nous vous conseillons de télécharger la version Debian 8.6 (beaglebone-debian-8.6-iot-armhf-2016-12-09-4gb.img).

Vous pouvez miantenant connecté votre Beaglebone en USB sur votre ordinateur. A ce moment la, allez dans vos périphériques réseaux et normalement, une deuxième carte réseau s'est montée. 

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




, il faut :

Désactiver le HDMI qui utilise les mêmes pins que SPI.
Activer le SPI
Activer les ports série UART
Pour cela, éditer le fichier /boot/uEnv.txt pour n'y garder que les lignes suivantes:

cat /boot/uEnv.txt
...
cmdline=coherent_pool=1M quiet net.ifnames=0 cape_universal=enable
#DISABLE HDMI AND ENABLE SPI
cape_disable=bone_capemgr.disable_partno=BB-BONELT-HDMI,BB-BONELT-HDMIN
cape_enable=bone_capemgr.enable_partno=BB-SPIDEV0,BB-SPIDEV1
#ENABLE UART
cape_enable=bone_capemgr.enable_partno=BB-UART1,BB-UART2,BB-UART4,BB-UART5
...
et redémarrez votre BBB car comme ce fichier est un fichier système, le BBB ne le prendra en compte que lors du rédmarrage. 

Vérifications :

Vérifier que les slots sont identifiés:
root@beaglebone:/sys/devices/platform/bone_capemgr# less slots
 0: PF----  -1 
 1: PF----  -1 
 2: PF----  -1 
 3: PF----  -1 
 4: P-O-L-   0 Override Board Name,00A0,Override Manuf,BB-SPIDEV0
 5: P-O-L-   1 Override Board Name,00A0,Override Manuf,BB-SPIDEV1
 6: P-O-L-   2 Override Board Name,00A0,Override Manuf,BB-UART1
 8: P-O-L-   3 Override Board Name,00A0,Override Manuf,BB-UART4
 9: P-O-L-   4 Override Board Name,00A0,Override Manuf,BB-UART5
2.Vérifiez que les bus SPI sont identifiés:

root@beaglebone:/dev# ls spidev*
spidev1.0  spidev1.1  spidev2.0  spidev2.1
root@beaglebone:/dev# ls i2c-*
i2c-0  i2c-2
root@beaglebone:/dev# ls ttyO*
/dev/ttyO0  /dev/ttyO1  /dev/ttyO4  /dev/ttyO5



Partie Cloud : 





