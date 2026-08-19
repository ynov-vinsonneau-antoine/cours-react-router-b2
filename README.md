# GameRank — Bonnes pratiques, Tailwind et routage

Cours 2h, React + TypeScript. 37 slides, 5 exercices machine, 2 quiz.

Fil rouge : **GameRank**, la tier list des jeux préférés des élèves.
Données locales dans un `games.ts` typé, aucun appel réseau.

## Lancer

```bash
npm install
npm run dev          # http://localhost:3030
```

## Fichiers

| Fichier | Rôle |
|---|---|
| `slides.md` | Le cours. Un `---` sépare deux slides. |
| `style.css` | Thème sombre violet/cyan. Chargé automatiquement par Slidev. |
| `NEXT-VS-REACT-ROUTER.md` | Mémo Next → React Router, **pour toi**. Jamais projeté. |

## Le plan

| | Partie | Slides | Durée |
|---|---|---|---|
| | Ouverture | 1–3 | 5 min |
| **0** | **On monte le projet** | 4–6 · **exo 7** | 17 min |
| 1 | Bonnes pratiques | 8–14 · **exo 15** | 28 min |
| 2 | Tailwind CSS | 16–20 · **exo 21** | 27 min |
| | Pause | 22 | 10 min |
| 3 | Routage et navigation | 23–34 · **exos 27 et 35** | 60 min |
| | Récap et TP | 36–37 | 5 min |

Le projet se monte **avant** les bonnes pratiques : dès la minute 20, ils ont un
projet qui tourne et peuvent essayer chaque règle en direct. Et si quelqu'un a un
souci de Node ou de `npm`, tu le découvres tôt, pas à mi-parcours.

### Détail

| # | Slide |
|---|---|
| 5 | **Structurer son projet** — l'arborescence, c'est le plan de l'exo 0 |
| 6 | Le setup Vite + TS + Tailwind |
| 9 | Pourquoi des règles |
| 10 | 1 · Un composant fait une seule chose |
| 11 | 2 · Un composant annonce ce qu'il reçoit |
| 12 | 2 · **Les props en pratique** — déclarer, typer, utiliser |
| 13 | 3 · Une liste a besoin d'étiquettes qui ne bougent pas |
| 14 | Quiz · combien d'erreurs ? |
| 17–20 | CSS classique · les deux versions · comment ça marche · responsive |
| 24 | Le problème · l'URL est un état |
| 25 | SPA vs site classique + **démo devtools** |
| 26 | Les trois briques |
| 28 | `Link` et `NavLink` |
| 29–30 | Le menu dupliqué → `Layout` + `Outlet` |
| 31–32 | Route `:slug` puis `useParams` |
| 33 | `useNavigate` et la route 404 |
| 34 | Quiz · page blanche, pourquoi ? |
| 37 | **Le sujet du TP** |

## Les cinq exercices

| Slide | Exercice | Durée | Livrable |
|---|---|---|---|
| 7 | **Exo 0** — Créer le projet | 10 min | Vite + TS + Tailwind, les 5 dossiers, `npm run dev` qui tourne |
| 15 | **Exo 1** — Les données et la carte | 10 min | Le type `Game`, `games.ts` rempli avec les 6 jeux imposés, un `GameCard` typé au bon endroit |
| 21 | **Exo 2** — Reproduire la liste en Tailwind | 12 min | Les cartes affichées sur la slide, en Tailwind seul |
| 27 | **Exo 3** — Trois pages, trois routes | 8 min | Les 3 URL répondent |
| 35 | **Exo 4** — Le layout et la page détail | 12 min | `Outlet`, routes imbriquées, `/jeux/:slug`, 404 |

L'exo 0 est de la plomberie pure. L'exo 1 est celui qui **applique les trois règles** —
c'est là qu'il faut circuler et poser des questions plutôt que corriger.

La **solution de l'exercice 2** est en note présentateur sur la slide 21, avec les
3 classes à écrire au tableau (`items-center`, `flex-col gap-3`, `hover:`).

## Le rythme des bonnes pratiques

Les slides 10, 11 et 13 suivent toutes le même schéma :

> **code cassé affiché → « qu'est-ce qui cloche ? » → ils cherchent à voix haute → clic → la règle**

Le code cassé **disparaît** au moment du reveal, pour laisser la place à la version juste.
Attends vraiment 20 secondes avant de cliquer, même si c'est silencieux.

La slide 12 est l'exception : c'est la seule slide « démonstration » de la partie 1.
Elle complète la règle 2 en montrant le **côté appelant**
(`<GameCard titre="Hades" note={9.5} />`), que la slide 11 ne montre jamais.

## Le TP — slide 37

Sujet retenu : **le carnet de recettes**, un projet neuf, refait de A à Z, pour
qu'ils recâblent eux-mêmes le router et le layout au lieu de prolonger GameRank.

- une page liste, une page détail en `/recettes/:slug`, une page de regroupement, une 404
- `Layout` + `Outlet` + nav en `NavLink`, le tout en Tailwind
- style libre, librairies autorisées sauf les kits de composants tout faits
- l'IA pour **le contenu**, jamais pour le code

**La slide 37 présente encore l'ancienne version du TP (GameRank v2). Elle est à
réécrire en carnet de recettes.**

## Timing

À plein régime le deck fait **~145 min** pour un créneau de 120. C'est assumé :
tu accélères en direct là où le groupe suit, et tu t'attardes où ça coince.

Les leviers, du plus rentable au moins :

