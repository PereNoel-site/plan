# Handoff : Plan au Naturel — site vitrine (PN Production)

## Overview
Site vitrine one-page pour **Plan au Naturel / PN Production**, un studio français qui dessine des plans illustrés de campings, villages vacances, aires de camping-car et parcs de loisirs. Objectif du site : présenter les réalisations, expliquer les prestations, afficher une tarification transparente à l'emplacement (avec un simulateur de prix interactif) et capter des demandes de devis.

## About the Design Files
Les fichiers de ce paquet sont des **références de design réalisées en HTML** — des prototypes qui montrent l'apparence et le comportement attendus, **pas du code de production à copier tel quel**.

La tâche est de **recréer ces designs HTML dans l'environnement du codebase cible** (React/Next, Vue, Astro, Svelte…) en utilisant ses patterns et librairies établis. S'il n'existe pas encore de codebase, choisir le framework le plus adapté (pour un site vitrine de ce type : Astro ou Next.js en statique) et y implémenter les designs.

Deux fichiers sont fournis :
- `Plan au Naturel.dc.html` — la **source de vérité** du design. Markup + styles inline + une classe de logique JS en fin de fichier. C'est ici qu'on lit les valeurs exactes.
- `plan-au-naturel-standalone.html` — build autonome (assets et images inlinés en base64). Utile pour **voir le rendu** dans un navigateur sans rien installer. Ne pas lire ce fichier comme source : il est compilé et énorme.

Note technique sur la source : le HTML utilise une syntaxe de templating légère — `{{ valeur }}` pour les valeurs calculées, `<sc-for list="{{ items }}" as="item">` pour les boucles, `<sc-if value="{{ flag }}">` pour le conditionnel. À la réimplémentation, ce sont respectivement des interpolations, des `.map()` et des rendus conditionnels. Le bloc `<helmet>` en tête contient les `@font-face`/liens de polices, les variables CSS et les resets — tout le reste du style est inline.

## Fidelity
**High-fidelity.** Couleurs, typographies, espacements, rayons et comportements sont définitifs. Recréer l'UI fidèlement au pixel avec les librairies du codebase. Le contenu textuel français est du copy réel et validé : le reprendre mot pour mot.

## Screens / Views
Le site est une **page unique** avec ancres de navigation. Conteneur standard : `max-width: 1160px; margin: 0 auto; padding: 0 24px` (la section FAQ est plus étroite : `max-width: 900px`). Rythme vertical des sections : `padding: 72px` haut/bas.

### 1. Header (sticky)
- **Purpose** : navigation permanente + CTA devis.
- **Layout** : `position: sticky; top: 0; z-index: 40`. Fond semi-transparent `color-mix(in srgb, var(--color-bg) 92%, transparent)` + `backdrop-filter: blur(10px)`, `border-bottom: 1px solid var(--color-divider)`. Intérieur : flex, `align-items: center`, `gap: 24px`, `flex-wrap: wrap`, `padding: 14px 24px`.
- **Important** : le conteneur racine de la page doit être en `overflow-x: clip` — un `overflow-x: hidden` casse l'ancrage sticky. Le header ne doit **jamais** disparaître au scroll (pas de hide-on-scroll).
- **Components** :
  - Logo : image `height: 30px`, + wordmark « Plan Naturel » en `--font-heading`, `19px`, `letter-spacing: -0.02em`. `margin-right: auto` pour pousser le reste à droite.
  - Nav : flex, `gap: 22px`, `font-size: 14px`, `font-weight: 600`, couleur `--color-text`, `text-decoration: none`. Liens : Réalisations (`#realisations`), Prestations (`#prestations`), Tarifs (`#tarifs`), Méthode (`#methode`), FAQ (`#faq`).
  - CTA : bouton primaire pill (`border-radius: 999px`) « Demander un devis » → `#devis`.

### 2. Hero (`#top`)
- **Layout** : grid 2 colonnes `minmax(0, 0.95fr) minmax(0, 1.05fr)`, `gap: 48px`, `align-items: center`, `padding: 64px 24px 40px`. Sous ~900px : passer en une colonne, visuel après le texte.
- **Components** :
  - Tag pill vert « Plan de camping sur mesure · France », `margin-bottom: 18px`.
  - H1 (`--font-heading`, poids 500, `letter-spacing: -0.01em`), taille fluide.
  - Paragraphe `font-size: 18px`, `max-width: 46ch`, couleur `--color-neutral-800`.
  - Deux boutons pill côte à côte (flex, `gap: 12px`, `flex-wrap: wrap`, `margin-top: 28px`, `padding: 13px 26px`, `font-size: 15px`) : primaire « Demander un devis gratuit » → `#devis`, secondaire « Voir des plans réalisés » → `#realisations`.
  - Mention de réassurance : `margin-top: 26px`, `font-size: 14px`, `--color-neutral-700`, « À partir de **150 €** pour un camping jusqu'à 49 emplacements. »
  - Colonne droite : visuel d'un plan réalisé, coins `--radius-lg`.

