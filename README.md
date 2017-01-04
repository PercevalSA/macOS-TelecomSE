# ROSE et SE20* sous macOS

### le guide du fanboy infiltré chez les barbus
#### Un tutoriel pour survivre dans la filière Systèmes Embarqués à Télécom ParisTech avec un ordinateur sous macOS 

#### Sommaire

1. Installer les XCode Command Line Tools
2. Installer la chaine de cross-compilation GCC ARM Embedded
	1. En téléchargeant les binaires sur le site  (plus à jour)
	2. Via Homebrew (plus simple)
3. Installer JLinkGDBServer et les drivers de la sonde SEGGER
4. Utiliser une console série
	1. Applications déjà présentes
	2. Installation d'applications plus pratiques
5. Bonus : Homebrew

## Installer les XCode Command Line Tools

Pour développer de manière générale sur macOS, vous pouvez installer l'IDE complet [XCode](https://developer.apple.com/xcode/) à partir de [l'App Store](https://itunes.apple.com/fr/app/xcode/id497799835?mt=12).
Personnellement, j'ai juste installé les Command Line Tools, qui sont les outils de développement classiques (gcc, g++, make, gdb...) en ligne de commmande.

1. Lancez le Terminal qui est dans ```/Applications/Utilities/```
2. Tapez : ```xcode-select -p```

Si vous avez un résultat tel que : ```/Applications/Xcode.app/Contents/Developer``` alors Xcode est déjà entièrement installé.
Mettez le à jour via l'App Store puis lancez le et acceptez les termes et conditions pour rendre la mise à jour effective. Redémarrez.

Sinon :

1. Installez les XCode Command Line Tools : ```xcode-select --install```
Vous avez normalement une popup qui s'affiche, cliquez sur "Installer"
2. Vérifiez la bonne installation : ```xcode-select -p```
Vous devriez avoir le résultat suivant : ```/Library/Developer/CommandLineTools```

## Installer la chaine de cross-compilation GCC ARM Embedded

### En téléchargeant les binaires sur le site officiel

1. Téléchargez l'archive sur le [site d'arm](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
2. Décompressez la (en double cliquant dessus)
3. Copiez la dans  ```opt``` : ```sudo mv gcc-arm-none-eabi-... /opt/gcc-arm-none-eabi```
4. Ajoutez la au ```PATH``` : ```export PATH=$PATH:/opt/gcc-arm-none-eabi/bin```
5. Vous pouvez aussi ajouter cette ligne dans votre ```.profile``` ou ```.bashrc``` ou ```.zshrc``` dans votre dossier personnel : ```cd $HOME && echo "export PATH=$PATH:/opt/gcc-arm-none-eabi/bin" >> .bashrc"```
6. Si vous utilisez zsh, changez ```.bashrc``` par ```.zshrc```

### Via Homebrew

1. Ajoutez un dépôt contenant la tool chain : ```brew tap PX4/homebrew-px4```
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
7. Vous pouvez aussi ajouter cette ligne dans votre ```.profile``` ou ```.bashrc``` ou ```.zshrc``` dans votre dossier personnel

## Installer une console série 

### Applications déjà présentes

* cu
* screen :  ```screen /dev/tty.usbserial 115200 cs8 -fn ```
pour se déconnecter pressez : CTRL+A suivi par ```:quit```

### Installation d'applications plus pratiques

* picocom qui est un logiciel dérivé de minicom :
	* Installer à partir du [code source](https://github.com/npat-efault/picocom)
	* Installer avec Homebrew Tapez : ```brew install picocom```
	* Installer avec MacPorts : ```ports install picocom```
Pour vous connecter tapez : ```picocom -b 115200 -f n -p n -d 8 /dev/cu.usbmodem```

Attention : ```cu.usbmodem``` est normalement suivi d'un chiffre, appuyez sur tab pour le compléter ; pour ma part j'ai ```cu.usbmodem1421```

* Serial Console (graphique) :
	* [App Store](https://itunes.apple.com/us/app/serialtools/id611021963)
	* [site officiel w7ay](http://www.w7ay.net/site/Applications/Serial%20Tools/Contents/download.html)
Connectez vous à la carte en sélectionnant "usbmodem" dans la section "Serial Port"

N'hésitez pas à appuyer sur le bouton reset de la board une fois connecté si vous n'avez pas de retour.

## Bonus : Homebrew : installer des logiciels plus simplement

[Homebrew](https://brew.sh/) est un peu l'équivalent de apt-get sous Debian pour installer des logiciels sous mac.

* Installez Homebrew : ```/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/homebrew/install/master/install)"```
* Vérifiez l'installation : ```brew doctor```
* Installez un paquet (ex: wget) : ```brew install wget```

Les paquets sont installés dans ```/usr/local/Cellar/``` et un lien symbolique est créé dans ```/usr/local/bin```.
