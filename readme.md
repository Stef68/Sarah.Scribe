PLUGIN: SCRIBE
==============
- Le plugin Scribe (pour Sarah) permet d'utiliser la reconnaissance vocale HTML5 de Google, � travers Google Chrome.
- Le plugin cr�e un serveur HTTPS **local** qui h�berge une page li�e au moteur de reconnaissance vocale HTML5 de Google et ouvre Google Chrome directement sur cette page
- Toute phrase prononc�e dans le micro est � la fois interpr�t�e par Sarah (et ses grammaires XML) et la page HTTPS.
- La page HTTPS envoie au plugin Scribe tout ce qu'elle reconnait comme mots (y compris durant la phase de reconnaissance "partielle").
- Le plugin Scribe offre des facilit�s pour utiliser ce que Google a reconnu comme phrase afin d'exploiter Google dans des plugins tiers.
- En bonus, le plugin Scribe permet d'�crire dans une zone de la page HTTPS quel est le plugin actif mais aussi ce que Sarah a dit en surlignant les mots qui sont prononc�s au fur et � mesure (le timing de cette partie est � r�gler ind�pendamment et est totalement exp�rimental) et en animant un petit visage form� de smileys ... :-)

Versions
--------
#### Version 1.0 
- release initial

#### Version 1.1
- correction de quelques bugs
- enfin compatible avec la v4
- la v4 ne supporte pas qu'on rende Sarah sourde � tour de bras. Pour simuler cela, je change le `context` � `rien` (ceci se passe uniquement en v4, en v3 Sarah est bel et bien rendue sourde sans changer le contexte).
- rajout du param�tre de config `pause_minimale_avant_synthese`
- nouveau plugin Liste Des Courses ! (voir plus bas)

Pr�requis
---------
- Sarah v3 OU v4
- Google Chrome en derni�re version
- une connexion internet

Installation
------------
1. copiez le r�pertoire `scribe` dans le r�pertoire `plugins` de Sarah
2. installez �ventuellement l'un ou l'autre plugin exemple (voir plus bas)
3. lancez Sarah (serveur+client)
4. Google Chrome s'ouvre sur la page `https://127.0.0.1:4300` (<-- HTTPS !!)
5. confirmez l'exception de s�curit�
6. au besoin confirmez l'utilisation du micro (cette �tape ne sera plus jamais demand�e car on est en HTTPS)
7. dans Chrome, appuyez sur ALT-F pour ouvrir le menu de Chrome, puis choisissez `Param�tres` (5e avant la fin)
8. Dans la section "Au d�marrage", choisissez "Ouvrir la page Nouvel Onglet" pour �viter d'avoir deux fois la page HTTPS ouverte entre deux red�marrages de Chrome.
9. Ensuite, allez tout en bas de la page des Param�tres et cliquez sur `Afficher les param�tres avanc�s`
10. cliquez sur le bouton `G�rer les certificats' dans la section HTTPS/SSL (vers la fin)
11. choisissez l'onglet `autorit�s de certification RACINES de confiance`
12. cliquez sur le bouton `importer` puis sur le bouton `suivant`, puis sur le bouton `parcourir`
13. dans la fen�tre qui s'ouvre choisissez `Certificats PKCS#7` dans la liste d�roulante en bas � droite (au dessus du bouton `ouvrir`)
14. s�lectionnez le fichier `sarah_chrome.p7b` qui est dans le r�pertoire du plugin et cliquez sur le bouton `ouvrir`
15. cliquez sur `suivant` deux fois, puis sur `terminer`

Les �tapes 9 � 15 ne sont � effectuer qu'une seule fois et permettent que Chrome n'�mette plus d'avertissement de s�curit� (�tape 5) la prochaine fois que vous lancerez Sarah

Plugins Exemples
----------------
Copiez les r�pertoires finissant par `_scribe` ou commen�ant par `scribe_` dans le r�pertoire `plugins` de Sarah pour b�n�ficier de plugins exemples utilisant le Scribe.

Pour tous les plugins exemple, n'h�sitez pas � voir comment Google interpr�te ce que vous dites. Vous verrez �galement Sarah parler. :-)

A noter que la vraie Sarah est sourde pendant ces petits exemples afin d'�viter qu'elle n'enclenche ses grammaires XML classiques.

