# Configuration

## Fichier docker-compose.yml

Ce fichier permet de lancer n8n dans un conteneur Docker. Ollama et TinyLlama doivent etre installes separement sur la machine hote (voir README principal).

## Comment creer l'environnement

### Etape 1 : Installer Ollama et TinyLlama

1. Installer Ollama depuis [ollama.com](https://ollama.com)
2. Ouvrir un terminal et executer : `ollama pull tinyllama`
3. Verifier : `ollama list` affiche tinyllama

### Etape 2 : Lancer n8n avec Docker

1. Placer ce dossier (`02-Configuration`) dans un repertoire accessible
2. Ouvrir un terminal dans ce dossier
3. Executer : `docker-compose up -d`
4. Attendre le demarrage (quelques secondes)
5. Ouvrir `http://localhost:5678` dans un navigateur

### Etape 3 : Configuration dans n8n

1. Creer un compte utilisateur a la premiere connexion
2. Creer des identifiants Ollama : Parametres > Identifiants > Ajouter > Ollama
3. URL de base : `http://host.docker.internal:11434`
4. Enregistrer sous le nom "Ollama local"

### Parametres importants du docker-compose

- **Port 5678** : Interface web de n8n
- **Volume n8n_data** : Persistance des workflows et des donnees utilisateur
- **extra_hosts** : Necessaire sur Linux pour que le conteneur accede a Ollama sur l'hote via `host.docker.internal`

### Lancement

```bash
docker-compose up -d
```

### Arret

```bash
docker-compose down
```
