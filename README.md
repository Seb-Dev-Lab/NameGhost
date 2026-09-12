# 👻 NameGhost

**Le skill qui retrouve les fantômes de ton ancien branding.**

Tu viens de renommer ton SaaS, ton app ou ton projet ? `NameGhost` inspecte le repo et retrouve les endroits où l'ancien nom, domaine ou slug est encore présent : UI, SEO, manifests, variables d'environnement, Docker, documentation, URLs, noms de fichiers, identifiants techniques, etc.

Il distingue aussi les occurrences qu'on peut renommer sans risque de celles qui nécessitent une vraie migration.

## Exemple

Tu passes de `TaskPilot` à `FlowPilot`.

Lance le skill avec l'ancien et le nouveau nom :

```text
/name-ghost "TaskPilot" "FlowPilot"
```

Exemple de résultat :

```text
👻 NameGhost — 14 traces trouvées
🚨 3 visibles utilisateur
⚙️ 7 techniques
📚 3 documentation/tests
🧊 1 historique à conserver

🚨 src/app/metadata.ts:12
Ancien nom : "TaskPilot"
Pourquoi ça compte : visible dans le titre SEO.
Action : remplacer

⚙️ ios/App.xcodeproj/project.pbxproj
Ancien identifiant : com.taskpilot.app
Action : migration à vérifier

❌ Rebranding incomplet
```

## Ce que NameGhost vérifie

- textes visibles, footer, emails et templates ;
- metadata, SEO, Open Graph et manifestes ;
- manifests de packages ;
- `.env.example` et variables d'environnement ;
- Docker, CI/CD et scripts de déploiement ;
- domaines, URLs, callbacks OAuth, CORS et webhooks ;
- identifiants mobiles et ressources cloud ;
- README, docs, tests et fixtures ;
- noms de fichiers, dossiers et assets.

Par défaut, NameGhost **audite sans modifier**. Si tu lui demandes ensuite de corriger, il remplace uniquement les occurrences sûres et signale séparément les identifiants qui nécessitent une migration.

## Installation

### Claude Code

Installation personnelle :

```bash
git clone https://github.com/Seb-Dev-Lab/NameGhost.git ~/.claude/skills/name-ghost
```

Ou uniquement dans un projet :

```bash
git clone https://github.com/Seb-Dev-Lab/NameGhost.git .claude/skills/name-ghost
```

Puis :

```text
/name-ghost "AncienNom" "NouveauNom"
```

### Codex

Installation personnelle :

```bash
git clone https://github.com/Seb-Dev-Lab/NameGhost.git ~/.agents/skills/name-ghost
```

Ou uniquement dans un projet :

```bash
git clone https://github.com/Seb-Dev-Lab/NameGhost.git .agents/skills/name-ghost
```

Puis invoque le skill dans Codex :

```text
$name-ghost "AncienNom" "NouveauNom"
```

### Autres clients compatibles Agent Skills

Le cœur du projet est un `SKILL.md` portable. Copie simplement le dossier dans l'emplacement de skills pris en charge par ton client.

## Pourquoi ce skill existe

Un rebranding paraît simple jusqu'au moment où l'ancien nom réapparaît dans :

- une balise SEO ;
- un email transactionnel ;
- un ancien domaine ;
- un manifest mobile ;
- une variable d'environnement ;
- ou un identifiant qu'il ne fallait surtout pas remplacer aveuglément.

`NameGhost` transforme ce nettoyage en audit reproductible.

## Philosophie SebDevLab

NameGhost fait partie des **micro-skills SebDevLab** : des outils très ciblés, rapides à comprendre et conçus pour résoudre un vrai petit problème de développement sans ajouter une nouvelle usine à gaz.

## Licence

MIT — libre à utiliser, modifier et partager.
