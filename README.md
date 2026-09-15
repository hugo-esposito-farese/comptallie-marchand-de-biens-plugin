# Comptallie — plugin Claude.ai, suite Marchand de biens

Ce repo est l'**emballage public** du plugin Claude.ai pour la **suite
commerciale marchand de biens** de Comptallie (repo privé `Comptallie_MCP`).
Même principe que `comptallie-immobilier-plugin` et
`comptallie-conciergerie-plugin` : un repo par suite, cloisonné (cf.
`Comptallie_MCP/CLAUDE.md` section 5/6bis).

**Un agent disponible** : `projection-renovation` (`/projection-renovation`
— génère une projection visuelle d'une pièce rénovée par IA générative à
partir de photos de la pièce et de photos d'objets de référence, chacun
positionné par une description textuelle précise plutôt que par un plan
annoté — cf. section "Marchand de biens" de `Comptallie_MCP/CLAUDE.md`,
2026-09-15, sur les flèches/texte qu'un plan annoté fait recopier
littéralement par ce type de modèle). Depuis le 2026-09-15ter, un second
workflow optionnel (mêmes tools sous-jacents `generer_grille_renovation` +
`generer_projection_renovation_inpainting`) permet un placement plus précis
par grille + masque géométrique, quand une simple description textuelle ne
suffit pas — cf. section dédiée du `SKILL.md`. Cf. `Comptallie_MCP/CLAUDE.md`,
section dédiée "Marchand de biens", pour le détail complet — y compris la
rupture avec le principe économique habituel de Comptallie (cette suite
déclenche une inférence tierce payante, fal.ai, sans garde-fou budgétaire
dans ce premier MVP) et la limite honnêtement assumée du placement des
objets (expérimental, jamais garanti précis, y compris avec le workflow par
grille : le masque contraint la zone, pas le rendu génératif à l'intérieur).

Il ne contient **aucune logique métier** : uniquement
`.claude-plugin/marketplace.json` et `adapters/claude_plugin/`
(`plugin.json`, `skills/<agent>/SKILL.md`, `.mcp.json` pointant vers le
serveur MCP distant déjà en ligne — le même que celui des autres suites,
jamais un second serveur dupliqué). Aucun connecteur fal.ai, aucune
logique de budget, aucune credential : tout ça vit exclusivement dans le
repo privé `Comptallie_MCP`.

## Structure

```
.claude-plugin/marketplace.json     ← déclare ce plugin
adapters/claude_plugin/
  .claude-plugin/plugin.json        ← métadonnées du plugin
  .mcp.json                         ← pointe vers le serveur MCP distant (Railway)
  skills/
    comptallie/SKILL.md             ← message d'accueil
    projection-renovation/SKILL.md  ← agent Projection de rénovation (/projection-renovation)
```

Quand un nouvel agent marchand de biens sera ajouté : créer son
`skills/<agent>/SKILL.md`, mettre à jour `comptallie/SKILL.md` pour le
lister, bumper `plugin.json`, et ajouter son id à l'entrée
`MARCHAND_DE_BIENS` de `core/catalog/suites.py` (repo privé).
