> ℹ️ **Ce repo n'est pas un projet en prod** : il existe parce qu'on s'en sert d'exemple pour la présentation « Les bonnes pratiques de dev, à l'ère du vibe coding ». Il peut disparaître sans que ça soit grave — l'important, c'est que vous ayez pris les infos et que vous les appliquiez sur vos propres projets.

# vibe-coding-for-infra

Support et bonnes pratiques pour coder avec l'IA — à partir de la présentation « Les bonnes pratiques de dev, à l'ère du vibe coding » (Nicolas Walthéry · SophIA Portal · KEYES).

---

## Bonnes pratiques de dev, à l'ère du vibe coding

Thèse : les bonnes pratiques de dev existaient bien avant l'IA. Elles comptent encore plus aujourd'hui — non pas pour « faire comme les devs », mais pour donner à l'IA les moyens de bien travailler.

### Ce qu'est une bonne pratique

Une habitude qui rend le code compréhensible, modifiable et vérifiable par quelqu'un d'autre que son auteur. Ce « quelqu'un d'autre », c'est souvent vous dans trois mois… ou l'IA à la prochaine demande.

**Organisation du projet**

- Un Git workflow clair : `main`, `develop` et une branche par fonctionnalité
- Un repo par projet : un historique lisible, des droits d'accès gérés projet par projet
- Une documentation claire : commencer par le *pourquoi* — quelle tâche ou quelle « souffrance » le projet résout (c'est aussi le meilleur contexte à donner à l'IA)

**Qualité du code**

- Clean code : des noms explicites, une fonction = une responsabilité
- DRY : pas de duplication
- KISS : le simple avant le malin
- Pas de code mort

### Pourquoi ces pratiques aident aussi votre IA

- Une erreur de l'IA ne coûte rien s'il existe un commit propre juste avant
- L'IA peut vérifier son propre travail s'il y a des tests à lancer
- L'IA comprend mieux votre code s'il est lisible : meilleur contexte, meilleures réponses

### 00 · L'outillage

VS Code et GitHub, reliés dès le départ.

- **VS Code** : l'éditeur (on y écrit et on relit), l'assistant IA (on y code avec l'IA), le panneau Source Control (là où Git devient visible : changements, commits, branches). Un autre IDE fait l'affaire : les concepts sont les mêmes.
- **Nouveau projet** : créer le repo sur GitHub (dans l'organisation, en privé), cocher un README et le `.gitignore` du langage, puis « Cloner le dépôt » dans VS Code.
- **Projet déjà sur votre PC** : ouvrir le dossier dans VS Code, **vérifier le `.gitignore` avant tout**, puis « Publish to GitHub », en privé.

> ⚠️ Le `.env` et ses clés partent souvent au premier push. Le `.gitignore` se vérifie **avant**, pas après. Une clé poussée sur GitHub doit être considérée comme compromise, même si on supprime le fichier ensuite : elle reste dans l'historique.

### 01 · Git workflow

**Le workflow classique**

```
main      = ce qui tourne
develop   = ce qui est prêt
feature/… = le travail se fait dans les branches
```

1. Partir de `develop` → créer `feature/nom-parlant` (un bac à sable : l'IA peut tout casser dedans)
2. Committer à chaque étape qui fonctionne (le point de sauvegarde)
3. Pull Request vers `develop` et relecture du diff (c'est là qu'on découvre que l'IA a modifié dix fichiers au lieu d'un)
4. `develop` → `main` à la livraison

**Version IA**

- Un commit avant chaque prompt important : si le résultat ne plaît pas, on revient en arrière en un clic au lieu de demander à l'IA de « défaire »
- Le diff remplace la confiance aveugle : on ne relit pas tout le code, on relit ce qui a changé
- L'IA peut rédiger les messages de commit — excellent usage, à condition de les relire (un message faux est pire que pas de message)
- Protéger `main` et `develop` sur GitHub : une règle imposée par l'outil tient mieux qu'une règle qu'on se rappelle

### 02 · Tests

- **Unitaires** : vérifient une fonction isolée, rapides (ex. : le calcul d'une remise)
- **End-to-end** : simulent un utilisateur dans l'application, plus lents et plus fragiles (ex. : se connecter et passer une commande)
- Automatisés : lancés par GitHub Actions sur chaque Pull Request, **pas** par l'agent à chaque prompt (sinon il devient très lent)

> ⚠️ Le minimum à retenir : si l'IA écrit le code et les tests, elle peut écrire des tests qui valident ses bugs, voire modifier un test pour le faire passer. **Les valeurs attendues, c'est vous qui les décidez, pas l'IA.**

### 03 · Clean code

Du code lisible par un humain, donc aussi par l'IA. Trois règles, pas un dogme :

1. **Des noms explicites** — `calculer_prix_ttc()` plutôt que `calc2()` : le nom dit ce que fait le code
2. **Une fonction, une chose** — si on doit écrire « et » pour la décrire, il faut la couper
3. **Pas de code mort** — ce qui ne sert plus disparaît, Git s'en souvient pour vous

Demander à l'IA de nettoyer… puis vérifier que les tests passent toujours. On ne nettoie sereinement que si on peut revenir en arrière (Git) et vérifier que rien n'a cassé (tests).

### À retenir

- `main` fonctionne toujours : un commit avant chaque prompt important
- Les valeurs attendues des tests, c'est vous qui les décidez
- Un code lisible, c'est une IA qui comprend mieux

### Pour aller plus loin : des MCP utiles

| MCP | Usage |
| --- | --- |
| **GitHub MCP** | Créer ses repos, issues et Pull Requests depuis l'agent (ex. avec opencode) |
| **Playwright MCP** | Piloter un vrai navigateur : utile pour les tests end-to-end |
| **Atlassian** | Lire et mettre à jour les tickets Jira et les pages Confluence |

> ⚠️ Chaque MCP alourdit le contexte de l'agent : n'activez que ceux que vous utilisez, avec des droits minimaux. Uniquement des MCP officiels, jamais de token dans le code ni dans un fichier poussé sur GitHub.