#### Animaux
Tout le plugin utilise strictement les fonctionnalit�s du Scribe (reconnaissance vocale Google), m�me pour les questions "oui/non"
- Dites: "Sarah devine � quel animal je pense" et Sarah va vous poser une s�rie de questions pour essayer de le deviner.
  - R�pondez aux questions par "oui" ou par "non". Mais vous pouvez �galement dire "c'est exact" ou "correct" ou "non, ce n'est pas �a" etc. 
  - Il n'y a pas vraiment de limites de phrase correspondant � "oui" ou "non".
  - D�s qu'une phrase contient: "oui", "exact", "correct", "juste", "accord" alors c'est consid�r� comme "oui"
  - D�s qu'une phrase contient: "non" ou "pas" ("il n'a PAS de trompe"), c'est consid�r� comme "non"
  - Dites "je ne joue plus" � tout moment pour arr�ter le jeu
  - A un moment Sarah va vous demander s'il s'agit de tel ou tel animal
  - Si Sarah ne trouve pas votre animal, elle va vous demander une question permettant de diff�rencier le v�tre et sa r�ponse � elle. 
  - Choisissez bien votre question et surtout **prononcez-la distinctement** car c'est la reconnaissance Google qui entre en jeu.
  - Sarah r�p�te votre question et vous permet de la formuler autrement si Google n'a pas reconnu correctement ou si vous voulez en donner une autre.
  - Sarah retient alors votre question et a ainsi appris un nouvel animal
  
Vous pouvez �galement demander � Sarah de vous en dire plus sur tel ou tel animal
- Dites: "Sarah que peux-tu me dire sur (animal)"
  - Sarah va alors parcourir sa base de connaissances pour ressortir tout ce qu'elle sait sur l'animal en question
  - Cette partie du plugin est capable de traiter les animaux au singulier et au pluriel.
  En effet, Sarah stocke le nom de l'animal au singulier (par ex: "un cheval"). Si vous lui demandez "Sarah que peux-tu me dire sur les chevaux", Sarah est capable d'identifier que le singulier de "chevaux" c'est cheval. Idem pour les animaux � noms compos�s ("les �toile**s** de mer", "les tortue**s** marine**s**", etc.)
  
Enfin, vous pouvez demander � Sarah si elle sait si tel ou tel animal a telle ou telle caract�ristique. Par ex:
- Dites: "Sarah est-ce qu'un chien vit dans l'eau"
	- Sarah identifie l'animal (un chien) et recherche parmi ses questions une qui contient "vit dans l'eau"
	- elle cherche ensuite � savoir si la question se pose pour l'animal
- pour ce morceau de plugin, vous **devez** dire le nom de l'animal au singulier car sinon j'aurais du traiter la conjugaison des verbes ("est-ce que les chiens **vivent** dans l'eau ?").

#### AskMeTest
Il s'agit d'un petit plugin tout simple pour tester comment fonctionne le `AskMe` du Scribe. S'ensuit un petit dialogue.
Dites:
- Sarah demande-moi ma couleur pr�f�r�e
  - Sarah vous pose la question
  - Vous pouvez r�pondre soit par votre couleur pr�f�r�e soit par celle que vous n'aimez justement pas. Sarah essaye de trouver dans votre r�ponse (et surtout **r�agit diff�remment !!**) les mots suivants:
	- "pas le (couleur)" ou "pas (couleur)" ou "pas la couleur (couleur)" (Ex: "je ne sais pas mais s�rement pas le noir !")
	- "le (couleur)" ou "la couleur (couleur)" (ex: "j'aime le bleu")
	- "aucune" ou "pas" (ex: "je n'en ai pas" ou "je n'en aime aucune")
	- "toutes" ou "tout" (ex: "j'aime un peu de tout" ou "toutes !")
- Sarah pose-moi des questions
	- Sarah vous demande votre pr�nom et vous salue, puis Sarah demande votre �ge
	- du moment que votre r�ponse contient un nombre elle sera comprise par Sarah (ex: "22 ans" ou "je suis �g� de 22 ans" ou "j'ai 22 ans", "j'ai eu 22 ans hier", etc)
	
N'h�sitez pas � lire le code source pour vous familiariser avec les possibilit�s du Scribe.
#### Poils aux doigts
Voici encore un plugin totalement inutile. 

Dites:
- Sarah poils aux doigts
	- A partir de ce moment Sarah va �couter tout ce que vous dites et essayer de trouver une rime qui correspond. 
	Ex: 
	  - Je me demande ce que je vais faire. 
	  - Poil aux art�res !

