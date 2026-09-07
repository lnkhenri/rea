# Outils et MCP

Deux usages distincts. Ne pas les mélanger.

1. **Build** — toi + l’agent, pour faire avancer REA.
2. **Produit** — ce que le Collecteur / Trieur / Rédacteur appellent une fois le système en place.

## Build (déjà tranché)

| Besoin | Outil |
|---|---|
| Code + vitrine | GitHub `lnkhenri/rea` |
| Tickets (bugs, évolutions) | GitHub Issues |
| Cadrage | `docs/cadrage/` (ce dossier) |
| Notes privées (candidat, candidatures, journal) | Obsidian, hors git |
| Secrets | `.env` local, jamais le repo |

Notion : seulement si, plus tard, un tableau de candidatures cloud manque vraiment. Pas pour les tickets logiciel.

## Produit — sources d’offres

Règle : **API officielle d’abord**. Un recruteur qui lit le repo doit voir un client d’API, pas un scraper LinkedIn.

| Source | Type | Pour REA | Pourquoi |
|---|---|---|---|
| **France Travail** (API *Offres v2* + ROME) | Officielle, FR | **Première source** | Légale, structurée (salaire, ROME, compétences), gratuite avec compte [francetravail.io](https://francetravail.io) |
| MCP `francetravail-mcp` | Wrapper de cette API | Utile en build *et* comme modèle de connecteur | [github.com/mohamed-amine-ben-mallessa/francetravail-mcp](https://github.com/mohamed-amine-ben-mallessa/francetravail-mcp) |
| Autres job boards (Welcome to the Jungle, APEC, etc.) | À voir | Plus tard, **seulement** s’il y a API / RSS / export | Pas de scraping « parce que ça marche » |
| LinkedIn / Indeed via scraper ou MCP scrape | ToS | **Hors vitrine** | Compte banni + mauvaise image recruteur |

Le Collecteur parle à un **connecteur** (interface unique). Derrière : France Travail d’abord, d’autres sources branchables ensuite.

## MCP publics — ce qui existe vs ce qu’on garde

Il y a beaucoup de MCP « LinkedIn jobs / people / messages ». Presque tous **scrapent** ou réutilisent ta session navigateur. Ça existe, ce n’est pas un feu vert.

| Idée | Verdict | Commentaire |
|---|---|---|
| France Travail MCP | **Oui** | API officielle. Premier à brancher quand on ouvrira la collecte. |
| Entreprises FR (SIRENE / Pappers) | **Oui, plus tard** | Enrichir une offre (taille, NAF, dirigeants) sans LinkedIn. |
| Browser-use (déjà là) | **Secours** | Page sans API, ponctuel, pas le cœur du Collecteur. |
| LinkedIn job/people MCP (guest, cookie, Playwright) | **Non pour le repo public** | Interdit par les CGU LinkedIn, même en « guest ». |
| JobSpy / Indeed scrape MCP | **Non** | Même problème, cassé souvent. |
| Envoi de messages LinkedIn via MCP | **Non** | Hors human-in-the-loop tel qu’on l’a défini. |
| Agrégateurs payants (TheirStack, etc.) | **Pas maintenant** | Coût + dépendance. Revoir si France Travail ne suffit pas. |
| Recherche de *personnes* / contacts | **Pas un MCP magique** | Pas de source people propre type LinkedIn. Entreprise via SIRENE ; contacts = toi + réseau, pas un scraper. |

**Contacts / entreprises / UX :**  
- Entreprise : registre officiel (SIRENE), pas LinkedIn scrape.  
- Personne : pas de MCP public propre. On ne cadre pas un « agent recruteur LinkedIn ».  
- UX du produit REA : IHM plus tard, pas un MCP.

## Ce qu’on ne décide pas ici

Langage UI, framework, base de données, quel LLM dans le Rédacteur. Ça se tranche à l’étape où on construit la pièce.

## Ordre d’outillage (quand on construira)

1. Compte API France Travail (gratuit).
2. Connecteur Collecteur → cette API (le MCP officiel peut servir de référence ou d’outil de test).
3. Enrichissement entreprise (SIRENE) si ça aide le Trieur.
4. Tout le reste : seulement si un trou concret apparaît.
