---
name: kroma-seo-geo
description: Recherche de sujets, rédaction, maillage interne, publication et optimisation de contenu pour kroma-art.com — vise le SEO classique (Google) et le GEO (réponses ChatGPT/Perplexity/Claude). Toujours ancré dans KROMA-FACTS.md, ne publie jamais sans validation explicite.
---

# Agent SEO/GEO — KROMA

Ce skill couvre les 4 fonctions demandées par Laurent : **recherche de sujets**, **rédaction**,
**maillage interne**, **publication**, plus un volet **optimisation** continue. Il est déclenché
à la demande (pas de cadence automatique) — voir `/loop` ou `schedule` séparément si un jour on veut
de la récurrence.

## Règle absolue — lire avant toute action

1. **Toujours lire `KROMA-FACTS.md` à la racine du repo en premier.** C'est la seule source de
   vérité factuelle sur KROMA (procédé, prix, presse, positionnement, garde-fous). Ce fichier est
   volontairement exclu de git (repo public, contenu sensible) — ne jamais tenter de le committer,
   ne jamais copier son contenu brut dans une page publique sans validation explicite de Laurent
   sur ce qui peut être rendu public.
2. **Ne jamais inventer un fait.** Si une information nécessaire n'est ni dans `KROMA-FACTS.md`
   ni déjà publiée sur le site, s'arrêter et demander à Laurent plutôt que d'improviser. C'est la
   contrainte la plus importante de ce skill — une marque qui publie une fausse affirmation sur la
   durabilité ou la fabrication de son propre produit a plus à perdre qu'à gagner d'un article de blog.
3. **Ne jamais pousser sur `main` sans confirmation explicite**, comme pour tout le reste du travail
   sur ce repo. Commit local possible ; `git push` seulement après un "oui" clair de Laurent.

## Contexte du site (relire si le skill n'a pas tourné depuis longtemps)

- Site statique HTML, pas de CMS. FR à la racine (`index.html`, `mar.html`, `solar.html`,
  `tierra.html`, `mentions-legales.html`, etc.), EN sous `en/` avec les mêmes noms de fichiers.
- Chemins d'assets en `/absolu` (fonctionnent identiquement à la racine et sous `en/`).
- Voix de marque : éditorial, "quiet luxury", phrases courtes et déclaratives, zéro jargon
  marketing ("streamline", "seamless", etc.), pas de tirets cadratins doubles en logique
  marketing creuse — cohérent avec le ton déjà utilisé sur `index.html`.
- `sitemap.xml` liste toutes les pages indexables avec hreflang réciproque fr/en — toute nouvelle
  page doit y être ajoutée (les deux versions).
- Schema.org : les œuvres sont typées `VisualArtwork` (pas `Product` — voir commit `75a1d91`,
  Search Console avait flaggé `Product` comme invalide sans prix/avis public). Réutiliser ce
  pattern pour tout nouveau contenu produit-adjacent.
- FAQPage (`#faq` sur la home, FR+EN) est le pattern de référence pour du contenu GEO : questions
  factuelles courtes, réponse en une ou deux phrases, reflet exact entre le HTML visible et le
  JSON-LD.

## Phase 1 — Recherche de sujets

Objectif : trouver des angles de contenu qui comblent un vrai manque, pas des sujets génériques.

- Si Search Console est accessible (Chrome connecté), regarder l'onglet Requêtes /
  Performances : quelles requêtes ont des impressions mais peu/pas de clics, et pour lesquelles
  aucune page dédiée n'existe encore ? C'est le signal le plus fort.
- Croiser avec `KROMA-FACTS.md` : quels faits vérifiés n'ont pas encore leur page ou paragraphe
  dédié ? (ex. le procédé de fabrication détaillé, l'histoire Mira Visual Creations, un
  comparatif face aux alternatives listées dans le fichier).
- Respecter les priorités de segment du fichier de faits (terrasses/rooftops/piscines en
  priorité ; galeries/hôtellerie à moyen terme ; yachts/chalets hors scope tant que non validés).
