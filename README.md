# ROSE et SE* sous macOS

#### Un tutoriel pour survivre dans la filière Systèmes Embarqués à Télécom ParisTech avec un ordinateur sous macOS 
### le guide du fanboy infiltré chez les barbus

#### Sommaire

1. Installer la chaine de cross-compilation GCC ARM Embedded
	1. En téléchargeant les binaires sur le site  (plus à jour)
	2. Via homebrew (plus simple)
2. Installer JLinkGDBServer et les drivers du la sonde SEGGER
3. Utiliser une console série
	1. Applications déjà présentes
	2. Installation d'applications plus pratiques
4. Homebrew

## Installer la chaine de cross-compilation GCC ARM Embedded

### En téléchargeant les binaires sur le site officiel

1. Téléchargez l'archive sur le [site d'arm](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
2. Décompressez la (en double cliquant dessus)
3. Copiez la dans  ```opt``` : ```sudo mv gcc-arm-none-eabi-... /opt/gcc-arm-none-eabi```
4. Ajoutez le au ```PATH``` : ```export PATH=$PATH:/opt/gcc-arm-none-eabi/bin```
5. Vous pouvez aussi ajouter cette ligne dans votre ```.profile``` ou ```.bashrc``` ou ```.zshrc``` dans votre dossier personnel : ```cd $HOME && echo "export PATH=$PATH:/opt/gcc-arm-none-eabi/bin" >> .bashrc"```
6. si vous utilisez zsh, changez ```.bashrc``` par ```.zshrc```.

### Via homebrew

1. Ajoutez un dépot contenant la tool chain : ```brew tap PX4/homebrew-px4```
2. Mettez à jour la liste des formules disponibles : ```brew update```
3. Installez gcc arm embedded : ```brew install px4/px4/gcc-arm-none-eabi```
4. Vérifiez la bonne installation : ```arm-none-eabi-gcc -v```

## Installer JLinkGDBServer et les drivers du la sonde SEGGER

1. Allez sur le site de [SEGGER](https://www.segger.com/downloads/jlink)
2. Dans la section "J-Link Software and Documentation Pack" > "Click for downloads" > [J-Link Software and Documentation pack for MacOSX](https://www.segger.com/downloads/jlink/JLink_MacOSX_V612d.pkg)
3. Lancez le pkg en double cliquant dessus
4. Suivez les instructions
5. Vérifiez la bonne installation : ```JLinkGDBServer```
6. Si le résultat est ```command not found```, ajoutez ```/Applications/SEGGER/JLink``` à votre PATH avec ```export PATH=$PATH:/Applications/SEGGER/JLink``` 
7. Vous pouvez aussi ajouter cette ligne dans votre ```.profile``` ou ```.bashrc``` ou ```.zshrc``` dans votre dossier personnel.

## Installer une console série 

### Applications déjà présentes
* cu
* screen :  ```screen /dev/tty.usbserial 115200 cs8 -fn ```
pour se déconnecter pressez : CTRL+A suivi par ```:quit```

### Installation d'applications plus pratiques

* picocom qui est un logiciel dérivé de minicom :
	* Installer à partir du [code source](https://github.com/npat-efault/picocom)
	* Installer avec homebrew Tapez : ```brew install picocom```
	* Installer avec MacPorts : ```ports install picocom```
Pour vous connecter tapez : ```picocom -b 115200 -f n -p n -d 8 /dev/cu.usbmodem```

Attention : cu.usbmodem est normalement suivi d'un chiffre, appuyez sur tab pour le compléter.
pour ma part j'ai ```cu.usbmodem1421```

* Serial Console (graphique):
	* [App Store](https://itunes.apple.com/us/app/serialtools/id611021963)
	* [site officiel w7ay](http://www.w7ay.net/site/Applications/Serial%20Tools/Contents/download.html)
Connectez vous à la carte en selectionnant "usbmodem" dans la section "Serial Port"

N'hésitez pas à appuyez sur le bouton reset de la board une fois connecté si vous n'avez pas de retour.

## BONUS : homebrew : installer des logiciels plus simplement

homebrew est un peu l'équivalent de apt-get sous Debian pour installer des logiciels sous mac.

* installer homebrew : ```/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```
* vérifier l'installation : ```brew doctor```
* installer un paquet (ex: wget) : ```brew install wget```

Les paquets sont installés dans ```/usr/local/Cellar/``` et un lien symbolique est créé dans ```/usr/local/bin```
