# RAG : Retrieval Augmented Generation

## Definition

Le RAG (Retrieval Augmented Generation) designe une architecture qui combine la generation de texte par un modele de langage avec un systeme de recuperation d'informations exterieures. Au lieu de ne s'appuyer que sur les connaissances internes du modele (entraine jusqu'a une date fixe), le systeme recupere des documents pertinents dans une base de donnees ou des sources externes, puis injecte ces documents dans le contexte du prompt avant la generation.

## Principe de fonctionnement

1. **Indexation** : Des documents (PDF, pages web, fichiers texte) sont indexes dans un systeme de recherche vectorielle ou une base de donnees.
2. **Recuperation** : Lors d'une requete utilisateur, le systeme cherche les passages les plus pertinents par similarite semantique ou par mots-cles.
3. **Augmentation** : Les passages recuperes sont ajoutes au prompt envoye au modele de langage.
4. **Generation** : Le modele genere une reponse en s'appuyant sur le contexte fourni, ce qui limite les hallucinations et ameliore la precision.

## RAG et agents IA

Dans un workflow agentique, le RAG peut etre implemente comme un outil (tool) : l'agent decide d'appeler un outil de recherche documentaire lorsqu'il estime avoir besoin d'informations complementaires. La logique est la meme que pour la recherche web : l'agent evalue la requete et choisit d'utiliser l'outil de recuperation plutot que de repondre a partir de son parametrage interne.

## Avantages

- Mise a jour des connaissances sans reentrainer le modele
- Reduction des reponses inventees grace a l'ancrage dans des documents reels
- Possibilite de travailler sur des corpus prives (donnees d'entreprise, documentation interne)

## Limites

- Qualite de la recherche : si les documents recuperes sont peu pertinents, la reponse peut etre degradee
- Cout en tokens : l'ajout de contexte augmente la taille des requetes
