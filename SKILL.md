---
name: name-ghost
description: Traque les traces résiduelles d'un ancien nom, domaine ou branding après un renommage de projet. Utiliser après un rebranding, un changement de nom de produit, de package, d'application ou de domaine pour trouver les références encore visibles ou techniques dans le code, la configuration, les métadonnées, la documentation et les chemins de fichiers.
---

# NameGhost

Traque les « fantômes » d'un ancien nom après un rebranding.

## Entrées

Obtiens ou déduis :

- **ancien nom** : obligatoire ;
- **nouveau nom** : facultatif mais recommandé ;
- **ancien domaine / ancien slug / ancien identifiant** : facultatifs.

Si l'ancien nom n'est pas identifiable dans la demande ou le contexte, demande-le avant de lancer l'audit. Ne demande pas le nouveau nom s'il n'est pas nécessaire pour trouver les traces.

## Mode par défaut

Effectue d'abord un **audit sans modifier les fichiers**.

Ne remplace rien automatiquement sauf si l'utilisateur demande explicitement de corriger les traces trouvées. Même dans ce cas, ne remplace jamais aveuglément une occurrence ambiguë ou un identifiant potentiellement externe.

## Recherche

### 1. Construire les variantes utiles

À partir de l'ancien nom, recherche l'orthographe exacte et les variantes évidentes réellement pertinentes :

- casse : `OldName`, `oldname`, `OLDNAME` ;
- séparateurs : `old-name`, `old_name`, `old name` ;
- formes compactes ou de slug si elles correspondent au projet ;
- ancien domaine, sous-domaines et adresses email associées lorsqu'ils sont connus.

N'invente pas une longue liste de variantes spéculatives. Si l'ancien nom est très court ou générique, privilégie les occurrences liées au contexte du produit pour éviter les faux positifs.

### 2. Chercher dans le contenu ET dans les chemins

Inspecte les fichiers suivis ou utiles du projet, mais aussi les noms de fichiers et de dossiers.

Priorité élevée :

- titres, textes UI, footer, emails et templates ;
- SEO, metadata, Open Graph, manifestes, PWA ;
- `package.json`, `pyproject.toml`, `Cargo.toml` et autres manifests ;
- variables d'environnement, `.env.example`, noms de services ;
- Docker, CI/CD, scripts de déploiement, IaC ;
- URLs, domaines, CORS, callbacks OAuth, webhooks ;
- bundle IDs, package IDs, app IDs et identifiants mobiles ;
- noms de bases, buckets, queues, cron jobs ou ressources cloud ;
- README, docs, tests, fixtures et exemples ;
- noms de fichiers, dossiers et assets.

Évite par défaut :

- `.git/` et l'historique Git ;
- `node_modules/`, `vendor/`, caches et sorties de build ;
- binaires ;
- fichiers générés non suivis.

Les lockfiles peuvent être inspectés si l'occurrence semble appartenir aux métadonnées du projet. Ignore les occurrences qui viennent seulement d'une dépendance tierce sans rapport avec le rebranding.

### 3. Classer chaque occurrence

Classe chaque résultat dans une seule catégorie :

- **🚨 Visible utilisateur** : peut exposer l'ancien branding dans l'interface, le SEO, un email, une URL publique ou un store.
- **⚙️ Technique** : configuration, code, infrastructure, identifiant, variable ou ressource interne.
- **📚 Documentation / test** : README, docs, fixtures, snapshots, exemples ou tests.
- **🧊 Historique volontaire** : changelog, migration, compatibilité, commentaire historique ou autre occurrence qui doit probablement rester.

Pour chaque résultat, donne le **chemin précis** et, lorsque possible, la ligne ou le symbole concerné.

### 4. Distinguer remplacement simple et migration risquée

Signale explicitement les occurrences qui ne doivent pas être renommées sans vérification :

- bundle/package IDs déjà publiés ;
- noms de buckets ou ressources cloud existantes ;
- noms de bases ou schémas ;
- secrets / clés externes ;
- callbacks OAuth et webhooks enregistrés chez un fournisseur ;
- URLs publiques déjà utilisées ;
- identifiants de paiement, analytics ou stores.

Pour ces cas, écris **« migration à vérifier »** au lieu de recommander un remplacement automatique.

### 5. Vérifier le nouveau nom si fourni

Si un nouveau nom est fourni :

- vérifie que les principaux points d'entrée utilisent bien le nouveau branding ;
- repère les mélanges ancien/nouveau nom dans une même surface ;
- ne considère pas l'absence du nouveau nom comme une erreur partout : certains fichiers n'ont aucune raison de contenir le branding.

## Sortie attendue

Commence par un verdict court :

```text
👻 NameGhost — X traces trouvées
🚨 X visibles utilisateur
⚙️ X techniques
📚 X documentation/tests
🧊 X historiques ou à conserver
```

Puis liste les occurrences par gravité avec :

```text
[catégorie] chemin:ligne
Ancien nom : "..."
Pourquoi ça compte : ...
Action : remplacer / vérifier / conserver
```

Termine par :

- **À corriger avant publication** : uniquement les éléments réellement bloquants ;
- **Migrations à vérifier** : identifiants ou ressources externes risqués ;
- **Verdict final** : `✅ Aucun fantôme critique` ou `❌ Rebranding incomplet`.

Si aucune occurrence pertinente n'est trouvée, dis-le clairement et indique les zones effectivement inspectées. Ne fabrique jamais de résultat pour rendre le rapport plus intéressant.

## Correction optionnelle

Si l'utilisateur demande de corriger :

1. corrige d'abord les occurrences sûres et non ambiguës ;
2. ne touche pas aux occurrences classées **historique volontaire** ;
3. demande ou signale une validation humaine pour toute **migration à vérifier** ;
4. relance la recherche après les modifications ;
5. termine par le nombre de fantômes restants.

Le but n'est pas de remplacer le plus de texte possible. Le but est de terminer un rebranding sans casser les identifiants qui ont une vie en dehors du code.
