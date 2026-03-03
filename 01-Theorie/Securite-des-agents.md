# Sécurité et contrôle d'un Agent IA local

L'un des avantages majeurs de l'approche locale est le contrôle des données. Cependant, un agent IA est conçu pour interagir avec son environnement. Si on lui donne les mauvais outils ou trop de permissions, il pourrait effectuer des actions indésirables sur votre machine ou vos comptes (supprimer des fichiers, envoyer des emails non sollicités, etc.).

Ce document explique les bonnes pratiques pour sécuriser votre environnement agentique.

## 1. L'isolation par conteneurisation (Docker)

Dans l'architecture proposée, **n8n s'exécute dans un conteneur Docker**. C'est la première ligne de défense de votre système.

* **Séparation du système hôte** : Le processus n8n (et donc les outils de l'agent qui s'y exécutent, comme l'outil d'exécution de code ou les requêtes HTTP locales) n'a pas accès directement aux fichiers de votre PC (Windows/Mac/Linux), sauf si vous avez explicitement monté un volume Docker.
* **Risque limité** : Si l'agent décide d'exécuter une commande système ou de lire un fichier local, il le fera à l'intérieur du conteneur isolé. Il ne pourra pas accéder à vos documents personnels, mots de passe, etc., situés sur la machine hôte.

**Recommandation** : Ne montez que les répertoires strictement nécessaires dans Docker. Ne donnez jamais accès à la racine de votre système (`C:\` ou `/`) au conteneur n8n.

## 2. Le principe du moindre privilège pour les Outils (Tools)

Un agent n'est capable de faire que ce que ses outils lui permettent. C'est vous qui décidez quels outils sont connectés au nœud "AI Agent".

* **Lecture seule vs Écriture** : Privilégiez toujours les accès en lecture seule. Par exemple, donnez un outil de "Recherche de documents" plutôt qu'un outil "Modification de fichiers".
* **Tokens avec périmètre restreint (Scopes)** : Si l'agent a besoin d'interagir avec une API tierce (ex: envoyer un email via Gmail, lire des données dans Odoo), utilisez un token d'API dont les droits sont limités à cette seule action. Ne fournissez pas de clé d'administration globale (Admin API Key).
* **Déconnexion des outils sensibles** : Si vous testez des prompts aléatoires ou que vous explorez, déconnectez purement et simplement les outils capables de modifier l'état d'un système (comme l'outil d'exécution de code Python/JS ou de requêtes SQL).

## 3. Le "Human in the Loop" (HITL) : Validation manuelle

La meilleure sécurité reste la supervision humaine. Lors de la conception de workflows complexes où l'agent peut entreprendre des actions critiques, insérez une étape de validation avant l'exécution finale.

Dans n8n, cela se traduit par :
1. L'agent prépare l'action (ex: il rédige un email, ou formule une requête d'insertion en base de données).
2. Au lieu de l'exécuter directement, l'agent envoie cette proposition au sein du workflow via une interface de validation (nœud "Wait" ou envoi d'un message sur Slack/Discord/Email avec des boutons d'approbation).
3. **L'action ne s'exécute que si l'humain valide.**

Cette approche "Human in the Loop" est essentielle en entreprise pour les agents qui interagissent directement avec les clients ou les systèmes financiers/ERP.

## 4. Filtrage des prompts et garde-fous

Bien que moins strict que l'isolation technique, le prompt système ("System Message") est crucial pour définir les limites comportementales de l'agent.

* **Instructions d'interdiction explicites** : Incluez dans le prompt des règles strictes. Exemple : `Tu n'es autorisé qu'à LIRE des informations, tu ne dois JAMAIS tenter de modifier, créer ou supprimer des données.`
* **Vérification (LLM Firewall)** : Dans des architectures plus avancées, un second LLM (souvent plus petit et rapide) peut analyser la réponse de l'agent principal avant son exécution pour s'assurer qu'elle ne contient pas d'instructions dangereuses ou d'extraction de données sensibles.

---
**En résumé** : Gardez n8n sous Docker, limitez les accès des outils au strict nécessaire (lecture seule), et ajoutez des étapes de validation humaine pour toute action modifiant l'état d'un système.