- Présenter 3 à 5 sujets à Laurent avec, pour chacun : l'angle, la requête ciblée, et pourquoi ce
  sujet comble un vrai manque (pas juste "c'est un bon sujet"). Ne pas rédiger avant validation
  du sujet.

## Phase 2 — Rédaction

- Un fait = soit dans `KROMA-FACTS.md`, soit déjà publié sur le site. Rien d'autre.
- Respecter les formulations imposées mot pour mot quand `KROMA-FACTS.md` en impose une (le
  fichier liste explicitement des formulations à utiliser et des formulations à proscrire —
  s'y référer à chaque rédaction, ne pas travailler de mémoire).
- Certains faits du fichier sont marqués "non public" ou nécessitent une validation avant
  publication (prix, nom de partenaires, dates). Ne jamais les faire passer dans une page publique
  sans validation explicite préalable de Laurent pour CETTE publication précise — l'autorisation
  donnée pour constituer le fichier de faits n'est pas une autorisation permanente de publication.
- Longueur et structure : suivre le gabarit existant (voir Phase 4) plutôt que d'improviser une
  nouvelle mise en page à chaque fois.
- Rédiger en FR et EN si le contenu est destiné à devenir une page publique (cohérence avec le
  reste du site, qui est intégralement bilingue).

## Phase 3 — Maillage interne

- Toute nouvelle page doit être reliée depuis au moins un point d'entrée existant pertinent
  (section Collection, bloc Contact, FAQ, ou une autre page produit) — voir comment `mar.html`
  est relié depuis les cartes de `#collection` sur `index.html` (lien "En savoir plus").
- Toute nouvelle page doit elle-même pointer vers `/#contact` (ou `/en/#contact`) et vers au
  moins une page produit ou section existante pertinente.
- Vérifier qu'aucun lien cassé n'est introduit (chemins relatifs vs absolus, `/en/` correct côté
  anglais).

## Phase 4 — Publication

Gabarit de référence pour une nouvelle page produit/contenu : **`mar.html`** (FR) et
**`en/mar.html`** (EN) — copier leur structure `<head>` (meta, canonical, hreflang réciproque,
OG/Twitter, JSON-LD `VisualArtwork` + `BreadcrumbList`), leur nav (`.legal-nav`), leur footer, et
leur script de reveal (`fade-up` + `IntersectionObserver`).

Checklist avant de committer une nouvelle page :
- [ ] `<title>`, meta description, canonical, hreflang (fr/en/x-default) uniques et corrects
- [ ] JSON-LD valide (vérifier avec un parseur JSON, pas juste à l'œil — voir les scripts Python
      utilisés lors de la création de `mar.html`/`solar.html`/`tierra.html` pour l'approche)
- [ ] Ajoutée à `sitemap.xml` (FR + EN, hreflang réciproque)
- [ ] Liée depuis et vers le reste du site (Phase 3)
- [ ] Testée en local (`preview_start` avec la config `kroma-site`, vérifier absence d'erreur
      console, rendu mobile)
- [ ] **Jamais de `git push` sans validation explicite de Laurent** — `git add` + `git commit`
      possibles pour préparer, mais le push est toujours une décision de Laurent.

## Phase 5 — Optimisation continue

- Après publication (et une fois Google ayant eu le temps de crawler, plusieurs jours), vérifier
  dans Search Console si la page capte des impressions sur les requêtes visées.
- Pour le GEO spécifiquement : de temps en temps, tester dans ChatGPT/Perplexity/Claude des
  questions du type "art mural aluminium extérieur" ou "décoration piscine intérieure résistante
  humidité" pour voir si KROMA apparaît, et si la formulation du site (FAQ, JSON-LD) est assez
  factuelle et citable. Ajuster le contenu existant plutôt que d'empiler de nouvelles pages si un
  format (ex. FAQ) sous-performe.
- Ne pas retirer ou dupliquer du contenu déjà indexé sans raison claire (risque de perdre le peu
  de signal déjà accumulé, cf. le faible trafic actuel du site).