#### Sale Gamine
M�me principe que "Poil aux doigts" mais sans les rimes ..
Dites:
- Sarah sale gamine

et Sarah r�p�te tout ce que vous dites comme une sale gamine ... :-)

#### Wikipedia
Dites:
- Sarah recherche (truc � rechercher) sur Wikipedia

et Sarah fait la recherche et vous lit le premier paragraphe wikipedia correspondant.

#### Liste des courses
Ce plugin g�re une liste de courses simple.

Pour ajouter un article � la liste, dites:
- Sarah je veux acheter (quelque chose)
- Sarah je n'ai plus de (quelque chose)
- Sarah rappelle-moi d'acheter (quelque chose)

Pour supprimer un article, dites
- Sarah supprime (quelque chose) de la liste (des courses)
- Sarah retire (quelque chose) de la liste (des courses)
- Sarah enl�ve (quelque chose) de la liste (des courses)

Sarah refusera de supprimer les articles si vous n'�tes pas assez sp�cifique. Ce sera le cas si vous dites "Sarah supprime les chips de la liste" alors que la liste contient des "chips au sel" et des "chips au paprika".
	
Pour vider toute la liste, dites
- Sarah vide (toute) la liste (des courses)
- Sarah nettoie (toute) la liste (des courses)
- Sarah supprime (toute) la liste (des courses)

Pour savoir ce qu'il y a dans la liste, dites
- Sarah qu'y a t-il dans la liste ?
	

La liste est stock�e dans un simple fichier texte avec une entr�e par ligne.
	
Fonctionnalit�s
---------------
Le plugin Scribe est compos� d'une partie NodeJS et d'une page web HTTPS. La page web HTTPS discute avec Sarah/NodeJS de mani�re bidirectionelle:
- elle renvoie � Sarah/NodeJS ce que Google a pu d�chiffrer, ainsi que l'indice de confiance de la reconnaissance Google
- elle re�oit de Sarah les phrases qu'elle est en train de prononcer et surligne les mots qu'elle prononce. En m�me temps un petit visage constitu� de smileys s'anime.

Quand vous dites une phrase, vous pouvez voir dans Chrome le moteur de Google chercher la meilleure correspondance (=reconnaissance **PARTIELLE**) jusqu'� ce que le moteur consid�re que la reconnaissance est termin�e, auquel cas il renvoie une reconnaissance **COMPLETE** (qui s'affiche sur fond noir).

Les deux types de reconnaissance sont envoy�s � Sarah, ce qui lui permet de r�agir beaucoup plus vite que s'il fallait attendre la reconnaissance compl�te. Cette fonctionnalit� est tr�s utile car Google peut parfois mettre plusieurs secondes � reconnaitre une phrase simple ("oui", "non", "Sarah allume la lumi�re"). La reconnaissance partielle permet au plugins bas�s sur Scribe de r�agir beaucoup plus vite. Ainsi, d�s que la reconnaissance partielle � identifi� "oui", "non" ou "allum*" (� rechercher avec un Regex par ex.), on pourrait utiliser ce r�sultat plut�t que d'attendre la reconnaissance compl�te. Bien entendu ce m�canisme est optionnel et vous n'�tes pas oblig� de l'utiliser syst�matiquement.

Le plugin Scribe expose de nouveaux objets JavaScript exploitables dans vos plugins, � travers l'objet `SARAH.context.scribe`.