### 3. Réalisations (`#realisations`)
- **Purpose** : preuve par les plans livrés, agrandissables.
- **Layout** : `padding: 24px 24px 72px`. Eyebrow H6 couleur `--color-accent-700` + H2 `clamp(30px, 3.4vw, 42px)`, `max-width: 22ch`. Sous-titre `max-width: 56ch`, `margin-bottom: 32px` : « Cliquez sur un plan pour l'agrandir. »
- **Grid** : `repeat(auto-fit, minmax(280px, 1fr))`, `gap: 22px`. Chaque carte = vignette d'un plan, cliquable.
- **Interaction** : clic → ouverture d'une **lightbox** plein écran (overlay sombre, image centrée, fermeture au clic hors image et à Échap). Dans la source, c'est le `<sc-if>` qui suit la section.

### 4. Prestations (`#prestations`)
- **Layout** : section pleine largeur avec fond `--color-accent-2-100` (`#eef1e7`), `padding: 72px 0`, contenu dans le conteneur 1160px.
- Eyebrow H6 en `--color-accent-2-700`, H2 `clamp(30px, 3.4vw, 42px)`, `max-width: 24ch`.
- **Grid** de cartes : `repeat(auto-fit, minmax(260px, 1fr))`, `gap: 22px`, `margin-top: 36px`. Cartes blanches, `border-radius: var(--radius-lg)`, ombre légère (élévation sm).

### 5. Tarifs + simulateur (`#tarifs`)
- **Layout** : conteneur 1160px, `padding: 72px 24px`. Eyebrow + H2 `max-width: 24ch` + paragraphe d'explication `max-width: 58ch`.
- **Modèle tarifaire** : forfait de base puis **paliers progressifs** — chaque tranche d'emplacements est facturée à son propre tarif (logique par tranches, pas un prix unique appliqué au total). Les tranches et montants exacts sont dans la classe de logique en fin de `Plan au Naturel.dc.html` — les reprendre de là, ne pas les réinventer.
- **Simulateur** : l'utilisateur saisit un nombre d'emplacements (état `n`, défaut **120**) et coche des options (ex. légende). Le total se recalcule en direct.
- **Affichage du total** : `--font-heading`, `font-size: 38px`, `line-height: 1`, couleur `--color-accent-700`. Formatage monétaire : `toLocaleString('fr-FR') + ' €'` (espace insécable avant €, séparateur de milliers français).
- CTA pleine largeur sous le total : pill, `margin-top: 22px`, `padding: 13px`, « Faire chiffrer précisément » → `#devis`.

### 6. Méthode (`#methode`)
- Fond `--color-surface`, `padding: 72px 0`. H2 `max-width: 26ch` : « Quatre étapes, du premier appel au fichier prêt à imprimer ».
- **Grid** : `repeat(auto-fit, minmax(220px, 1fr))`, `gap: 26px`, `margin-top: 40px` — 4 étapes numérotées.

### 7. Avant / après
- Conteneur 1160px, `padding: 72px 24px`. H2 `max-width: 26ch`.
- **Grid** : `repeat(auto-fit, minmax(300px, 1fr))`, `gap: 26px`, `margin-top: 34px`. Deux `<figure>` comparatives avec légendes.

### 8. FAQ (`#faq`)
- Conteneur étroit `max-width: 900px`, `padding: 72px 24px`.
- Liste d'accordéons : flex colonne, `gap: 12px`, `margin-top: 32px`. Chaque item = `<details>` natif, fond `--color-surface`, `border-radius: var(--radius-lg)`, `padding: 20px 26px`. Reprendre le comportement natif (un seul élément ouvrable à la fois n'est pas requis).

