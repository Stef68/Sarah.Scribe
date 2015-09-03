PLUGIN: SCRIBE
==============
- Le plugin Scribe (pour Sarah) permet d'utiliser la reconnaissance vocale HTML5 de Google, � travers Google Chrome.
- Le plugin cr�e un serveur HTTPS **local** qui h�berge une page li�e au moteur de reconnaissance vocale HTML5 de Google et ouvre Google Chrome directement sur cette page
- Toute phrase prononc�e dans le micro est � la fois interpr�t�e par Sarah (et ses grammaires XML) et la page HTTPS.
- La page HTTPS envoie au plugin Scribe tout ce qu'elle reconnait comme mots (y compris durant la phase de reconnaissance "partielle").
- Le plugin Scribe offre des facilit�s pour utiliser ce que Google a reconnu comme phrase afin d'exploiter Google dans des plugins tiers.
- En bonus, le plugin Scribe �crit dans une zone de la page HTTPS quel est le plugin actif mais aussi ce que Sarah a dit en surlignant les mots qui sont prononc�s au fur et � mesure (le timing de cette partie est � r�gler ind�pendamment et est totalement exp�rimental) et en animant un petit visage form� de smileys ... :-)

Pr�requis
---------
- Sarah v3 (j'ai mis du code compatible v4 mais je n'ai pas test�)
- Google Chrome en derni�re version

Installation
------------
1. copiez le r�pertoire `scribe` dans le r�pertoire `plugins` de Sarah
2. installez �ventuellement l'un ou l'autre plugin exemple (voir plus bas)
3. lancez Sarah (serveur+client)
4. Google Chrome s'ouvre sur la page `https://127.0.0.1:4300` (<-- HTTPS !!)
5. confirmez l'exception de s�curit�
6. au besoin confirmez l'utilisation du micro (cette �tape ne sera plus jamais demand�e car on est en HTTPS)
7. dans Chrome, appuyez sur ALT-F pour ouvrir le menu de Chrome, puis choisissez `Param�tres` (5e avant la fin)
8. allez tout en bas de la page des Param�tres et cliquez sur `Afficher les param�tres avanc�s`
9. cliquez sur le bouton `G�rer les certificats' dans la section HTTPS/SSL (vers la fin)
10. choisissez l'onglet `autorit�s de certification RACINES de confiance`
11. cliquez sur le bouton `importer` puis sur le bouton `suivant`, puis sur le bouton `parcourir`
12. dans la fen�tre qui s'ouvre choisissez `Certificats PKCS#7` dans la liste d�roulante en bas � droite (au dessus du bouton `ouvrir`)
13. s�lectionnez le fichier `sarah_chrome.p7b` qui est dans le r�pertoire du plugin et cliquez sur le bouton `ouvrir`
14. cliquez sur `suivant` deux fois, puis sur `terminer`

Les �tapes 7 � 14 ne sont � effectuer qu'une seule fois et permettent que Chrome n'�mette plus d'avertissement de s�curit� (�tape 5) la prochaine fois que vous lancerez Sarah

Fonctionnalit�s
---------------
Le plugin Scribe est compos� d'une partie NodeJS et d'une page web HTTPS. La page web HTTPS discute avec Sarah/NodeJS de mani�re bidirectionelle:
- elle renvoie � Sarah/NodeJS ce que Google a pu d�chiffrer, ainsi que l'indice de confiance de la reconnaissance Google
- elle re�oit de Sarah les phrases qu'elle est en train de prononcer et surligne les mots qu'elle prononce. En m�me temps un petit visage constitu� de smileys s'anime

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
- `microOFF()`: fonction appelant `nircmd` pour �teindre le micro. C'est notamment utile pendant que Sarah parle afin que Google n'interpr�te pas ce que dit Sarah. Cette fonction est d�j� appel�e par `ScribeSpeak  et `ScribeAskMe`, il n'est donc pas n�cessaire de l'appeler lorsqu'on utilise ces deux fonctions-l�.
- `microON()`: fonction appelant `nircmd` pour allumer le micro.
- `SarahEcoute(true|false)`: cette fonction permet de rendre sourde Sarah si on passe `false` en param�tre. Pour r�tablir l'�coute de Sarah, on passe `true`. Cette fonction est tr�s utile quand on d�sire ne traiter QUE la reconnaissance Google tout en emp�chant Sarah d'ex�cuter la moindre grammaire. Cette fonction est d�j� appel�e par `ScribeAskMe`.
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
  

Avantages
---------
- PLUS DE LIMITATION A 50 UTILISATIONS !!
- Plus besoin de cr�er une cl� Google API (dont le principe d'inscription change tout le temps)
- vous pouvez utiliser le m�me principe (sans n�cessairement Garbage) pour r�cup�rer ce que Chrome a compris A TOUT INSTANT ! Par ex:
  - votre grammaire contient "Sarah allume la lumi�re du salon" et "Sarah allume la lumi�re de la cuisine"
  - anciennement il fallait passer en argument (action/data) le mot "salon" ou "cuisine" si on voulait que Sarah r�ponde "j'ai allum� la lumi�re de la cuisine/salon"
  - vous pouvez � pr�sent utiliser la derni�re phrase reconnue par Google pour savoir quelle lumi�re vous avez demand� en dernier !
- Pour certains mots, Chrome est beaucoup plus pr�cis au niveau de la reconnaissance. C'est notamment le cas pour tous les mots en anglais mais aussi pour les nombres.
  

Inconv�nients
-------------
- tributaire de Google Chrome (plantages ? comment les d�tecter, relancer Chrome, etc)
- d�pend de la qualit� de la connexion (vitesse, stabilit�, disponibilit� des serveurs Google etc)
- Chrome peut entendre la r�ponse de Sarah ou d'autres bruits et donc renvoyer quelques chose d'erron� (solution ? couper le micro ?)
- il faut (l�g�rement) r��crire les plugins qui utiliseraient la r�gle GARBAGE si on veut utiliser ce principe (il faut aussi r��crire le code de speech_test.js pour qu'il utilise un certificat SSL qui ne change pas � chaque lancement ... mais bon c'est une autre histoire)