- `compteur`: il s'agit d'un compteur de phrases reconnues enti�rement par Google. A chaque fois qu'une phrase est reconnue COMPLETEMENT, ce compteur est incr�ment� de 1. Ce m�canisme permet de s'assurer tr�s rapidement qu'une nouvelle phrase a �t� prononc�e dans le micro simplement en comparant une valeur m�moris�e du compteur avec l'actuelle valeur.
- `compteurPartial`: il s'agit d'un compteur de phrases reconnues PARTIELLEMENT par Google. A chaque fois qu'une phrase est reconnue PARTIELLEMENT, ce compteur est incr�ment� de 1. D�s que la phrase est reconnue enti�rement, `compteurPartial` reprend la m�me valeur que `compteur`. Si on utilise la reconnaissance partielle, il faut mettre soi-m�me ce compteur � la m�me valeur que `compteur` sous peine de mal identifier les reconnaissances partielles suivantes.
- `lastReco`: contient la derni�re phrase reconnue COMPLETEMENT par Google.
- `lastPartial`: contient la derni�re phrase reconnue PARTIELLEMENT par Google.
- `lastConfidence`: contient la valeur de confiance de la derni�re reconnaissance COMPLETE
- `lastPartialConfidence`: idem pour la derni�re reconnaissance PARTIELLE
- `lastX`: contient un objet `[{compteur: ..., reco: ..., confidence: ...}, {}, ...]` des X derni�res reconnaissances compl�tes. La derni�re phrase reconnue est toujours en `[0]`
- `microOFF(callback)`: fonction appelant `nircmd` pour �teindre le micro. C'est notamment utile pendant que Sarah parle afin que Google n'interpr�te pas ce que dit Sarah. Cette fonction est d�j� appel�e par `ScribeSpeak  et `ScribeAskMe`, il n'est donc pas n�cessaire de l'appeler lorsqu'on utilise ces deux fonctions-l�. Peut appeler une fonction `callback` (optionelle).
- `microON(callback)`: fonction appelant `nircmd` pour allumer le micro. Fonction `callback` en option.
- `SarahEcoute(true|false, callback)`: cette fonction permet de rendre sourde Sarah si on passe `false` en param�tre. Pour r�tablir l'�coute de Sarah, on passe `true`. Cette fonction est tr�s utile quand on d�sire ne traiter QUE la reconnaissance Google tout en emp�chant Sarah d'ex�cuter la moindre grammaire. Cette fonction est d�j� appel�e par `ScribeAskMe`. Fonction `callback` en option.
**Attention** en v4, cette fonction ne rend pas r�ellement Sarah sourde, mais change le `context` des grammaires � `rien` pour �viter qu'elle ne r�agisse aux grammaires, puis � `default` pour r�tablir son �coute. Cela signifie donc qu'on d�pend aussi du `ctxTimeOut` dans le `custom.ini`
- `hook()`: ceci permet d'appeler une fonction `callback` d�s que Chrome a reconnu partiellement ou compl�tement une phrase. La fonction `callback` prend en argument un `event` indiquant si la reconnaissance est partielle, compl�te ou si il y a eu un time-out.
Exemple de code:
```javascript
	SARAH.context.scribe.hook = function(event) {
		if (event==SARAH.context.scribe.FULLRECO) {
			// ... traiter la reconnaissance compl�te
		} else if (event==SARAH.context.scribe.PARTIALRECO) {
			// ... traiter la reconnaissance partielle
		} else if (event==SARAH.context.scribe.TIME_ELAPSED) {
			// ... traiter le time out
		}
	};
