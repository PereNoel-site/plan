# Plan au Naturel — site vitrine

Site vitrine one-page pour **Plan au Naturel / PN Production**, studio français qui dessine des plans illustrés
de campings, villages vacances, aires de camping-car et parcs de loisirs.

En ligne sur **[pnproduction.fr](https://pnproduction.fr)**, publié automatiquement sur GitHub Pages à chaque
push sur `main` via [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml).

Implémenté en **[Astro](https://astro.build)** (rendu statique, SSG) à partir du design de référence livré dans
[`design-reference/`](./design-reference) — voir [`design-reference/README.md`](./design-reference/README.md)
pour le brief de handoff original.

## Stack

- **Astro** (statique) — pas de framework JS lourd, quelques `<script>` vanilla scopés par composant pour
  l'interactivité (lightbox, simulateur de tarif, formulaire de devis).
- Polices **Newsreader** et **Karla** via Google Fonts.
- Images passées par le pipeline d'optimisation d'Astro (`astro:assets`) : conversion WebP + dimensionnement.

## Développement

```bash
npm install
npm run dev
```

Le site est servi sur `http://localhost:4321`.

## Build

```bash
npm run build
npm run preview
```

La sortie statique est générée dans `dist/`.

## Structure

- `src/layouts/Layout.astro` — head (meta, SEO, Open Graph, `LocalBusiness` JSON-LD, polices, favicons).
- `src/components/` — une section de la page par composant (`Header`, `Hero`, `StatsStrip`, `Realisations`,
  `Prestations`, `Tarifs`, `Methode`, `AvantApres`, `Temoignages`, `Faq`, `Devis`, `About`, `Footer`).
- `src/styles/global.css` — design tokens (couleurs, typographie, espacement, rayons, ombres) et classes de
  composants (`.btn`, `.tag`, `.card`, `.table`, `.dialog`, `.field`/`.input`).
- `src/images/` — sources des images, optimisées à la volée par `astro:assets`.
- `design-reference/` — le paquet de handoff original (prototype HTML, captures, assets bruts) conservé pour
  référence et provenance des textes/valeurs.

## Points restant à câbler

- **Formulaire de devis** (`src/components/Devis.astro`) : validation et états loading/succès/erreur sont en
  place côté client, mais l'envoi réel n'est pas branché sur un service d'email — voir le commentaire `TODO(dev)`
  dans le script du composant.
- **Droits de diffusion** des plans clients utilisés en illustration (`Réalisations`, `Avant/Après`) à vérifier
  avant mise en ligne, comme indiqué dans le handoff.
- **Photo "avant"** de la section Avant/Après : le prototype utilisait un slot d'image à fournir par le client,
  actuellement un simple placeholder.