### 9. Devis (`#devis`)
- **Layout** : `padding: 0 24px 80px`. Bloc intérieur foncé : fond **#2f4038** (vert forêt bleuté), texte **#fbf9f5**, `border-radius: var(--radius-lg)`, `padding: 56px`, grid `repeat(auto-fit, minmax(300px, 1fr))`, `gap: 44px`.
- Colonne gauche : H2 `clamp(30px, 3.2vw, 42px)` en `#fbf9f5`, `max-width: 20ch` « Demandez votre devis » ; paragraphe `rgba(255,255,255,0.78)`, `max-width: 40ch` ; bloc de coordonnées (flex colonne, `gap: 10px`, `font-size: 15px`), mentions secondaires en `rgba(255,255,255,0.68)`.
- Colonne droite : formulaire de contact (nom, établissement, email, téléphone, nombre d'emplacements, message) + bouton d'envoi.
- **À câbler côté dev** : la soumission n'est pas implémentée dans le prototype. Brancher sur l'endpoint / service d'email du codebase, avec états *loading*, *succès* et *erreur*, et validation (email valide, champs requis, nombre d'emplacements numérique).
- **Variantes de couleur validées** pour ce bloc (si besoin d'alternative) : bleu encre `#2b3947`, terre cuite foncée `#5b3a2c`, brun taupe `#36322b`.

### 10. À propos
- Grid deux colonnes : texte à gauche, photo d'équipe à droite.
- Eyebrow H6 « À propos », H2 `clamp(28px, 3vw, 38px)`, `max-width: 20ch` « Une petite équipe, basée en France ».
- Trois paragraphes, `--color-neutral-800`, `max-width: 52ch` : (1) équipe de passionnés / mission de valorisation des espaces, (2) clientèle — campings indépendants et groupes, villages vacances, aires de camping-car, parcs de loisirs, collectivités, (3) « Diplômés en gestion de projet digital, nous allons aussi au-delà du plan : création de sites internet, référencement local et accompagnement sur vos outils de communication. »
- Photo : `<figure>` flex colonne `gap: 12px`, image `width: 100%`, `aspect-ratio: 4 / 3`, `object-fit: cover`, `border-radius: var(--radius-lg)`. Légende `font-size: 14px`, `--color-neutral-700` : « Pierre-Nicolas et Amandine — PN Production ».

### 11. Footer
Logo, liens de navigation répétés, mentions légales.

## Interactions & Behavior
- **Navigation par ancres** : tous les liens de nav et CTA pointent vers des `id` de section. Activer `scroll-behavior: smooth` et prévoir un `scroll-margin-top` sur les sections cibles égal à la hauteur du header sticky (~59px), sinon les titres passent sous la barre.
- **Header sticky permanent** — jamais masqué au scroll. Rappel : `overflow-x: clip` (et non `hidden`) sur l'ancêtre.
- **Lightbox réalisations** : ouverture au clic sur une vignette, fermeture au clic sur l'overlay et à Échap. Bloquer le scroll du body pendant l'ouverture, restaurer le focus au bouton d'origine à la fermeture.
- **Simulateur de tarif** : recalcul synchrone à chaque frappe / changement de case. Borner la saisie à des entiers positifs et gérer le champ vide sans afficher `NaN`.
- **Accordéons FAQ** : `<details>`/`<summary>` natifs, transition d'ouverture douce.
- **Formulaire devis** : validation + états loading/succès/erreur à implémenter.
- **Responsive** : toutes les grilles sont en `auto-fit`/`minmax` et se replient seules. Les deux grids explicitement à 2 colonnes (hero, à propos) doivent passer en une colonne sous ~900px. La nav du header a besoin d'un traitement mobile (menu compact ou repli en `flex-wrap`, déjà autorisé).
- **Hover** : boutons et cartes ont des états hover subtils (assombrissement du fond pour les boutons, légère montée d'ombre pour les cartes) — valeurs exactes dans les `style-hover` de la source.

## State Management
État local à la page, aucun besoin de store global :
- `n` (number, défaut **120**) — nombre d'emplacements du simulateur.
- `legende` (boolean) — option de légende dans le simulateur.
- `total` (dérivé) — recalculé à partir de `n` et des options via la logique de paliers ; formaté `fr-FR`.
- image ouverte dans la lightbox (référence ou `null`).
- état du formulaire de devis : valeurs des champs, `submitting`, `error`, `success`.
Aucun data fetching côté lecture : le contenu est statique. Seule requête sortante : la soumission du formulaire de devis.

## Design Tokens

### Typographie
- `--font-heading: "Newsreader", Georgia, serif` — poids **500**, `letter-spacing: -0.01em` sur h1–h5.
- `--font-body: "Karla", system-ui, sans-serif`.
- Échelle des titres de section : `clamp(30px, 3.4vw, 42px)` ; titres secondaires `clamp(28px, 3vw, 38px)` ; total du simulateur `38px`.
- Corps : `18px` (hero), `15–16px` (courant), `14px` (mentions, légendes, nav).
- Eyebrows (h6) : petites capitales/labels en `--color-accent-700` (ou `--color-accent-2-700` sur fond vert).

### Couleurs de base
| Token | Valeur |
| --- | --- |
| `--color-bg` | `#faf7f2` |
| `--color-surface` | `#f3efe7` |
| `--color-text` | `#3b362e` |
| `--color-divider` | `color-mix(in srgb, #3b362e 11%, transparent)` |

### Neutres
`100 #f4ece1` · `200 #ebe2d5` · `300 #ded4c5` · `400 #c8bfb1` · `500 #a8a092` · `600 #8d8578` · `700 #766f63` · `800 #5d5549` · `900 #443e35`

### Accent 1 — terre cuite (`--color-accent: #b37c55`)
`100 #f7ece3` · `200 #efdccd` · `300 #e2c5ad` · `400 #cfa47f` · `500 #bd8b64` · `600 #a5734d` · `700 #8a5e3e` · `800 #6e4a31` · `900 #513626`

### Accent 2 — vert sauge (`--color-accent-2: #869176`)
`100 #eef1e7` · `200 #e2e6d8` · `300 #d0d6c2` · `400 #b3bba3` · `500 #97a087` · `600 #7d876d` · `700 #656e57` · `800 #4e5644` · `900 #3a4033`

### Couleurs hors échelle
- Bloc devis : fond `#2f4038`, texte `#fbf9f5`, textes secondaires `rgba(255,255,255,0.78)` et `rgba(255,255,255,0.68)`.
- `theme-color` / favicon : `#faf7f2`.

### Espacement
Pas de barème formel mais un usage cohérent : `10 / 12 / 14 / 18 / 22 / 24 / 26 / 28 / 32 / 36 / 40 / 44 / 48 / 56 / 64 / 72 / 80` px. Gap de grille : `22px` (cartes), `26px` (méthode, avant/après), `44px` (devis), `48px` (hero). Padding vertical de section : `72px`.

### Rayons
- `--radius-lg` — cartes, blocs, images, accordéons.
- `999px` — tous les boutons (pill).

### Ombres
Élévations légères sur les cartes (classe `elev-sm` dans la source). Pas d'ombres portées marquées : le design repose sur les fonds crème et les bordures fines.

### Largeurs de mesure
Contrainte de lisibilité systématique en `ch` : titres `20–26ch`, paragraphes `40–58ch`. À conserver.

## Assets
Tous les assets sont dans le dossier `assets/` de ce paquet. Ils sont **déjà inlinés** dans le build autonome, mais à servir normalement dans le codebase cible (passer les images par le pipeline d'optimisation du framework — formats modernes + `srcset`).
- **Logo** — `firefly_gemini-flash_sans-feu-208138-mu1jw5j1-rm69.png`, généré pour le projet. Utilisé dans le header (30px de haut) et le footer.
- **Favicons** — `favicon-32.png`, `apple-touch-icon.png` (180px), `favicon-512.png`. Recadrés depuis le logo sur fond `#faf7f2`.
- **Photo d'équipe** — Pierre-Nicolas et Amandine, fournie par le client. Cadrage 4/3, `object-fit: cover`.
- **Plans de camping** — captures des réalisations, utilisées en vignettes dans « Réalisations » et en comparatif « Avant / après ». Ce sont des travaux clients : vérifier les droits de diffusion avant mise en ligne.
- **Polices** — Newsreader et Karla, via Google Fonts (liens dans le `<helmet>`). Préférer un self-hosting avec `font-display: swap` en production.

## Files
- `Plan au Naturel.dc.html` — **source de vérité** du design (markup, styles inline, classe de logique en fin de fichier : calcul du tarif, état du simulateur, lightbox).
- `plan-au-naturel-standalone.html` — build autonome pour visualiser le rendu dans un navigateur (assets en base64, ne pas éditer).
- `assets/` — logo, favicons, photo d'équipe, plans.

## Notes d'implémentation
- Le site est **statique** : privilégier un rendu statique (SSG) pour le SEO. Le référencement local est un argument commercial du studio lui-même — soigner `<title>`, meta description, Open Graph, données structurées `LocalBusiness`, `lang="fr"`, `theme-color: #faf7f2`, et les `alt` de toutes les images.
- Tout le copy est en **français** : conserver les accents et la typographie française (espace insécable avant `€`, `:`, `?`, `!`).
- Accessibilité : contraste OK sur les fonds crème ; vérifier les états focus visibles sur les boutons pill et les liens de nav, le piégeage du focus dans la lightbox, et les labels du formulaire de devis.
