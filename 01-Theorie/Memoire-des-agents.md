# Memoire des agents

## Contexte et necessite

Un agent IA traite des requetes successives. Sans mecanisme de memoire, chaque interaction est independante : l'agent ne se souvient pas des echanges precedents, des preferences de l'utilisateur ou des donnees deja recuperees. Pour des taches prolongees ou iteratives (comme la construction d'une strategie marketing), une forme de memoire est indispensable.

## Types de memoire

### Memoire de conversation (court terme)

- Contient les derniers messages echanges dans la session courante
- Geree automatiquement par n8n dans le contexte du noeud AI Agent
- Limitee par la taille du contexte du modele (fenetre de contexte)

### Memoire persistante (long terme)

- Stockage externe : base de donnees, fichiers, vecteurs
- Permet de conserver des informations entre les sessions
- Utilisee pour les preferences utilisateur, l'historique des analyses, les donnees RAG

### Memoire de travail (contexte de la requete)

- Donnees injectees dans le prompt a chaque tour (URL analysee, resultats d'outils)
- Ephemere : disparait a la fin du cycle de traitement
- Essentielle pour que l'agent s'appuie sur les resultats de ses appels d'outils

## Implementation dans n8n

Dans n8n, la memoire de conversation est geree via le parametrage du noeud AI Agent (option "Conversation" ou "Memory"). Pour une memoire persistante, il faut connecter des noeuds de base de donnees ou de fichiers qui stockent et recuperent les informations entre les executions.

## Relation avec l'autonomie

La memoire influe sur les decisions de l'agent : s'il dispose d'informations recentes en contexte, il peut choisir de ne pas rappeler un outil. A l'inverse, l'absence d'informations pertinentes le conduit a invalider une hypothese et a declencher une recherche supplementaire.
