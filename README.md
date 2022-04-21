# WriteUp CTF HackDay : J'ai perdu le flag

On commence ce challenge avec un dossier *johnnix* et un zip *johnHacked.zip* comme montré ci-dessous.

![image](https://user-images.githubusercontent.com/73934639/164445255-afb931e6-c79f-45c3-bd89-703bdf6be187.png)

Le zip est protégé par un mot de passe. On se rend donc dans le dossier *johnnix* qui contient un grand nombre de fichiers texte. Les fichiers texte n'ont pas l'aire d'être important en soit excepté certaines chaines encodées en base64.

![image](https://user-images.githubusercontent.com/73934639/164445865-88a78daf-2471-48b3-9495-d116bbe87aaa.png)

Dans ce dossier il y a une image du logo de l'épreuve. On se rend donc sur [aperisolve](https://aperisolve.fr). On nous dit que l'image contient un fichier *Readme*. On l'extrait donc avec **steghide** sans utiliser de mots de passe.

![image](https://user-images.githubusercontent.com/73934639/164446356-3e11759e-cf00-4730-97a4-40d70079f1f3.png)

Le texte est disponible [ici](https://github.com/Nouman404/WrtiteUp-J-ai-perdu-le-flag/blob/main/readme).\
J'ai l'impression que le challenge se découpe en deux parties, OSINT et récupération des mots de chaque fichier. Je commence une recherche rapide de ce *john nix* sans succès. Je crée donc un scripte pour récupérer le **premier mot de la ligne 24 des fichiers suivants :\
682910xecoz\
537w3zly33p\
u3ow02q3r77\
2i64pvpe639\
99u6ov4n2p2\
b0448gpzn49\
n68ktas0402\
fkz90adazd1**

Le résultat est le suivant :

![image](https://user-images.githubusercontent.com/73934639/164447762-1012d31f-0edd-4308-979a-985cacf4ea51.png)

J'ai comme l'impression qu'on va devoir retourner faire de l'OSINT .\
À la fin du fichier, il y a le nom et prénom de la personne qui a écrit le fichier, mais aussi son mail **mailto@john.nix@gmx.fr**.
J'envoie donc un mail à ce cher John et .... obtiens une réponse automatique :

![image](https://user-images.githubusercontent.com/73934639/164448862-9d441085-0382-4111-8407-cbcefe7f6e72.png)

Les liens Instagram et LinkedIn ne fonctionnent pas... Mais le Twitter oui !

![image](https://user-images.githubusercontent.com/73934639/164449403-077a4e41-16c5-4daf-8f5d-b03074372125.png)

On se rend sur le lien mega et on télécharge un fichier ".wav".
Mon premier réflexe (après avoir écouté ce doux son mélodieux) est d'utiliser [sonic visualizer](https://www.sonicvisualiser.org/download.html) qui est très utile pour les challenges de CTF audio. On ajoute un spectrogramme et ... taddaaaaa :

![image](https://user-images.githubusercontent.com/73934639/164449972-4d97366b-6e06-4515-8da3-ded32570ad41.png)

Le mot de passe du zip... ou pas. En fait, c'est le nom d'un fichier. On se rend à la 24e ligne de ce dernier et le premier mot est :
```NzQgOTcgNjggMTExIDgyIDEwMSA3NiA5NyA4MyAxMTYgMTAxIDEwMyA5NyAxMTAgMTExIDM1IDEwOSAxMDggMzYgNDkgMzYgMTAyIDEwMSA1NiA5OSAxMDEgNTUgNTQgOTcgNTMgNDk=```

Pour cette partie l'outil [CyberChef](https://gchq.github.io/CyberChef/) peut aider.
Ce texte est évidemment de la base 64 ce qui nous donne une fois décodé :\
```74 97 68 111 82 101 76 97 83 116 101 103 97 110 111 35 109 108 36 49 36 102 101 56 99 101 55 54 97 53 49```  

Ce nouveau texte est en base 10 ce qui nous donne une fois décodé :```JaDoReLaStegano#ml$1$fe8ce76a51```

Bingo le mot de passe du zip !

On le décompresse et on obtient ... 19 images.
Je mets la 1ere dans aprerisolve et il y a un fichier que l'on peut extraire avec steghide sans mot de passe, idem pour la 2e et la 3e...
Je crée un script pour automatiser tout ça et ... retour à la case départ. Des textes une image de Rick et un readme qui nous dit :
```
Hey ! Tu y es presque ...
Encore un peu de recherche :)

🎶 NEVER GONNA GIVE YOU UP 🎶 
🎶 NEVER GONNA LET YOU DOWN 🎶
```

Je mets l'image *ImRick.jpg* dans apérisolve et ... elle contient un autre fichier (fkz90adazd1_) contenant la chaine : ```fnB0Jnh7Kkx0K3doekllY2JwREhfPUNO```

Encore de la base64 qui nous donne : ```~pt&x{*Lt+whzIecbpDH_=CN```

L'outil (dcode)[https://www.dcode.fr/identification-chiffrement] peut être utile pour cette partie.
J'essaye de trouver l'encodage de cette chaine et le ROT47 nous donne un résultat plutôt satisfaisant : ```OAEUILY{EZH9Kx643Asw0lr}```

Bon ce n'est pas le flag mais ça y ressemble.

De là on a deux options :
1. On a déjà décodéer les chaines en base64 des fichiers précédent contenant les faux flags
2. On l'a pas fait

Si on l'a fait on voit que les faux flags sont de la forme **HackFlag[xxx]**. On test de décoder avec vigenère et la clé *HACKFLAG* et ... FIN. On a le flag.

Sinon on peut le deviner. On test par exemple la clé HACKDAY et on obtient : ```HACKFLA{XZF9Au643Aup0lp}```

De là, soit on bruteforce la clé de la forme *HACK** soit on utilise *HACKFLA* et ... FIN... ah non le flag **HACKDAY{XZF9As643Psp0lp}** ne marche pas. Après un peu de réflexion on essaye *HACKFLAG* et là on a le bon flag.





