# Workflows

## Fichier agent-marketing-exemple.json

Ce fichier contient un workflow n8n exemple pour l'agent de marketing. Il peut etre importe dans n8n pour servir de point de depart.

### Structure du workflow

Le workflow comprend :

1. **Chat Trigger** : Declencheur qui fournit une interface de chat a l'utilisateur
2. **AI Agent** : Noeud principal qui orchestre les appels au modele et aux outils
3. **Ollama Chat Model** (sous-noeud) : Modele de langage local (TinyLlama recommande, ou Llama 3, Mistral)
4. **HTTP Request Tool** (sous-noeud) : Outil permettant a l'agent de recuperer le contenu d'une URL web
5. **Simple Memory** (optionnel) : Memoire de conversation pour les echanges multi-tours

### Import dans n8n

1. Ouvrir n8n (http://localhost:5678)
2. Menu Workflow > Importer depuis un fichier
3. Selectionner le fichier `agent-marketing-exemple.json`
4. Configurer les identifiants Ollama (voir ci-dessous)
5. Activer le workflow

### Configuration requise apres import

**Identifiants Ollama** : Creer une connexion Ollama dans n8n (Parametres > Identifiants) avec :
- URL de base : `http://host.docker.internal:11434` (si n8n est dans Docker et Ollama sur l'hote)
- Modele : `tinyllama` (recommande), `llama3` ou `mistral` (selon le modele installe)

**Prompt systeme** : Le prompt definit le role de l'agent (consultant marketing) et les instructions pour utiliser l'outil de recuperation d'URL. Adapter si besoin.

### Cas d'usage

L'utilisateur peut envoyer une requete du type : "Analyse le site https://exemple.com et propose une strategie de contenu marketing."

L'agent choisira d'appeler l'outil HTTP Request pour recuperer le contenu de l'URL, puis generera une strategie coherente a partir des informations collectees.

### Remarque sur l'autonomie

L'agent decide lui-meme d'utiliser l'outil de recuperation web lorsqu'il estime avoir besoin du contenu d'une page. Il ne repond pas directement a partir de ses parametres internes, car il n'a pas acces aux pages web sans cet outil. Cette autonomie de decision illustre la difference entre un workflow lineaire et un workflow agentique.
