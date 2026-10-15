# Changelog

## 1.3.3 — 2026-10-16

### Nouveautes

- Chat : longueur des reponses reglable depuis la barre d'outils (Court, Normal, Detaille). Court par defaut ; le choix est memorise.
- Chat : plus de tableaux, d'emojis ni de lignes de separation dans les reponses. Un tableau produit malgre tout s'affiche en liste lisible.
- Chat : un rappel de forme accompagne chaque question envoyee au modele, pour qu'il ne recopie pas la longueur et la structure de ses reponses precedentes.
- Mode hypothese : plus d'affaires, articles de loi ou statistiques cites hors du graphe, plus de recommandations adressees aux autorites.

### Correctifs

- Les dates envoyees au modele etaient decalees d'un jour (conversion en UTC) : une date du 18 septembre arrivait comme le 17, et les dates de proprietes partaient sous forme brute (`2026-05-03T22:00:00.000Z`). Toutes les dates sont desormais envoyees en heure locale, au format `AAAA-MM-JJ` (et `AAAA-MM-JJ HH:mm` pour les dates avec heure). Concerne le chat, la coherence, les observations, l'analyse croisee et l'affichage des candidats de fusion.
- Coherence et observations : le modele recoit la date du jour. Il ne considere plus les dates de l'annee en cours comme futures.
- Coherence : le type des elements (leurs tags) est transmis au modele, qui ne recevait que « ? ». Un document et l'acte ou le site dont il parle ne sont plus signales comme doublons.
- Coherence : les elements sont designes par des references courtes (E1, E2...) ; les doublons et contradictions renvoyes par le modele sont relies aux bons elements du graphe, et ceux qui ne correspondent a aucun element sont ecartes.
- Coherence : la synthese ne s'affiche plus dans un bloc de code, et le modele ne doit plus lister de « contradiction » qui n'en est pas une.
- Coherence : une paire d'elements sans aucun tag commun (un document et l'acte qu'il decrit, un site et sa societe) n'est plus proposee comme doublon, quoi qu'en dise le modele.
- Coherence : l'age calcule a la date du jour est fourni a cote de la date de naissance ; le modele ne signale plus de fausse incoherence age / date de naissance.
- Pseudonymisation (fournisseurs cloud) : les dates etaient prises pour des numeros de telephone et masquees (`TEL_02`), et un SIREN aussi. Le modele les declarait alors « non renseignees ». Les dates ne sont plus masquees ; le SIREN/SIRET est masque sous `IDENTIFIANT_NN` ; une societe taguee « Societe » est pseudonymisee comme organisation.
- Pseudonymisation : quand les donnees sont pseudonymisees, le modele est prevenu que les jetons (`PERSONNE_01`, `ELEMENT_04`...) remplacent des valeurs reelles et renseignees (chat, observations, rapports, analyse croisee).
- Coherence : avec un fournisseur cloud et la pseudonymisation activee, l'analyse de coherence envoyait les donnees en clair (noms, dates de naissance, SIREN). Elle est desormais pseudonymisee comme le chat, les observations et les rapports ; les resultats sont affiches en clair.
- Pseudonymisation : le modele n'elide plus devant un jeton (« d'Perigord Tir et Chasse » une fois le nom remis en clair).
- Observations : plus de qualification des faits (infraction, contournement, intention de masquer) ; une propriete est lue selon son nom (un SIREN reste un SIREN).

## 1.3.2 — 2026-10-11

### Correctif

- Sur certains navigateurs, la lecture du texte d'un PDF (extraction) echouait avec « getOrInsertComputed is not a function ». OneNeurone utilise desormais la version compatible de pdf.js.

## 1.3.1 — 2026-10-10

### Correctif

- Avec OneNeurone installe, des boutons de ZeroNeurone disparaissaient (en haut du dossier : theme, cote du panneau, etc.). Les classes de mise en forme embarquees par le plugin passaient avant celles de ZeroNeurone ; elles passent desormais apres (sous-couche `utilities.plugin`).

## 1.3.0 — 2026-10-10

### Extraction — sources au passage avec MarkNeurone (ZeroNeurone 2.64+)

