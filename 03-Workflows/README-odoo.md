# Intégration Odoo : Création de Skills pour l'Agent

Cette section explique comment ajouter des "compétences" (skills) à votre agent IA pour qu'il puisse interagir avec une instance Odoo (ERP).

## Prérequis

1. **Une instance Odoo** : Accessible via une URL (ex: `https://mon-entreprise.odoo.com`).
2. **Des identifiants Odoo** : Un nom d'utilisateur (email), un mot de passe (ou clé d'API) et le nom de la base de données.
3. **Le workflow `agent-odoo-exemple.json`** importé dans votre n8n local.

## Objectif du Workflow Exemple

Le workflow fourni (`agent-odoo-exemple.json`) illustre comment un agent peut répondre à des requêtes complexes en interrogeant Odoo de manière autonome.

* **Exemple de requête utilisateur** : "Quel est le produit le plus cher de notre catalogue et combien en avons-nous en stock ?"
* **Action de l'agent** : L'agent comprend qu'il doit interroger la base de données Odoo, il appelle l'outil (Tool) "Recherche Produit Odoo", récupère la liste des produits sous forme de JSON, filtre l'information, et formule sa réponse finale.

## Configuration dans n8n

### 1. Importer le JSON
Dans n8n, allez dans **Workflows** > **Import from File** et sélectionnez le fichier `03-Workflows/agent-odoo-exemple.json`.

### 2. Configurer les Credentials Odoo
Le nœud "Odoo Tool" a besoin d'identifiants pour se connecter à votre instance.
1. Cliquez sur le nœud Odoo (qui agit ici comme un outil pour l'agent).
2. Dans le panneau de droite, créez de nouveaux identifiants "Odoo API".
3. Renseignez l'URL de votre instance, la base de données, l'email et la clé d'API (ou mot de passe).

### 3. Ajuster le Prompt Système de l'Agent
L'agent a besoin d'instructions claires sur la façon d'utiliser l'outil Odoo. Dans le nœud "AI Agent", le prompt système inclus :
> "Tu es un assistant connecté à l'ERP Odoo de l'entreprise. Utilise l'outil 'Recherche Produit Odoo' pour obtenir des informations sur le catalogue. Ne donne que l'information demandée sans inventer de données."

### 4. Gérer les risques (Sécurité)
**Important :** Par défaut, donnez uniquement des accès en **lecture seule** (Read) sur les modèles Odoo non critiques (ex: Produits, Catalogues). Ne créez pas d'outils de modification de données (Write/Update) sans avoir mis en place des validations humaines (comme vu dans `Securite-des-agents.md`), sous peine que l'agent supprime ou altère des données commerciales importantes par erreur.

## Aller plus loin : Créer d'autres Outils Odoo
Dans le workflow n8n, un outil pour agent (Tool) est souvent un simple composant qui expose une action.
Pour ajouter une nouvelle compétence (ex: "Lire un contact"), ajoutez un nouveau nœud Odoo, configurez son opération sur "Get" (Resource : Contact/Partner), et reliez-le à l'entrée "Tools" du nœud AI Agent. N'oubliez pas de donner un nom explicite à l'outil dans n8n, car c'est ce nom que le LLM verra et choisira d'utiliser.
