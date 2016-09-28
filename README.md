# macOS-TelecomSE ou ROSE sous macOS

#### Un tutoriel pour survivre dans la filière Systèmes Embarqués à Télécom ParisTech avec un mac aka le guide du fanboy infiltré chez les barbus

## installer des logiciels plus simplement

on peut utiliser homebrew qui est l'équivalent de apt-get sous Debian

* installer homebrew : ```/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```
* vérifier l'installation : ```brew doctor```
* installer un paquet (ex: wget) : ```brew install wget```

les paquets sont installés dans ```/usr/local/Cellar/``` et un lien symbolique est créé dans ```/usr/local/bin```

## Installer la chaine de cross-compilation GCC ARM Embedded
1. Ajoutez un dépot contenant la tool chain : ```brew tap PX4/homebrew-px4```
2. Mettez à jour la liste des formules disponibles : ```brew update```
3. Installez gcc arm embedded : ```brew install px4/px4/gcc-arm-none-eabi-54```
4. Vérifiez la bonne installation : ```arm-none-eabi-gcc -v```

## Installer JLinkGDBServer et les drivers du la sonde SEGGER
Le device utilisé est un STM32F407ZG

1. Allez sur le site de [SEGGER](https://www.segger.com/downloads/jlink)
2. Dans la section "J-Link Software and Documentation Pack" > "Click for downloads" > [J-Link Software and Documentation pack for MacOSX](https://www.segger.com/downloads/jlink/JLink_MacOSX_V610a.pkg)
3. Lancez le pkg en double cliquant dessus
4. Suivez les instructions
5. Ajoutez "/Applications/SEGGER/JLink" à votre PATH avec ```export PATH=$PATH:/Applications/SEGGER/JLink/.``` 
	* Vous pouvez aussi ajouter cette ligne dans votre .profile ou .bashrc ou .zshrc dans votre dossier personnel.
6. Pour vérifier la bonne installation, tapez : JLinkGDBServer

## Installer Serial Console

Pour la connection série nous pouvons utiliser cu. Il existe cependant pleins d'applications pour cela.
Installez Serial Console :
* soit via l'[App Store](https://itunes.apple.com/us/app/serialtools/id611021963)
* soit via le [site w7ay](http://www.w7ay.net/site/Applications/Serial%20Tools/Contents/download.html)

1. Connectez vous à la carte en selectionnant "usbmodem" dans la section "Serial Port"
2. Si vous appuyez sur le bouton Reset sur la carte vous devriez avoir un retour
3. Vous pouvez aussi utiliser picocom qui est un logiciel dérivé de minicom, pour cela :
	1. Tapez : brew install picocom
	2. Pour vous connecter tapez : picocom -b 115200 -f n -p n -d 8 /dev/cu.usbmodem

Attention : cu.usbmodem est normalement suivi d'un chiffre, appuyez sur tab pour le compléter.
pour ma part j'ai "cu.usbmodem1421"