- Avec MarkNeurone installe, un element, une relation ou un evenement accepte a pour source le passage du document ou il a ete lu, `[Piece 12 p.3](mn:1a2b3c4d)`, au lieu du document entier. MarkNeurone retrouve le passage (le nom, les mots d'un nom a l'autre, du nom a la date) et le surligne ; sinon la source reste au niveau du document.
- **Accepter auto** donne des passages a verifier dans MarkNeurone ; une entite acceptee une par une donne un passage valide.
- Le lecteur de MarkNeurone peut confier un document, ou la selection, a l'extraction : le panneau Cortex s'ouvre sur ce texte (service `oneneurone:extraction`, si la licence comprend l'extraction).
- Sans MarkNeurone ou avec une version de ZeroNeurone anterieure, rien ne change.
- Nouvelle extraction (lecteur MarkNeurone ou menu contextuel) alors que des entites restent a traiter : confirmation avant de remplacer les resultats affiches. Auparavant, le menu contextuel changeait la source en laissant les anciens resultats.

### Correctif

- Une date sans heure renvoyee par le modele (`2026-03-03`) etait lue comme minuit UTC et affichee 01:00 ou 02:00 en France. Elle est maintenant enregistree a 00:00 locale, que ZeroNeurone lit comme la journee entiere, comme dans MarkNeurone.

### Documentation

- Extraction : section « Avec MarkNeurone ».

## 1.2.8 — 2026-10-07

### Extraction — une source par ligne

- Le champ source s'ecrit maintenant avec une piece par ligne, au lieu de pieces separees par ` ; ` sur une seule ligne.
- Lecture des deux formats : un champ deja ecrit avec ` ; ` est toujours reconnu, et passe au format une piece par ligne la prochaine fois qu'une piece y est ajoutee.
- Correctif : dans un champ source sur plusieurs lignes, une piece deja citee pouvait etre ajoutee en double.

## 1.2.7 — 2026-10-07

### Extraction — sources cliquables (ZeroNeurone 2.62+)

- Une piece qui est un fichier s'ecrit maintenant comme un lien dans le champ source : `[Piece 12](asset:3fa1c2d9)`. ZeroNeurone 2.62 l'affiche cliquable et ouvre l'apercu du document ; un document absent du dossier apparait grise.
- Les sources issues des notes d'un element restent au format `Notes [on:3fa1c2d9]` (pas de fichier a ouvrir), de meme que toutes les sources avec une version de ZeroNeurone anterieure (detection par `features.sourceLinks`).
- Migration : une source deja ecrite `nom [on:3fa1c2d9]` est reconnue comme la meme piece et passe au format lien la prochaine fois qu'elle est citee. Une source editee a la main autour du lien n'est ni modifiee ni dupliquee.
- Libelles echappes (`]`, `\`) selon le parseur de ZeroNeurone.

### Documentation

- Extraction : sources cliquables, cas des notes et des fichiers non conserves, migration.

## 1.2.6 — 2026-10-07

### Extraction — provenance des pieces

- Une fusion avec un element existant perdait la trace du second document : le champ source n'etait pas complete et le lien `extrait de` n'etait pas cree. Chaque piece est maintenant ajoutee au champ source de l'element (`Piece 12 [on:3fa1c2d9] ; Piece 14 [on:8be07a11]`), separee par ` ; ` comme dans la fusion de ZeroNeurone. Le code identifie le document par le SHA-256 de son contenu : il est le meme pour un fichier depose ou joint au dossier, et survit a l'export/import.
- Relations et evenements deja presents ne sont plus recrees : la nouvelle piece est ajoutee a leur source.
- Plusieurs documents extraits ensemble : chaque entite, relation ou evenement n'est rattache qu'aux documents dont le texte le cite.
- Reference de piece (`piece 12`, `scelle 3`...) saisie dans la liste des sources, conservee par dossier (table `documentRefs`, chiffree) et ecrite a la place du nom du fichier. Les sources ecrites auparavant sous le nom du fichier sont mises a jour a la prochaine extraction du document.
- « Media en source » est coche par defaut.

### Extraction — fusion

- Propriete deja presente avec une autre valeur : choix entre **Garder l'existante**, **Remplacer** et **Conserver les deux** (deux proprietes de meme nom, comme la fusion de ZeroNeurone), au lieu d'un ecrasement (11 langues).
- Correction : l'ajout d'evenements a un element remplacait ses evenements existants.

### Canvas

- Les elements crees par le Neurone sont dimensionnes pour afficher leur label en entier lorsque la taille automatique de ZeroNeurone le tronquerait (forme du TagSet prise en compte, elements redimensionnes a la main inchanges).
- Les actions du menu contextuel ouvrent l'onglet Cortex du panneau lateral.

### Documentation

- Extraction : provenance des pieces, reference, fusion des proprietes ; tags du Tag Manager et onglet Tags (1.2.5).
- Theme hugo-book epingle en v0.14.0 (la v0.15.0 supprime le SASS utilise par la doc).

## 1.2.5 — 2026-10-03

### Extraction — tags du Tag Manager

- Les elements extraits recevaient des tags en doublon de ceux du Tag Manager (`personne` au lieu de `Personne`, `vehicule` au lieu de `Véhicule`, `compte` au lieu de `Compte bancaire`) et perdaient l'icone, la forme et la couleur du TagSet. Le type extrait est maintenant rattache au tag existant (sans tenir compte de la casse ni des accents), et place en premier : ZeroNeurone n'applique le visuel que du premier tag. `source:neurone` passe en second.
- Nouvel onglet **Tags** dans les parametres : associe chaque type extrait (y compris les types metier comme `ioc`, `infraction` ou les termes GLiNER) a un tag existant, choisi dans la liste des TagSets et des tags du dossier, avec filtre et icones (11 langues).

### Developpement

- Le point d'entree de developpement attend l'initialisation asynchrone d'i18n de ZeroNeurone avant d'enregistrer les plugins (`addResourceBundle is not a function`).

## 1.2.4 — 2026-10-01

### Menu contextuel

- « Demander au Neurone » et « Expliquer les connexions » transmettaient l'identifiant complet de l'element, alors que le contexte du graphe n'en expose que les 6 derniers caracteres : le modele repondait que l'element n'apparaissait pas dans les donnees. Le prompt cite desormais le libelle et l'identifiant court, au format du contexte. En mode compact, le libelle permet aussi au filtrage par mots-cles de retenir l'element et ses voisins.
- « Demander au Neurone » devient « Discuter de la selection » (11 langues).

### Endpoints personnalises

- Le chemin de l'URL saisie etait ignore : `https://hote/api/v1` listait les modeles sur `https://hote/v1/models`. Les URLs de chat et de liste des modeles sont maintenant derivees du chemin saisi (URL `/chat/completions` complete ou base d'API). Ollama conserve aussi le prefixe d'un reverse proxy pour `/api/tags`.

## 1.2.3 — 2026-08-24

### Extraction — documents volumineux

- Un document partait en une seule requete quelle que soit sa taille : au-dela d'un certain volume la reponse du modele etait coupee par la limite de tokens et seule une reparation partielle du JSON sauvait le resultat. Le texte est desormais decoupe en morceaux d'environ 14 000 caracteres, aux frontieres de sources puis de paragraphes, avec un recouvrement pour ne pas trancher une entite en deux.
- Les morceaux partent en parallele sur les fournisseurs cloud, sequentiellement en local. Le bouton indique le morceau en cours.
- Les resultats sont fusionnes : entites rapprochees par nom (confiance la plus haute, proprietes cumulees), relations et evenements dedupliques. En dessous du seuil de decoupage, le comportement est inchange.
- Un PDF de plus de 10 pages etait tronque silencieusement : l'avertissement est desormais affiche, avec le nombre de pages lues.

### Extraction — rapprochement avec le graphe

- La liste des elements deja presents dans le graphe n'etait transmise au modele que pour le prompt generique. Des que la classification reussissait — donc dans le cas courant — les sept prompts specialises partaient sans elle, et le rapprochement suggere par le modele (`suggested_match`) ne pouvait jamais se produire. La section de correspondance est maintenant commune a tous les types de documents.
- Detection de doublons : les libelles du graphe sont normalises une seule fois au lieu d'une fois par entite extraite, et les trois passes successives (exact, suggestion du modele, similarite) sont fusionnees en une.

### Cout et fiabilite des appels

- Le prompt de NER (7 a 12 Ko) et celui de classification sont desormais marques pour la mise en cache cote fournisseur, la liste des elements du graphe restant dans un bloc separe non cache. Le prefixe reste ainsi stable d'une extraction a l'autre et d'un morceau au suivant.
- Ollama : la taille de fenetre de contexte (`num_ctx`) etait calculee sur le budget de sortie seul, sans rapport avec la taille reelle du prompt. Comme Ollama tronque par la gauche, c'est le prompt systeme qui disparaissait en premier sur les gros documents. Elle est maintenant dimensionnee sur l'entree reelle, sans jamais descendre sous la valeur precedente.
- Une erreur sur un morceau interrompt desormais la file d'attente au lieu de laisser partir les requetes suivantes.
- Une annulation pendant la classification n'est plus confondue avec un echec de classification (qui faisait retomber silencieusement sur le prompt generique).

### Analyse

- Coherence et impulsions LLM, qui ne dependent pas l'une de l'autre, sont lancees en parallele au lieu de s'enchainer.
- Reparation du JSON tronque etendue a la classification de document et aux impulsions d'analyse — elle n'existait que pour l'extraction.
- Le contexte du graphe est memoise et recalcule uniquement quand les donnees changent, au lieu d'etre reserialise a chaque tour de dialogue et a chaque rendu de modale.
- Nettoyage des commentaires dans les reponses JSON : une URL contenant `//` voyait sa fin supprimee.

### Analyse croisee

- Le mode compact ne se declenchait jamais : la decision etait prise avant l'ajout de la moindre entite. Le contexte est desormais rendu en entier puis re-rendu en compact s'il depasse le budget.
- Troncature alignee sur l'estimation de tokens reellement utilisee (elle depassait le budget annonce d'environ 14 %).
- Comparaison des libelles entre enquetes : les paires dont les longueurs excluent deja tout rapprochement sont ecartees avant le calcul de distance.

## 1.2.2 — 2026-08-20

### Pseudonymisation — extraction depuis un document non importe

- La carte de pseudonymisation ne couvrait que les entites deja presentes dans le graphe : un PDF fraichement depose atteignait le LLM cloud sans redaction, y compris pour la passe de classification.
- La carte est desormais etendue avec le contenu du document lui-meme (regex email/telephone/IBAN + GLiNER local) avant tout envoi, et reutilisee a l'identique pour l'apercu de confidentialite et pour l'envoi reel.
- Les indices GLiNER inseres dans le prompt de NER sont desormais eux aussi passes par la sanitisation (ils pouvaient reintroduire des noms deja masques).
- Regex telephone corrigee : les mobiles francais a 5 groupes (06 XX XX XX XX) etaient tronques a 4, laissant les deux derniers chiffres en clair.
- La passe GLiNER de sanitisation utilise desormais un jeu de labels dedie (adresse, telephone, IBAN...) et un seuil de confiance abaisse, au lieu des labels generiques du type de document.
- Avertissement affiche quand GLiNER est desactive : le masquage des noms d'un document neuf en depend.

### Extraction — montants et proprietes de relation

- Le prompt PV judiciaire ne demandait jamais les montants (prix, versement, prejudice) — ajoute en propriete d'entite et dans la description des evenements.
- `ExtractedRelationship` supporte desormais des proprietes : cinq prompts (rapport d'enquete, corporate, sinistre, presse, generique) demandaient deja un `Montant`/`Devise` sur les relations financieres, mais le parseur les ignorait silencieusement.

### Extraction — ergonomie et tracabilite

- Le bouton "Extraire" ne se desactivait plus pendant la preparation de la sanitisation (passe GLiNER) ; il indique maintenant la phase en cours (analyse locale puis extraction LLM).
- Acceptation d'une entite extraite : choix cumulables entre creer un lien graphe vers le media source et/ou inscrire son libelle dans le champ source de l'entite.

## 1.2.1 — 2026-07-05

### Pseudonymisation renforcee

- **Alias de noms de personnes** : chaque token distinctif d'un nom de personne ou d'analyste ("Lemaire", "M. LEMAIRE", "Marc" seul dans une note) est remplace par le meme pseudonyme que le nom complet.
- **Frontieres de mots** : le remplacement respecte les frontieres de mots Unicode — un "Marc" mappe ne corrompt plus "Marche" ni "Marcus".
- **Insensibilite aux accents** : "Geneve" ecrit sans accent matche la cle "Genève", et inversement.
- **Scan inline etendu** : les telephones, emails et IBAN embarques dans les valeurs de proprietes, labels de liens et labels d'evenements sont desormais detectes (seules les notes et descriptions l'etaient).
- **Detection multilingue des cles de proprietes** : les cles sensibles sont reconnues dans les 11 langues du plugin (telefono, handy, komórka, телефон, indirizzo, dowód, geburtsdatum...), avec matching en debut de token pour eviter les faux positifs ("hotel" ne declenche pas "tel").
- **Dates de naissance** : les cles de type "ne le" / birth / geburtsdatum sont pseudonymisees.
- Le nom du dossier beneficie des alias de forme courte comme les organisations.
- Valide par un banc de 14 cas : fuites, faux positifs, accents, aller-retour, mode expurgation.

## 1.2.0 — 2026-07-05

### Prompts personnalises

- **Modeles de rapport personnalises** : l'analyste peut creer ses propres modeles de rapport (nom + prompt libre), listes dans le menu de generation sous les 5 registres natifs. CRUD complet, persistance locale (Dexie v8), import/export JSON pour partage entre postes. Les modeles partent d'un squelette neutre — les prompts natifs restent chiffres et ne sont jamais exposes.
- **Consignes libres par rapport** : champ optionnel a chaque generation (destinataire, angle, longueur, points a developper), ajoute au prompt sans affaiblir les regles de factualite.
- **Instructions persistantes du dialogue** : instructions par dossier (bouton reglages dans la toolbar du chat), appliquees a chaque question.

### Optimisation du contexte (tokens)

- Le contexte graphe est place dans un **bloc systeme dedie et cacheable**, avant les instructions de tache : prompt caching Anthropic (lectures en cache a 10% du prix sur dialogue multi-tour, regeneration de section, verification) et reutilisation du KV-cache local (Ollama, LM Studio, WebLLM) entre taches.
- **Profils de contexte par tache** : rapport (sans index des entites), analyse (notes 150, sans extraits de documents), dialogue (inchange).
- **Liens** : detail complet cote source uniquement (au lieu d'une duplication → et ←), notes de lien nettoyees et tronquees a 200 caracteres.
- **Groupes homogenes en tables** : les entites de meme type et memes proprietes (>= 8, typique des imports forensiques) sont serialisees en table markdown — cles ecrites une fois par groupe.
- Estimation de tokens calibree pour le francais (3.5 chars/token au lieu de 4).
- Historique de dialogue plafonne aux 12 derniers messages envoyes au LLM (l'UI affiche tout).

### Corrections

- **Double enregistrement du plugin en dev workspace** : le manifest de plugins est filtre par le serveur de dev (le oneneurone.js compile n'est plus charge en plus du plugin workspace) et `registerSlots()` retire ses extensions precedentes avant de re-enregistrer.

## 1.1.22 — 2026-07

- Provider Mistral AI + OCR d'images.

## 1.1.20 — 2026-06

- Fallback OCR vision pour les PDF scannes attaches au graphe.

## 1.1.19 — 2026-06

- Plafonds du context builder (echantillonnage evenements, textes extraits).
- Liste de modeles Anthropic dynamique.
- Fusion differentielle + rapprochement d'entites assiste par LLM.

## 1.1.17 — 2026-05

- Durcissement securite : air-gap, detection PII inline, validation de schema LLM, decompression DOCX securisee.
