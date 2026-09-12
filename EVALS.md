# Évaluations manuelles — NameGhost

Ces scénarios servent à vérifier que le skill reste utile sans devenir un simple « rechercher/remplacer ».

## Évaluation 1 — Rebranding SaaS classique

**Contexte**

Ancien nom : `TaskPilot`  
Nouveau nom : `FlowPilot`

Le repo contient volontairement :

- `TaskPilot` dans `metadata.ts` ;
- `TASKPILOT_API_URL` dans `.env.example` ;
- `taskpilot-logo.svg` dans `public/` ;
- une mention `TaskPilot` dans le README ;
- une mention dans un changelog expliquant l'ancien nom.

**Attendu**

- quatre traces à corriger ou vérifier ;
- la mention du changelog classée en **historique volontaire** ;
- aucun remplacement automatique sans demande explicite.

## Évaluation 2 — Changement de domaine

**Contexte**

Ancien domaine : `taskpilot.io`  
Nouveau domaine : `flowpilot.app`

Le repo contient :

- `api.taskpilot.io` dans une config ;
- `support@taskpilot.io` dans un template email ;
- `https://taskpilot.io/oauth/callback` dans une config OAuth ;
- une ancienne URL dans un test snapshot.

**Attendu**

- email et URL publique classés **visible utilisateur** ou **technique** selon leur usage ;
- callback OAuth marqué **migration à vérifier** ;
- snapshot classé documentation/test ;
- pas de conclusion « tout remplacer ».

## Évaluation 3 — Faux positifs et identifiants externes

**Contexte**

Ancien nom : `Pulse`

Le mot `pulse` apparaît de nombreuses fois comme terme métier générique. Le bundle ID historique est `com.company.pulse` et l'application est déjà publiée.

**Attendu**

- le skill évite de traiter chaque occurrence générique de `pulse` comme du branding ;
- le bundle ID est signalé comme **migration à vérifier** ;
- le rapport explique l'ambiguïté au lieu d'inventer une certitude.