1. **Donner l'exo 0 en amont** — « arrivez avec un projet Vite + Tailwind créé ».
   Tu vérifies l'arborescence en 3 min au lieu de 10. → **−7 min**
2. **Slide 6** (setup) au rythme rapide, ils n'ont rien à noter. → **−2 min**
3. **Exo 4** à 10 min : le TP le reprend de toute façon. → **−2 min**

**À ne jamais couper :** la démo devtools (slide 25), les slides 5 et 12, et l'exercice 4.

## Notes présentateur

Les commentaires HTML dans `slides.md` sont tes notes. Elles contiennent, pour chaque
exercice, les erreurs précises à guetter dans les rangs.

`http://localhost:3030/presenter` sur ton écran, `http://localhost:3030` sur le vidéoprojecteur.

## Raccourcis

| Touche | Action |
|---|---|
| Espace / → | Slide ou reveal suivant |
| ← | Retour |
| `f` | Plein écran |
| `o` | Vue d'ensemble |
| `g` | Aller à une slide par son numéro |

## Export

`style.css` se termine par un bloc `@media print` qui neutralise deux effets à
l'export uniquement — **la présentation à l'écran n'est pas touchée** :

- les **titres en dégradé** (`background-clip: text`), qui sortaient en barres
  blanches pleines dans certains lecteurs PDF, notamment Preview.app sur macOS
- le **`backdrop-filter: blur()`** des encadrés, mal rasterisé en PDF

Si tu ajoutes un effet CSS reposant sur du masquage ou de la transparence avancée,
pense à le neutraliser dans ce bloc aussi.

`playwright-chromium` est déjà installé, donc les deux commandes marchent directement :

```bash
npm run export
npx slidev export --format png --with-clicks --output .shots
```

La seconde sort **une image par état de clic**. C'est ce qui sert à vérifier
qu'aucune slide ne déborde — refais-le après une grosse modification.

### Mettre le deck en ligne

`npm run build` produit un site statique dans `dist/`, à déposer sur Netlify,
Vercel ou Cloudflare Pages. Pour GitHub Pages il faut préciser le sous-dossier,
sinon page blanche : `npm run build -- --base /nom-du-repo/`.

**Attention** : les notes présentateur sont **incluses** dans le site publié.
N'importe qui peut ouvrir `/presenter` et lire les solutions des exercices et des
quiz. Si tu publies pour les élèves, retire d'abord les commentaires `<!-- -->`
de `slides.md`.

## Éditer slides.md

- **Convention de code du deck** : les composants sont écrits en fonctions fléchées
  (`const HomePage = () => { ... }`), jamais `function HomePage()`. C'est ce que
  génère `rafce` et c'est la façon dont tu codes en direct — ne pas mélanger les deux formes.
- `<v-clicks>` révèle une liste élément par élément, `v-click` révèle un `<div>`
- une fence de code avec `{all|1|2-3}` révèle le code progressivement au fil des clics
- Classes maison : `.box` + `.bad` / `.good` / `.rule` / `.trap` / `.info`,
  `.tag`, `.timer`, `.mock`, `.tier`, `.flow`, `.demo-card`
- `class: code-xs` en frontmatter de slide réduit la taille du code (slides à deux colonnes)

### Cinq pièges à ne pas réintroduire

**1. Ne jamais imbriquer un clic dans un clic.** Un `<div v-click>` qui contient un
`<v-clicks>` ou un autre `v-click` **casse la numérotation** : Slidev donne aux enfants
un index plus petit qu'au parent, donc le parent reste invisible pendant que ses enfants
« passent » leur tour. Résultat : des clics morts, puis tout apparaît d'un coup.
La parade est d'aplatir avec des index explicites — un `v-click="1"` sur le bloc parent,
puis un `<v-clicks at="2">` pour la liste.

**2. Les seuils de `v-if="$clicks < n"`.** Six slides (10, 11, 13, 24, 26, 30) font
disparaître leur premier bloc de code au reveal, via un seuil écrit en dur.
Si tu **ajoutes un clic** sur une de ces slides, mets le seuil à jour, sinon le code
disparaît au mauvais moment.

**3. Les numéros de ligne d'une surbrillance comptent les lignes vides.**
Deux bugs sont déjà passés comme ça : une surbrillance sur une ligne vide, une autre
sur une accolade fermante. Compte les lignes avant de valider.

**4. Pas d'accolades nues hors backticks.** `mdc: true` est activé : un titre contenant
`key={index}` est lu comme un bloc d'attributs. Mets-le entre backticks.

**5. Une ligne vide** avant et après tout markdown placé dans un `<div>`.

### Une slide pleine ne prévient pas

Un élément ajouté en bas d'une slide déjà remplie **ne s'affiche pas du tout** —
il part hors écran, sans erreur ni warning. C'est arrivé en voulant ajouter un
encadré à la slide 12. Après tout ajout, exporte en PNG et regarde le dernier
état de clic.

### Synchroniser une surbrillance de code avec son explication

La slide 26 est le modèle. Les étapes du bloc de code et les puces partagent le même
compteur de clics : la fence `{all|5|6|7-8}` prend les clics 1, 2 et 3, donc les trois
puces portent `v-click="1"`, `v-click="2"` et `v-click="3"`. Chaque clic surligne une
ligne **et** fait apparaître son explication.

Sans ça, il faut cliquer à travers toutes les surbrillances avant de voir le moindre texte.

Après modification : `npx slidev build` pour valider que tout compile, puis un export
PNG si tu as touché à la mise en page.