```
- `ScribeSpeak(tts, callback)`: remplace la fonction `SARAH.speak()` avec en param�tre:
  - `tts`: le texte � dire. Ce texte peut �tre une cha�ne de caract�res classiques, ou un array (ex: `['bonjour','salut',�a ca']`) ou une chaine de caract�res sp�ra�es par des `|` (ex: `"Bonjour|Salut|�a va"`). Dans les deux derniers cas, le Scribe fera dire � Sarah **au hasard** une des phrases de l'array ou une des phrases s�par�es par `|` (ex: Sarah dira "bonjour" ou "salut" ou "�a va").
  - `callback`: Peut �tre
    - une fonction `callback()` classique (comme `SARAH.speak()`) 
	- � `true` afin de forcer la synchronisation
	- � `false` ou �tes omis pour fonctionner en asynchrone (le code principal continue de s'ex�cuter sans attendre la fin de la vocalisation)

  `ScribeSpeak` n'est pas qu'un simple remplacement de `SARAH.speak()` car en plus,
    - il arr�te la reconnaissance Google le temps de la vocalisation
    - il coupe le micro avant vocalisation afin d'�viter que Google n'interpr�te ce que Sarah dit comme �tant quelque chose qu'un humain aurait dit
	- il surligne au fur et � mesure, sur la page web HTTPS, les mots que Sarah prononce et demande � la page web d'animer le visage
    - il r�tablit le micro apr�s la vocalisation
  Exemple: 
  ```javascript
  ScribeSpeak("Bonjour je m'appelle Sarah et " + 
			  "les mots de cette phrase sont surlign�s au fur et � mesure.", function() {
				// fonction callback appel�e une fois que la vocalisation est termin�e
  })
  ```
  
- `ScribeAskMe(question, reponses, callback, options)`: remplace la fonction `SARAH.askme()`. Param�tres:
  - `question`: la question a poser (chaine de caract�res)
  - `reponses`: un array de la forme `[{'regex': regexp, 'match_number': number, 'answer': object}, {...}]`. 
    - Les phrases reconnues par Google sont compar�es avec une expression **regex** en utilisant la m�thode JS **match()**. 
	- L'expression regex se place dans le param�tre `regex`, 
	- le n� de l'item � **matcher** se place dans `match_number` (la fonction `match()` peut renvoyer plusieurs r�sultats sous forme d'un array, `match_number` contient le n� de l'item � consid�rer). Par d�faut, vaut `0`.
	- s'il y a un match, le Scribe renverra l'objet associ� � `answer` ou, s'il n'a pas �t� d�fini, il renverra le n� de la place dans l'array **+ 1**
  - `callback` est la fonction `callback` de rappel sous la forme `callback(answer,phrase,match,wholeMatch)`. Avec
    - `answer`: l'objet d�finit dans `reponses` s'il y a eu un match (ou bien un num�ro correspondant � l'indice de la a r�ponse re�ue **+ 1** -- voir juste avant), ou bien `false` si il n'y a pas eu de match ou bien `undefined` s'il n'y a pas eu de r�ponse
	- `phrase`: la phrase reconnue par Google et qui a servi au traitement regex `match()`
	- `match`: le bout de phrase qui a �t� trouv� par le regex
	- `wholeMatch`: l'array match complet qui a �t� renvoy�e par le regex
  - `options`: un objet `{}` comprenant les param�tres optionnels suivants:
     - `timeout`: nombre de millisecondes avant de consid�rer qu'on n'a pas r�pondu � la question. 10000 par d�faut.
	 - `essais`: nombre d'essais autoris�s en cas de timeout ou en cas de r�ponse ne contenant pas ce qu'on cherche (si l'option `retryIfNoMatch` est � `true`). Par d�faut �gal � 1.
	 - `repeter`: s'il nous reste des essais et **qu'on a atteint le timout**, on a la possibilit� de faire r�p�ter la question ou de la formuler autrement. Vaut `true` par d�faut. Valeurs autoris�es:
	   - `true` alors, s'il nous reste des essais, la `question` est r�p�t�e quand on atteint le timeout 
	   - `false`: la question n'est pas r�p�tee
	   - Si contient une chaine de caract�res, alors cette chaine est vocalis�e plut�t que de r�p�ter la question
	 - 'retryIfNoMatch`: s'il nous reste des essais et qu'une phrase a �t� reconnue mais qu'on n'a pas trouv� de match, on a la possibilit� de faire r�p�ter la question ou de la formuler autrement. Cette option est similaire � `repeter` mais intervient uniquement en cas de non-match. Vaut `false` par d�faut. Valeurs autoris�es:
	   - `true`: la `question` originale est r�p�t�e  
	   - `false`: la question n'est pas r�p�t�e
	   - Si contient une chaine de caract�res, alors cette chaine est vocalis�e plutt�t que de r�p�ter la question
	 - `waitForFinal`: Si `true` alors seule une reconnaissance **compl�te** est autoris�e. Si `false` alors on peut utiliser une reconnaissance **partielle**. Vaut `true` par d�faut.
	 - `usePartialAfterTimeout`: Si `true` alors, en cas de timeout parce qu'il n'y a pas de phrase reconnue **compl�tement**, on autorise l'utilisation des bribes de phrase reconnues **partiellement** (c'est un peu l'option de la derni�re chance). Si `false` alors on ne donne pas la possibilit� d'utiliser ce qui a �t� reconnu **partiellement** en cas de timeout. Vaut `true` par d�faut
	 - `partialThreshold`: flottant indiquant l'indice de confiance minimum � consid�rer si `waitForFinal` est �gal � `false` ou que `usePartialAfterTimeout` est `true` et que donc on utilise la reconnaissance partielle. Vaut 0.8 par d�faut.
  
  Exemple:
  ```javascript
  q = "Est-ce que l'animal auquel tu penses poss�de des ailes ?";
  ScribeAskMe(q, [
				// le regex suivant permet de dire "sarah je ne joue pas" ou "je ne joue plus"
				 {'regex': /(joue plus|joue pas)/i, 'answer':'stop'},
				 // le regex suivant permet de dire "oui","d'accord","correct","ok","exact","juste"
				 // et sera consid�r� comme �quivalent � "oui"
				 {'regex': /(oui|correct|juste|exact|accord|ok)/, 'answer':'oui'},
				 // le regex suivant permet de r�agir � "non" ou � "il n'a PAS d'ailes"
				 {'regex': /(non|pas)/, 'answer':'non'}
				], function(answer,phrase,match,wholeMatch) {
						if (answer=='oui') {
							// traiter le fait qu'on a r�pondu oui
						} else if (answer=='non') {
							// traiter le fait qu'on a r�pondu non
						} else if (answer=="stop") {
							// traiter le fait qu'on a demand� d'arr�ter de jouer
						} else if (answer==false) {
							// si on arrive ici c'est qu'on a r�pondu quelque chose 
							// qui n'a pas �t� compris par le scribe/google
						} else {
							// si on arrive ici c'est qu'on n'a rien r�pondu
							// car en fait (typeof answer === 'undefined')
						}
				}, 
			{'timeout':15000, 	// on demande 15 sec de timeout
			// si on n'a pas r�pondu, on explique puis on r�p�te la question
			'retryIfNoMatch': "Je ne suis pas s�r d'avoir compris. Peux-tu r�p�ter ? " + q, 
			'essais': 2, 		// on a droit � deux essais
			waitForFinal: false, 		// on autorise le traitement des r�ponses partielles
										// avec un taux de confidence de 1% ! 
			partialThreshold: 0.01 		// (c'est le plus bas possible !)
			}
	);
  ```

Options de configuration du plugin
----------------------------------
Depuis l'interface serveur de Sarah
- `ports_https`: Choix du port https (par d�faut 4300)
- `autorun_browser`: Permet de lancer Chrome automatiquement au d�marrage de Sarah sur la page du serveur
- `kill_broswer_on_startup`: Si `autorun_browser` est `true`, ce param�tre permet de tuer toute autre fen�tre Chrome d�j� ouverte
- `maxReco`: nombre de reconnaissances vocales � stocker dans le param�tre `lastX` du Scribe (voir plus haut)
- `speak_surcharge`: `true` ou `false` (d�faut). Permet de surcharger la fonction `SARAH.speak()` pour qu'elle utilise syst�matiquement les fonctionnalit�s de `ScribeSpeak()`. Exp�rimental. Plus vraiment test�/v�rifi� depuis un bail. De toutes fa�ons la surcharge ne fonctionne pas avec les `out.action._attributes.tts` qui sont dans les grammaires XML donc ca reste bancal.

Les param�tres suivants permettent de r�gler le temps en millisecondes qu'il faut � Sarah pour prononcer une lettre ou un chiffre, le temps de pause qu'elle met apr�s une virgule, un point d'interrogation, etc. A r�gler selon la voix que vous utilisez. Les pr�r�glages fonctionnent de mani�re satisfaisante avec la voix de Hortense (Microsoft):
- `pause_par_lettre`: 56
- `pause_virgule`: 500
- `pause_exclamation`: 600
- `pause_interrogation": 600
- `pause_point_virgule`: 250
- `pause_deux_points`: 250
- `pause_trois_petits_points`: 0
- `pause_point`: 1250
- `pause_par_chiffre`: 300
- `pause_minimale_avant_synthese`: 800

Enfin, ce dernier param�tre permet de faire dire � Sarah toute une s�rie de phrase au d�marrage pour vous aider � r�gler les diff�rents timings pr�c�demment cit�s.
- `tests_vocaux`: `false` (d�faut) ou `true`


Cr�er un plugin qui utilise les fonctionnalit�s du Scribe
---------------------------------------------------------
Utilisez `ScribeSpeak()` et `ScribeAskMe()` pour vocaliser et attendre des r�ponses tout en rendant Sarah sourde et en coupant le micro au bon moment.

N'utilisez donc plus `SARAH.speak()` sinon Google vous entendra (ou alors utilisez `microOFF()` pour couper le micro).

N'utilisez plus non plus `callback({tts: 'texte � dire'})` car cela ne coupe pas le micro. Utilisez plut�t:
```javascript
ScribeSpeak("texte � dire", function() {
	callback();
});
```

Evitez le tts directement dans la grammaire XML (`out.action._attributes.tts`) puisqu'� nouveau c'est Sarah qui parle directement sans couper le micro.