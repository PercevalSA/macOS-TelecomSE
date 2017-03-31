# ROSE et SE20* sous macOS

### le guide du fanboy infiltré chez les barbus
#### Un tutoriel pour survivre dans la filière Systèmes Embarqués à Télécom ParisTech avec un ordinateur sous macOS 

#### Sommaire

1. Installer les XCode Command Line Tools
2. Installer la chaine de cross-compilation GCC ARM Embedded
	1. En téléchargeant les binaires sur le site
	2. Via Homebrew
3. Installer JLinkGDBServer et les drivers de la sonde SEGGER
4. Utiliser une console série
	1. Application déjà présente
	2. Installation d'applications plus pratiques
5. Bonus : Homebrew

## Installer les XCode Command Line Tools

Pour développer de manière générale sur macOS, vous pouvez installer l'IDE complet [XCode](https://developer.apple.com/xcode/) à partir de [l'App Store](https://itunes.apple.com/fr/app/xcode/id497799835?mt=12).
Personnellement, j'ai juste installé les Command Line Tools, qui sont les outils de développement classiques (gcc, g++, make, gdb...) en ligne de commmande. Personnellement j'utilise [Sublime Text](https://www.sublimetext.com/). Vous pouvez également utiliser [Atom](https://atom.io/), vim ou emacs.

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
3. Copiez la dans  ```opt``` : ```sudo mv gcc-arm-none-eabi-[version] /usr/local/opt/gcc-arm-none-eabi```. Utilisez la touche tabulation pour compléter [version].
4. Ajoutez la au ```PATH``` : ```export PATH=$PATH:/usr/local/opt/gcc-arm-none-eabi/bin```
5. Vous devez aussi ajouter cette ligne dans votre ```.bashrc``` ou ```.zshrc``` dans votre dossier personnel : ```cd $HOME && echo "export PATH=$PATH:/usr/local/opt/gcc-arm-none-eabi/bin" >> .bashrc"```
6. Si vous utilisez zsh, changez ```.bashrc``` par ```.zshrc```
7. Vérifiez la bonne installation : ```arm-none-eabi-gcc --version``` et ```arm-none-eabi-gdb --version```

La destination ```/usr/local/opt``` n'existe qu'à partir de macOS Sierra. Si ce dossier n'existe pas, vous devrez utiliser ```/opt``` comme destination. Si aucun des deux n'existe vous devez en créer un des deux à la main : ```sudo mkdir /opt```. Veillez à bien utiliser le même chemin pour les autres commandes.

### Via Homebrew

1. Ajoutez un dépôt contenant la tool chain : ```brew tap PX4/homebrew-px4```
2. Mettez à jour la liste des formules disponibles : ```brew update```
3. Recherchez la version la plus à jour : ```brew search arm-none-eabi```
4. Installez gcc arm embedded : ```brew install px4/px4/gcc-arm-none-eabi-[version]```
5. Vérifiez la bonne installation : ```arm-none-eabi-gcc --version``` et ```arm-none-eabi-gdb --version```

Attention : ```gcc-arm-none-eabi``` n'installe pas la dernière version. Pour plus de détails : ```brew info px4/px4/gcc-arm-none-eabi```

## Installer JLinkGDBServer et les drivers du la sonde SEGGER

1. Allez sur le site de [SEGGER](https://www.segger.com/downloads/jlink)
2. Dans la section "J-Link Software and Documentation Pack" > "Click for downloads" > [J-Link Software and Documentation pack for MacOSX](https://www.segger.com/downloads/jlink/JLink_MacOSX_V612d.pkg)
3. Lancez le pkg en double cliquant dessus
4. Suivez les instructions
5. Vérifiez la bonne installation : ```JLinkGDBServer```
6. Si le résultat est ```command not found```, ajoutez ```/Applications/SEGGER/JLink``` à votre PATH avec ```export PATH=$PATH:/Applications/SEGGER/JLink``` 
7. Vous devez aussi ajouter cette ligne dans votre ```.bashrc``` ou ```.zshrc``` dans votre dossier personnel : : ```cd $HOME && echo "export PATH=$PATH:/Applications/SEGGER/JLink" >> .bashrc"```
8. Si vous utilisez zsh, changez ```.bashrc``` par ```.zshrc```
9. Re-vérifiez la bonne installation : ```JLinkGDBServer```

## Installer une console série 

### Application déjà présente

* cu
* screen :  ```screen /dev/tty.usbserial 115200 cs8 -fn ```
pour se déconnecter pressez : CTRL+A suivi par ```:quit```

### Installation d'applications plus pratiques

* minicom :
	* Installer à partir du [code source](https://alioth.debian.org/frs/?group_id=30018)
	* Installer avec Homebrew : ```brew install minicom```
	* Installer avec MacPorts : ```sudo port install minicom```

* picocom qui est un logiciel dérivé de minicom :
	* Installer à partir du [code source](https://github.com/npat-efault/picocom)
	* Installer avec Homebrew : ```brew install picocom```
	* Installer avec MacPorts : ```sudo port install picocom```

Pour vous connecter tapez : ```picocom -b 115200 -f n -y n -d 8 -p 1 /dev/cu.usbmodem<numero>```
Attention : ```cu.usbmodem``` est suivi d'un chiffre, appuyez sur tab pour le compléter; pour ma part j'ai ```cu.usbmodem311```
Pour se déconnecter pressez : CTRL+A suivi de CTRL+Q

* Serial Console (graphique) :
	* [App Store](https://itunes.apple.com/us/app/serialtools/id611021963)
	* [site officiel w7ay](http://www.w7ay.net/site/Applications/Serial%20Tools/Contents/download.html)
Connectez vous à la carte en sélectionnant "usbmodem" dans la section "Serial Port"

N'hésitez pas à appuyer sur le bouton reset de la board une fois connecté si vous n'avez pas de retour.

## Bonus : Homebrew : installer des logiciels plus simplement

[Homebrew](https://brew.sh/) est un peu l'équivalent de apt-get sous Debian pour installer des logiciels sous macOS.
Il existe d'autres gestionnaires de paquets tels que MacPorts ou Fink. Je vous laisse faire vos recherches...

* Installez Homebrew : ```/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/homebrew/install/master/install)"```
* Vérifiez l'installation : ```brew doctor```
* Installez un paquet (ex: wget) : ```brew install wget```

Les paquets sont installés dans ```/usr/local/Cellar/``` et un lien symbolique est automatiquement créé dans ```/usr/local/bin```.
