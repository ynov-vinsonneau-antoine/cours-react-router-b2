---
theme: default
title: GameRank — Bonnes pratiques, Tailwind et routage
info: |
  ## GameRank
  Bonnes pratiques React, Tailwind CSS, routage et navigation.
  Cours 2h — React + TypeScript
class: text-center
transition: slide-left
colorSchema: dark
highlighter: shiki
lineNumbers: false
mdc: true
fonts:
  sans: Outfit
  mono: JetBrains Mono
  weights: '300,400,500,600,700'
drawings:
  persist: false
---

<div class="tag tag-violet mb-6">React + TypeScript · 2 heures</div>

# GameRank

<p class="big">Bonnes pratiques, Tailwind<br/>et navigation entre les pages</p>

<div class="abs-br m-6 muted text-xs">
  <span class="kbd">→</span> pour avancer
</div>

<!--
Avant de parler : lancer l'app finie et cliquer dedans 30 secondes.
Ils doivent voir où on va avant de savoir comment on y va.
Aucune ligne de code sur cette slide.
-->

---
layout: center
---

# Le projet du jour

<div class="mock w-160 mt-6">
  <div class="mock-bar">
    <div class="mock-dot"></div><div class="mock-dot"></div><div class="mock-dot"></div>
    <div class="mock-url">localhost:5173<b>/tier-list</b></div>
  </div>
  <div class="mock-body">
    <div class="mock-nav">
      <span>Accueil</span><span class="on">Tier list</span><span>Catalogue</span><span>À propos</span>
    </div>
    <div class="tier tier-s">
      <div class="tier-badge">S</div>
      <div class="tier-games">
        <span class="chip">Hollow Knight</span><span class="chip">Hades</span><span class="chip">Elden Ring</span>
      </div>
    </div>
    <div class="tier tier-a">
      <div class="tier-badge">A</div>
      <div class="tier-games">
        <span class="chip">Celeste</span><span class="chip">Portal 2</span><span class="chip">Baldur's Gate 3</span>
      </div>
    </div>
    <div class="tier tier-b">
      <div class="tier-badge">B</div>
      <div class="tier-games">
        <span class="chip">Stardew Valley</span><span class="chip">Undertale</span>
      </div>
    </div>
  </div>
</div>

<div class="mt-6 muted">GameRank — la tier list de vos jeux préférés.<br/>On la construit ensemble, du composant jusqu'aux pages.</div>

---

# Le programme

<div class="grid grid-cols-2 gap-4 mt-6">

<div class="box" v-click>
<span class="box-label" style="color: var(--gr-cyan)">Partie 0 · 15 min</span>

### On monte le projet

Vite, TypeScript, Tailwind, et une arborescence propre. Vous codez dès la première demi-heure.

</div>

<div class="box" v-click>
<span class="box-label" style="color: var(--gr-violet)">Partie 1 · 30 min</span>

### Bonnes pratiques

Écrire un composant qu'on peut relire dans trois semaines.

</div>

<div class="box" v-click>
<span class="box-label" style="color: var(--gr-cyan)">Partie 2 · 25 min</span>

### Tailwind

Styler sans jamais chercher un nom de classe.

</div>

<div class="box" v-click>
<span class="box-label" style="color: var(--gr-pink)">Partie 3 · 45 min</span>

### Routage et navigation

Passer d'une page à cinq, sans recharger.

</div>

</div>

<div class="box info mt-5" v-click>
<span class="box-label">Un exercice après chaque partie</span>

On code. Vous repartez avec un projet qui tourne, pas avec des slides.
Et un mot de vocabulaire : on écrit du **TSX**, c'est la même syntaxe que le JSX
que vous connaissez, dans un fichier TypeScript.

</div>

<!--
On commence par monter le projet : s'il y a un souci de Node ou de npm,
je veux le découvrir maintenant, pas à la minute 40 avec toute la classe qui attend.
-->

---
layout: section
---

<div class="part-num">00</div>

# On monte le projet

<div class="muted mt-2">Avant de parler de code, on ouvre le chantier</div>
---

# Structurer son projet

<div class="grid grid-cols-2 gap-6">

<div>

```
src/
├── main.tsx
├── App.tsx
├── components/
│   ├── GameCard.tsx
│   └── TierRow.tsx
├── pages/
│   ├── HomePage.tsx
│   └── TierListPage.tsx
├── hooks/
├── types/
└── data/
    └── games.ts
```

</div>

<div>

<v-clicks>

- **`pages/`** un fichier par écran. C'est ce que l'utilisateur voit comme « une page ».
- **`components/`** tout ce qui est réutilisable. Ne sait rien des pages.
- **`hooks/`** la logique qu'on réutilise à plusieurs endroits.
- **`types/`** les types partagés entre plusieurs fichiers.
- **`data/`** les données. Plus tard, l'accès à l'API.

</v-clicks>

</div>

</div>

<div class="box rule mt-3" v-click>
<span class="box-label">La règle qui en découle</span>

**Les pages lisent les données. Les composants les reçoivent en props.**

Un composant par fichier, qui porte son nom, en `PascalCase`. Et si vous cherchez
un fichier plus de 5 secondes, l'arborescence est à revoir.

</div>

<!--
Insister lourdement ici. C'est ce qui leur servira le plus longtemps,
et c'est le plan de l'exercice qui suit : ils créent ces dossiers vides.

Question à poser : "une carte de jeu réutilisée sur deux écrans, elle va où ?"
Réponse : components/, parce que ce n'est pas un écran et qu'on la réutilise.
Le réflexe à leur donner : est-ce que l'utilisateur dirait "je suis sur cette page" ?
Si oui, pages/. Sinon, components/.

hooks/ et types/ resteront vides jusqu'à la partie 3, c'est normal.
On les crée maintenant pour ne plus y penser.
-->
---

# Le setup, en trois étapes

```bash
npm install tailwindcss @tailwindcss/vite
```

```ts
// vite.config.ts — un import et un plugin à AJOUTER
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],  // ← aux plugins déjà là
})
```

```css
/* src/index.css */
@import "tailwindcss";
```

<div class="box good mt-3" v-click>

C'est tout. Pas de fichier de configuration à remplir.
Installez l'extension **Tailwind CSS IntelliSense** dans VS Code : elle complète
les classes et affiche le CSS correspondant au survol.

</div>
---
class: code-xs
---

# <span class="tag tag-cyan mr-3">Exo 0</span>Créer le projet <span class="timer ml-3">⏱ 10 minutes</span>

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="tag tag-violet mb-1">1 · Le projet</div>

```bash
npm create vite@latest gamerank
```

| L'installeur demande | Vous répondez |
|---|---|
| Select a framework | **React** |
| Select a variant | **TypeScript + React Compiler** |
| Which linter to use? | **ESLint** |
| Install and start now? | **No** |

<div class="muted text-sm mt-2">Puis <code>cd gamerank</code> et <code>npm install</code>.</div>

</div>

<div>

<div class="tag tag-cyan mb-1">2 · Tailwind</div>

```bash
npm install tailwindcss @tailwindcss/vite
```

```ts
// vite.config.ts — une ligne à ajouter
import tailwindcss from '@tailwindcss/vite'
plugins: [react(), babel(…), tailwindcss()],
```

```css
/* src/index.css — tout en haut */
@import "tailwindcss";
```

<div class="tag tag-amber mb-1 mt-2">3 · Le ménage</div>

<div class="muted text-sm">Videz <code>index.css</code> sauf l'<code>@import</code>, <b>supprimez</b> <code>App.css</code>, videz <code>App.tsx</code>.</div>

</div>

</div>

<div class="box good mt-1">
<span class="box-label">4 · Les dossiers dans src/, puis on vérifie</span>

`components/` `pages/` `hooks/` `types/` `data/`, puis `npm run dev` : testez une classe Tailwind.

</div>

<style>
h1 { font-size: 2rem; margin-bottom: 0.4rem; }
td, th { padding-top: 0.22rem !important; padding-bottom: 0.22rem !important; }
.box { padding-top: 0.55rem; padding-bottom: 0.55rem; }
</style>

<!--
Le seul moment de la séance où il faut vraiment circuler vite.

L'installeur est INTERACTIF : s'ils répondent au hasard, on se retrouve avec trois
configurations différentes dans la salle. Lire les quatre réponses à voix haute.
La variante "TypeScript + React Compiler" est celle qui donne le vite.config.ts
à trois plugins montré à droite.

Le ménage : le template Vite livre 111 lignes dans index.css et
184 dans App.css, purement décoratives. Celle qui fait le plus de dégâts :

  #root { width: 1126px; text-align: center; border-inline: 1px solid ... }
  h1 { font-size: 56px }

Résultat s'ils ne nettoient pas : le contenu reste centré dans une colonne de
1126px avec deux bordures verticales, et tous les h1 font 56px quoi qu'ils écrivent.
Ils croiront que Tailwind ne marche pas, et l'exercice 2 sera faussé.

Erreurs fréquentes :
- une autre variante choisie → leur vite.config.ts n'a que react(), c'est bon aussi,
  ils ajoutent simplement tailwindcss() à côté
- Tailwind installé mais l'import oublié dans src/index.css : rien ne change,
  et ils cherchent pendant dix minutes
- index.css vidé EN ENTIER, @import compris : plus aucune classe ne s'applique
- version de Node trop ancienne : Vite 8 exige Node 20+

Les dossiers hooks/ et types/ seront vides jusqu'à plus tard. C'est normal, le dire.

Si quelqu'un est bloqué plus de 5 minutes : lui donner un projet déjà monté
et le débloquer à la pause. Ne pas laisser un élève rater la partie 1 pour un npm.
-->
---
layout: section
---

<div class="part-num">01</div>

# Bonnes pratiques

<div class="muted mt-2">Trois règles, et une arborescence de projet</div>

---
layout: center
---

# Pourquoi des règles

<div class="grid grid-cols-2 gap-6 mt-8 text-left">

<div class="box bad" v-click>
<span class="box-label">Sans</span>

Votre code marche. Vous le relisez dans trois semaines : vous ne comprenez plus rien.

Vous n'osez plus rien modifier de peur de casser.

</div>

<div class="box good" v-click>
<span class="box-label">Avec</span>

Chaque fichier tient sur un écran. Vous savez où chercher.

Vous ajoutez une fonctionnalité sans relire tout le reste.

</div>

</div>

<div class="mt-8 big" v-click>

Les trois règles qui suivent ne sont pas des goûts personnels.<br/>
Chacune évite un **bug précis** que vous allez rencontrer.

</div>

<!--
Ne pas vendre ça comme "du code propre pour faire joli".
Chaque règle = un bug. C'est le seul argument qui les fait accrocher.
-->

---

# 1 · Un composant fait une seule chose

<div class="muted -mt-3 mb-4 text-sm">séparation des responsabilités</div>

<div v-if="$clicks < 2">

```tsx
const TierListPage = () => {
  // 25 lignes de state
  // 20 lignes de fonctions
  return (
    <div>
      {/* 180 lignes de TSX : le menu, les filtres,
          les 4 lignes de tiers, les cartes, le footer */}
    </div>
  )
}
```

</div>

<div class="big mt-4" v-click="1">Qu'est-ce qui cloche ?</div>

<div class="grid grid-cols-2 gap-4 mt-4" v-click="2">

<div class="box good">

```tsx
const TierListPage = () => {
  return (
    <>
      <PageTitre texte="Ma tier list" />
      <TierRow tier="S" games={jeuxS} />
      <TierRow tier="A" games={jeuxA} />
    </>
  )
}
```

</div>

<div class="box rule">
<span class="box-label">La règle</span>

Si vous devez **scroller** pour lire votre composant, découpez-le.

Si vous ne pouvez pas le nommer en un seul mot, il fait deux choses.

</div>

</div>

<!--
Les faire répondre à voix haute AVANT le reveal. Attendre 20 secondes,
même si c'est silencieux. Ils trouvent toujours "c'est trop long".
Enchaîner : "trop long, oui, mais pourquoi c'est un problème ?"
Réponses attendues : illisible, pas réutilisable, pas testable,
impossible de travailler à deux dessus sans conflit git.
-->

---

# 2 · Un composant annonce ce qu'il reçoit

<div class="muted -mt-3 mb-4 text-sm">typer ses props · jamais <code>any</code></div>

<div v-if="$clicks < 3">

```tsx
const GameCard = (props: any) => {
  return <h2>{props.name}</h2>
}
```

</div>

<div class="big mt-3" v-click="1">Ce composant affiche du vide. Pourquoi ?</div>

<div class="box bad mt-3" v-click="2">

La prop s'appelle `game`, pas `name` : il fallait écrire `props.game.name`.
Avec `any`, **TypeScript se tait**. Aucun souligné rouge, juste une page vide.

</div>

<div v-click="3" class="mt-3">

```tsx
type GameCardProps = {
  game: GameCardType
}

const GameCard = ({ game }: GameCardProps) => {
  return <h2>{game.name}</h2>
}
```

<div class="box rule mt-3">
<span class="box-label">La règle</span>

Un type au-dessus de chaque composant. En échange, votre éditeur
autocomplète les props et hurle quand vous en oubliez une.

</div>

</div>

---
class: code-xs
---

# 2 · Les props en pratique

<div class="muted -mt-3 mb-4 text-sm">déclarer, typer, utiliser</div>

<div class="grid grid-cols-2 gap-4">

<div>

<div class="tag tag-cyan mb-2">Le composant reçoit</div>

```tsx
type GameCardProps = {
  name: string
  note: "S" | "A" | "B" | "C" | "D"
  isNew?: boolean
}
const GameCard = (
  { name, note, isNew }: GameCardProps
) => {
  return (
    <article>
      <h2>{name} — rang {note}</h2>
      {isNew && <span>NOUVEAU</span>}
    </article>
  )
}
export default GameCard
```

</div>

<div v-click="1">

<div class="tag tag-violet mb-2">La page envoie</div>

```tsx
import GameCard from '../components/GameCard'
const HomePage = () => {
  return (
    <div>
      <GameCard name="Hades" note="S" isNew />
      <GameCard name="Celeste" note="A" />
    </div>
  )
}
```

<div class="box rule mt-3" v-click="2">
<span class="box-label">Trois réflexes</span>

- Le `?` rend la prop **facultative**.
- Guillemets pour une string, accolades pour le reste : `name="Hades"` mais `isNew={true}`.
- Un composant ne **modifie jamais** ses props.

</div>

</div>

</div>

<!--
La slide précédente montre comment TYPER les props. Celle-ci montre comment
les UTILISER — c'est le morceau qui leur manque toujours.

Sur Hades, `isNew` est écrit tout court : c'est le raccourci de `isNew={true}`. Le dire,
sinon ils cherchent où est passée la valeur. Ça ne marche que pour un booléen à true.

Les deux erreurs à guetter au TP :
- note="E" : TypeScript refuse, "E" n'est pas dans l'union. C'est tout l'intérêt du type littéral.
- vouloir modifier une prop dans le composant (name = name.toUpperCase())

Si quelqu'un connaît Next et écrit `@/components/GameCard` : cet alias n'existe pas
dans un projet Vite par défaut, il faut le configurer dans vite.config.ts ET
tsconfig.json. En chemin relatif, ça marche tout de suite.

Question à poser : "et si je veux passer le jeu entier plutôt que name + note ?"
Réponse : une seule prop `game: GameCardType`. C'est souvent mieux, et c'est ce qu'on fait
partout ailleurs dans le cours.
-->

---

# 3 · Une liste a besoin d'étiquettes qui ne bougent pas

<div class="muted -mt-3 mb-4 text-sm">la prop <code>key</code></div>

<div v-if="$clicks < 3">

```tsx
<div>
  {games.map((game, index) => (
    <GameCard key={index} game={game} />
  ))}
</div>
```

</div>

<div class="mt-3" v-click="1">

Ça marche. Jusqu'au jour où vous **réordonnez** votre tier list.

<div class="tier tier-s mt-3">
  <div class="tier-badge">S</div>
  <div class="tier-games">
    <span class="chip">Hollow Knight</span><span class="chip">Hades</span><span class="chip">Celeste</span>
  </div>
</div>

</div>

<div class="box bad mt-3" v-click="2">

Vous montez Celeste en premier. Les étiquettes `0, 1, 2` restent aux mêmes **positions**,
mais elles désignent maintenant d'autres jeux. React croit que le jeu n°0 a juste
changé de titre : il **garde** son contenu interne. Case cochée, animation, champ de saisie :
sur le mauvais jeu.

</div>

<div v-click="3" class="mt-3">

```tsx
<GameCard key={game.name} game={game} />
```

<div class="box rule mt-3">
<span class="box-label">La règle</span>

La `key` répond à « **est-ce le même élément qu'avant ?** ».
Un index répond « est-ce à la même position ? ». Ce n'est pas la même question.

</div>

</div>

---
layout: center
class: code-xs
---

<div class="text-center">
<div class="tag tag-amber mb-3">Quiz · 30 secondes</div>

# Combien d'erreurs ?

</div>

<div class="grid grid-cols-2 gap-5 mt-4">

<div>

```tsx
type GameListProps = {
  games: GameCardType[]
}

const GameList = (props: any) => {
  return (
    <div>
      {props.games.map((game) => (
        <GameCard game={game} />
      ))}
    </div>
  )
}
```

</div>

<div v-click>

<div class="box info">

**Deux.** Le type existe juste au-dessus, mais le composant reçoit `any`.
Et la `key` manque sur la liste.

```tsx
const GameList = ({ games }: GameListProps) => {
  return (
    <div>
      {games.map((game) => (
        <GameCard key={game.name} game={game} />
      ))}
    </div>
  )
}
```

</div>

</div>

</div>

---
class: code-xs
---

<div class="text-center">
# <span class="tag tag-cyan mr-3">Exo 1</span>Les données et la carte <span class="timer ml-3">⏱ 10 minutes</span>

</div>

<div class="grid grid-cols-2 gap-5 mt-4 text-sm">

<div>

1. Dans `types/game.ts`, le type `GameCardType` : name, studio, releaseDate, note
2. Dans `data/games.ts`, un tableau `gameList: GameCardType[]`
3. Remplissez-le avec **les jeux ci-contre** : à vous de trouver le studio, la date de sortie et de mettre votre note
4. Un composant `GameCard` avec un type de props explicite. **Au bon endroit.**
5. Affichez la liste dans `App.tsx`, avec une `key` stable

</div>

<div>

<div class="box">
<span class="box-label">Les jeux à saisir</span>

- League of Legends
- World of Warcraft
- Valorant
- Pokémon Champions
- God of War Ragnarök
- EA Sports FC

</div>

</div>

</div>

<div class="box good mt-3">
<span class="box-label">Checkpoint</span>

Les six jeux s'affichent. Zéro `any`, zéro `key={index}`, et `GameCard.tsx` est dans
`components/`, pas dans `pages/`. Ne stylez rien : c'est la partie 2.

</div>

<style>
h1 { font-size: 2rem; margin-bottom: 0.4rem; }
.box { padding-top: 0.55rem; padding-bottom: 0.55rem; }
</style>

<!--
La liste est imposée pour deux raisons : ils ne perdent pas cinq minutes à choisir,
et tout le monde a les mêmes données à l'écran au moment de la correction.
Le travail reste le leur : studio, date de sortie, note.

À DIRE À L'ORAL : la note n'est pas un chiffre, c'est un rang — S, A, B, C ou D.
Sinon ils partent sur un number et la carte de l'exercice 2 ne collera pas.

Les trois règles qu'on vient de voir sont toutes exerçables ici. Circuler et
poser la question qui va bien plutôt que corriger :
- "ce composant, tu peux le nommer en un seul mot ?"
- "c'est quoi le type de cette prop ?"
- "ta key, elle change si tu réordonnes la liste ?"

Erreurs fréquentes :
- GameCard rangé dans pages/ : c'est LE point de l'exo, les reprendre dessus
- key={index} par réflexe, alors qu'on vient d'en parler
- note="9.5" entre guillemets, donc une string

S'ils demandent : la note est la LEUR, il n'y a pas de bonne réponse.
Pour les jeux-services (LoL, WoW, Valorant, FC), l'année est celle de la sortie
initiale — leur dire, sinon ils cherchent longtemps.
-->
---
layout: section
---

<div class="part-num">02</div>

# Tailwind CSS

<div class="muted mt-2">Styler sans chercher de nom de classe</div>

---

# Ce qui fatigue en CSS classique

<v-clicks>

- **Trouver un nom.** `.card`, `.game-card`, `.card-title`, `.card-title--big`… vous passez plus de temps à nommer qu'à styler.
- **La cascade.** Votre règle ne s'applique pas, quelque chose l'écrase, vous ajoutez `!important` et vous passez à autre chose.
- **Le CSS mort.** Vous supprimez un composant, sa CSS reste. Six mois plus tard, personne n'ose y toucher.
- **L'aller-retour.** Modifier une couleur = deux fichiers ouverts et un va-et-vient constant.

</v-clicks>

<div class="box info mt-8" v-click>

Tailwind ne remplace pas le CSS. **C'est du CSS**, avec une classe par propriété,
et les noms déjà trouvés pour vous.

</div>

<!--
Question à leur poser d'abord : "qui a déjà galéré à nommer une classe CSS ?"
Toutes les mains se lèvent. C'est l'accroche.
-->

---
class: code-xs
---

# Le même composant, deux fois

<div class="grid grid-cols-2 gap-4 mt-2">

<div>

<div class="tag mb-2">CSS classique · 2 fichiers</div>

```css
/* GameCard.css */
.game-card {
  display: flex;
  gap: 12px;
  padding: 16px;
  border-radius: 12px;
  background: #1e293b;
}
.game-card__name {
  font-weight: 600;
  color: #f8fafc;
}
.game-card__studio {
  font-size: 14px;
  color: #94a3b8;
}
```

```tsx
<article className="game-card">
  <h2 className="game-card__name">{game.name}</h2>
  <p className="game-card__studio">{game.studio}</p>
</article>
```

</div>

<div v-click>

<div class="tag tag-cyan mb-2">Tailwind · 1 fichier</div>

```tsx
<article
  className="flex gap-3 p-4 rounded-xl bg-slate-800"
>
  <h2 className="font-semibold text-slate-50">
    {game.name}
  </h2>
  <p className="text-sm text-slate-400">
    {game.studio}
  </p>
</article>
```

<div class="box good mt-3">

Zéro nom à inventer. Zéro fichier à ouvrir à côté.
Vous supprimez le composant : le style part avec.

</div>

</div>

</div>

---

# Comment ça marche

Une classe = une propriété CSS. Le nom décrit ce qu'elle fait.

<div class="grid grid-cols-2 gap-4 mt-5">

<div class="box">

| Classe | CSS |
|---|---|
| `flex` | `display: flex` |
| `p-4` | `padding: 1rem` |
| `gap-3` | `gap: 0.75rem` |
| `rounded-xl` | `border-radius: 0.75rem` |
| `text-sm` | `font-size: 0.875rem` |
| `bg-slate-800` | `background: #1e293b` |

</div>

<div>

<v-clicks>

- **Une échelle imposée.** `p-4` vaut 1rem, `p-6` vaut 1.5rem. Vous n'écrivez plus `padding: 13px` par accident : tout l'écran reste cohérent.
- **Des couleurs nommées.** `slate-400`, `slate-800` : une palette de 11 nuances par teinte, déjà équilibrée.
- **Rien d'inutile livré.** Au build, Tailwind lit vos fichiers et ne garde que les classes réellement utilisées.

</v-clicks>

</div>

</div>

<div class="box rule mt-4" v-click>

Vous n'avez pas à retenir les classes. Vous tapez ce que vous voulez faire
dans la doc, ou vous laissez l'extension VS Code compléter.

</div>

---

# Responsive et dark mode

Un préfixe devant la classe, et c'est réglé.

<div class="grid grid-cols-2 gap-4 mt-4">

<div>

<div class="tag mb-2">CSS classique</div>

```css
.grille {
  display: grid;
  grid-template-columns: 1fr;
}
@media (min-width: 768px) {
  .grille {
    grid-template-columns: repeat(3, 1fr);
  }
}
@media (prefers-color-scheme: dark) {
  .grille { background: #0f172a; }
}
```

</div>

<div>

<div v-click="1">

<div class="tag tag-cyan mb-2">Tailwind</div>

```tsx
<div className="grid grid-cols-1 md:grid-cols-3
                dark:bg-slate-900">
```

</div>

<div class="mt-4"></div>

<v-clicks at="2">

- `md:` s'applique **à partir de** 768px
- `sm:` `md:` `lg:` `xl:` pour les autres seuils
- `dark:` quand le système est en thème sombre
- `hover:` `focus:` fonctionnent pareil

</v-clicks>

</div>

</div>

<div class="box info mt-4" v-click="6">

Tailwind est **mobile-first** : une classe sans préfixe s'applique partout,
et le préfixe ne fait que l'écraser à partir d'une certaine largeur.

</div>

---
layout: center
class: text-center
---

# <span class="tag tag-cyan mr-3">Exo 2</span>Reproduisez cette liste <span class="timer ml-3">⏱ 12 minutes</span>

<div class="flex justify-center gap-8 mt-6 items-start">

<div class="text-left">

<div class="demo-card">
  <div class="demo-card-badge">A</div>
  <div>
    <div class="demo-card-title">League of Legends</div>
    <div class="demo-card-studio">Riot Games · 2009</div>
  </div>
</div>

<div class="demo-card mt-3">
  <div class="demo-card-badge">S</div>
  <div>
    <div class="demo-card-title">World of Warcraft</div>
    <div class="demo-card-studio">Blizzard Entertainment · 2004</div>
  </div>
</div>

<div class="muted text-center mt-3" style="letter-spacing: 0.35em">···</div>

</div>

<div class="text-left text-sm">

<div class="box">
<span class="box-label">Le cahier des charges</span>

- Tout en **Tailwind**, aucun fichier `.css`
- Une carte = une ligne horizontale, éléments centrés verticalement
- Fond sombre, coins arrondis, `padding` confortable
- Le badge : carré, coloré, texte en gras
- Le titre en clair, le studio plus petit et plus terne
- Les cartes empilées, même espace entre chacune
- Au survol : le fond s'éclaircit

</div>

<div class="mt-3 muted">Doc ouverte : <b>tailwindcss.com/docs</b></div>

</div>

</div>

<!--
Écrire au tableau les 3 classes qu'ils ne devineront pas :
  items-center   (alignement vertical dans la carte)
  flex-col gap-3 (l'empilement régulier de la liste)
  hover:bg-...   (le survol)

Solution :
<div className="flex flex-col gap-3">
  {games.map((game) => (
    <article key={game.name}
      className="flex items-center gap-4 p-4 rounded-xl bg-slate-800
                 hover:bg-slate-700 transition-colors">
      <div className="grid place-items-center w-10 h-10 rounded-lg
                      bg-rose-500 font-bold text-slate-900">{game.note}</div>
      <div>
        <h2 className="font-semibold text-slate-50">{game.name}</h2>
        <p className="text-sm text-slate-400">{game.studio} · {game.releaseDate.slice(0, 4)}</p>
      </div>
    </article>
  ))}
</div>

Ne pas donner la solution avant 9 minutes. Faire corriger par un étudiant
au vidéoprojecteur pendant que les autres dictent.

Ils réutilisent le GameCard de l'exercice 1 : le travail ici, c'est le style,
pas la structure. Le badge affiche la note, qui est un rang (S/A/B/C/D).
-->

---
layout: center
class: text-center
---

# Pause

<div class="timer mt-6" style="font-size: 1.1rem">⏱ 10 minutes</div>

---
layout: section
---

<div class="part-num">03</div>

# Routage et navigation

<div class="muted mt-2">D'une page à cinq, sans jamais recharger</div>

---

# Le problème

Votre application n'a qu'une page. Alors on bricole.

<div v-if="$clicks < 6">

```tsx
const App = () => {
  const [page, setPage] = useState('accueil')

  return (
    <div>
      {page === 'accueil' && <HomePage />}
      {page === 'tierList' && <TierListPage />}
    </div>
  )
}
```

</div>

<div class="mt-3" v-click="1">Honnêtement, ça marche. Puis vous testez :</div>

<v-clicks at="2">

- L'URL reste `localhost:5173`. **Toujours.**
- Le bouton retour du navigateur quitte votre application.
- Vous ne pouvez envoyer **aucun lien** à quelqu'un.
- <span class="kbd">F5</span> et vous revenez à l'accueil.

</v-clicks>

<div class="mt-4 box rule" v-click="6">
<span class="box-label">Le vrai sujet</span>

**L'URL est un état.** Le seul que l'utilisateur peut copier, partager,
mettre en favori et recharger. Un router, c'est ce qui la branche sur vos composants.

</div>

<!--
Si un seul concept passe aujourd'hui, c'est "l'URL est un état".
Le répéter au moins trois fois pendant la partie 3.
-->

---

# Deux façons de faire un site

<div class="grid grid-cols-2 gap-6 mt-6">

<div class="box">

### Site classique

Chaque clic part au serveur, qui renvoie une nouvelle page HTML complète.

<div class="mt-3 muted text-sm">
Le navigateur jette tout et repart de zéro.<br/>
Écran blanc entre chaque page.
</div>

</div>

<div class="box" v-click>

### Application monopage · SPA

Un seul HTML chargé une fois. Ensuite JavaScript réécrit le contenu **et** l'URL.

<div class="mt-3 muted text-sm">
Aucun rechargement, navigation instantanée.<br/>
Votre state React survit.
</div>

</div>

</div>

<div v-click class="mt-8">

<div class="box info">
<span class="box-label">Démo — ouvrez l'onglet Réseau de vos devtools</span>

D'abord, la librairie qui fournit `<Link>` : `npm install react-router-dom`

Je clique sur un `<a href="/tier-list">`. Regardez la liste des requêtes.
Puis sur un `<Link to="/tier-list">`. Regardez encore.

</div>

</div>

<!--
NE PAS SAUTER CETTE DÉMO. C'est le moment où ça devient concret.
- Avec <a href> : la liste réseau se vide, tout se recharge, l'onglet clignote.
- Avec <Link> : rien du tout, zéro requête.
Encore plus parlant : mettre un compteur useState visible à l'écran.
Avec <a href> il retombe à 0, avec <Link> il survit.
Leur faire refaire sur leur machine avant de continuer — d'où l'installation
annoncée ici, avant la slide suivante qui la reprend comme référence.
-->

---

# Les trois briques

<div class="muted mb-3 text-sm">Installation : <code>npm install react-router-dom</code></div>

<div v-if="$clicks < 4">

```tsx {all|5|6|7-8}
import { BrowserRouter, Routes, Route } from 'react-router-dom'

const App = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/tier-list" element={<TierListPage />} />
      </Routes>
    </BrowserRouter>
  )
}
```

</div>

<div v-click="1">

- `BrowserRouter` branche votre app sur l'historique du navigateur. **Une seule fois**, tout en haut.

</div>

<div v-click="2">

- `Routes` regarde l'URL et choisit **une seule** route gagnante.

</div>

<div v-click="3">

- `Route` associe un chemin à un composant.

</div>

<div class="box trap mt-3" v-click="4">

`element={<HomePage />}` **avec** les chevrons. Pas `element={HomePage}`.
C'est l'erreur numéro un de la séance.

</div>

---
layout: center
class: text-center
---

# <span class="tag tag-cyan mr-3">Exo 3</span>Trois pages, trois routes <span class="timer ml-3">⏱ 10 minutes</span>

<div class="mt-6 text-left mx-auto w-fit">

1. `npm install react-router-dom`
2. Dans `pages/`, créez `HomePage.tsx`, `TierListPage.tsx`, `AboutPage.tsx`
   — un `h1` dans chacune, rien de plus
3. Dans `App.tsx` : `BrowserRouter`, `Routes`, et trois `Route`
4. Les chemins : `/`, `/tier-list`, `/a-propos`

</div>

<div class="mt-6 box good text-left">
<span class="box-label">Checkpoint</span>

Vous tapez les trois URL à la main dans la barre d'adresse. Les trois répondent.

</div>

<!--
Erreurs à guetter dans les rangs :
- element={HomePage} sans chevrons
- BrowserRouter oublié → "useRoutes() may be used only in the context of a Router"
- export default oublié dans la page
Ne rien corriger au tableau avant 7 minutes.
-->

---

# Naviguer : trois outils, un seul mauvais

<div class="grid grid-cols-2 gap-4 mt-2">

<div>

<div class="box bad">
<span class="box-label">Jamais dans une SPA</span>

```tsx
<a href="/tier-list">Tier list</a>
```

Rechargement complet. Vous perdez tout votre state React.

</div>

<div class="box good mt-3" v-click="1">
<span class="box-label">Le choix par défaut</span>

```tsx
<Link to="/tier-list">Tier list</Link>
```

Navigation instantanée. Dans le DOM final c'est un vrai `<a>` :
le clic droit « ouvrir dans un nouvel onglet » marche toujours.

</div>

</div>

<div class="box good" v-click="2">
<span class="box-label">Pour un menu</span>

```tsx
<NavLink
  to="/tier-list"
  className={({ isActive }) =>
    isActive ? 'text-white' : 'text-slate-400'
  }
>
  Tier list
</NavLink>
```

`NavLink` **sait** s'il pointe vers la page affichée,
et `className` accepte une fonction.

</div>

</div>

---
class: code-xs
---

# <span class="tag tag-cyan mr-3">Mini-exo</span>Votre barre de navigation <span class="timer ml-3">⏱ 5 minutes</span>

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="tag tag-violet mb-1">Le squelette d'un lien</div>

```tsx
<nav className="flex gap-4">
  <NavLink
    to="/"
    end
    className={({ isActive }) =>
      isActive ? 'text-white' : 'text-slate-400'
    }
  >
    Accueil
  </NavLink>

  {/* les deux autres, à vous */}
</nav>
```

</div>

<div>

<div class="text-sm">

1. Dans `App.tsx`, **au-dessus** de `<Routes>`, ajoutez ce `<nav>`
2. Complétez avec `/tier-list` et `/a-propos`
3. Choisissez vos deux couleurs, actif et inactif

</div>

<div class="box info mt-3">
<span class="box-label">Ce que fait ce className</span>

Il accepte une **fonction**, pas une chaîne. React Router la rappelle à chaque changement d'URL avec `{ isActive }` : à vous de renvoyer les classes. Zéro `useState`.

</div>

<div class="box trap mt-2">
<span class="box-label">Le mot-clé <code>end</code></span>

Sans lui, « Accueil » reste allumé **partout** : tous les chemins commencent par `/`.

</div>

</div>

</div>

<style>
h1 { font-size: 2rem; margin-bottom: 0.4rem; }
.box { padding-top: 0.55rem; padding-bottom: 0.55rem; }
</style>

<!--
Court et volontairement facile : l'objectif n'est pas la difficulté, c'est
qu'ils AIENT une nav sous les yeux avant qu'on parle de Layout.

Le squelette est donné exprès. Ce qu'on veut leur faire sentir, c'est le
className-fonction, pas leur faire chercher une syntaxe.

Erreurs à guetter :
- <a href> par réflexe → la page recharge, ça se voit dans l'onglet Réseau
- className={isActive ? ...} sans les accolades de déstructuration
- end oublié sur "/" → les trois liens allumés en même temps, ils ne comprennent pas
- NavLink pas importé depuis react-router-dom

Ne pas les laisser styler pendant 10 minutes. Deux couleurs suffisent.

Checkpoint : ils cliquent, l'URL change, le lien courant se démarque, et
l'onglet Réseau ne bouge pas. C'est une SPA.
-->

---
class: code-xs
---

# Ça marche. Et pourtant

<div class="grid grid-cols-5 gap-5 mt-3">

<div class="col-span-3">

<div class="tag mb-1">Votre App.tsx, maintenant</div>

```tsx
const App = () => {
  return (
    <BrowserRouter>
      <nav>
        <NavLink to="/" end className={…}>Accueil</NavLink>
        <NavLink to="/tier-list" className={…}>Tier list</NavLink>
        <NavLink to="/a-propos" className={…}>À propos</NavLink>
      </nav>

      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/tier-list" element={<TierListPage />} />
        <Route path="/a-propos" element={<AboutPage />} />
      </Routes>
    </BrowserRouter>
  )
}
```

</div>

<div class="col-span-2">

<div class="big mt-4" v-click="1">Ajoutez maintenant un footer, un conteneur, du padding.</div>

<div class="box bad mt-3" v-click="2">
<span class="box-label">Deux problèmes</span>

`App.tsx` fait **deux métiers** : la table des routes **et** la mise en page.

Et une page qui ne doit **pas** avoir ce cadre — une 404 plein écran — devient impossible.

</div>

<div class="big mt-3" v-click="3">Il faut un cadre, avec un trou où la page s'insère.</div>

</div>

</div>

<!--
Ne pas enchaîner trop vite. Leur faire dire à voix haute ce qui les gêne
dans le fichier de gauche. Ils répondent souvent "c'est long" : recadrer sur
"il fait deux choses", c'est la règle 1 du cours, elle revient ici.

Le deuxième argument est le plus fort : une page SANS le cadre est impossible
tant que le cadre est dans App. C'est exactement ce que résout Layout.
-->

---
class: code-xs
---

# Étape 1 · Le cadre

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="tag tag-violet mb-1">components/Layout.tsx</div>

```tsx {all|8}
import { Outlet, NavLink } from 'react-router-dom'

const Layout = () => {
  return (
    <div className="min-h-screen p-8">
      <nav>{/* vos trois NavLink */}</nav>

      <main><Outlet /></main>

      <footer>GameRank</footer>
    </div>
  )
}
export default Layout
```

</div>

<div>

<v-clicks at="2">

- On **déplace** la nav de `App.tsx` vers ici. Elle n'est plus écrite qu'une fois.
- `<Outlet />` est un **trou**. Il ne prend aucune prop : il n'y a rien à configurer dedans.
- Le `Layout` ne connaît **aucune** de vos pages. C'est ce qui le rend réutilisable.

</v-clicks>

<div class="box info mt-3" v-click="5">
<span class="box-label">Si vous venez de Next</span>

C'est `app/layout.tsx`. Même idée, même rôle. Une seule différence, et elle est de taille :
Next l'applique **tout seul**, React Router attend que vous le déclariez. C'est l'étape 2.

</div>

</div>

</div>

<!--
Insister sur "Outlet ne prend aucune prop". C'est LA question qui revient :
"on configure quoi dedans ?" Rien. C'est un marqueur de position.

L'analogie qui marche : c'est children, sauf que ce n'est pas le parent qui
passe l'enfant, c'est le routeur, en fonction de l'URL.

À ce stade le Layout existe mais ne s'affiche nulle part. Le dire explicitement,
sinon ils croient avoir fini.
-->

---
class: code-xs
---

# Étape 2 · Brancher les routes

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="tag mb-1">Avant</div>

```tsx
<Route path="/" element={<HomePage />} />
<Route path="/tier-list" element={<TierListPage />} />
<Route path="/a-propos" element={<AboutPage />} />
```

<div class="tag tag-cyan mb-1 mt-3">Après</div>

```tsx
<Route path="/" element={<Layout />}>
  <Route index element={<HomePage />} />
  <Route path="tier-list" element={<TierListPage />} />
  <Route path="a-propos" element={<AboutPage />} />
</Route>
```

</div>

<div>

<v-clicks>

- La route parente **n'est plus auto-fermante** : `<Route …>` … `</Route>`, les enfants dedans. C'est cette imbrication qui crée la relation.
- Les chemins enfants **perdent leur `/`** : `"tier-list"`, pas `"/tier-list"`. Ils se collent au parent.
- `HomePage` passe de `path="/"` à **`index`** : l'enfant affiché quand l'URL vaut exactement le chemin du parent.

</v-clicks>

<div class="box info mt-3" v-click="4">
<span class="box-label">Sur /tier-list, dans l'ordre</span>

`/` gagne → React Router rend `<Layout />` : la nav et le footer.
Puis `tier-list` gagne → `<TierListPage />` prend la place de l'`<Outlet />`.

</div>

</div>

</div>

<!--
C'est LA slide de la partie. Ne pas la presser.

Le malentendu numéro un : "je crée Layout.tsx et c'est bon". Non — sans cette
imbrication, le Layout n'est jamais rendu. Le test à leur donner : mettre une
bordure rouge dans le Layout, et constater qu'on ne la voit jamais.

La preuve à faire en direct dans les devtools React : la nav n'est PAS remontée
quand on change de page. C'est ce que le cadre partagé apporte.
-->

---
class: code-xs
---

# Étape 3 · La page détail

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="tag tag-violet mb-1">pages/GameDetailPage.tsx</div>

```tsx
const GameDetailPage = () => {
  return <h1>Une page de jeu</h1>
}
export default GameDetailPage
```

```txt {5}
src/
├── components/
│   └── Layout.tsx
├── pages/
│   ├── GameDetailPage.tsx
│   ├── HomePage.tsx
│   ├── TierListPage.tsx
│   └── AboutPage.tsx
└── App.tsx
```

<div class="muted text-sm mt-2">Une page, c'est un fichier dans <code>pages/</code>. Exactement comme à l'exercice 3.</div>

</div>

<div>

<div class="tag tag-cyan mb-1" v-click="1">Et on la branche, sous le Layout</div>

<div v-click="1">

```tsx
<Route path="/" element={<Layout />}>
  <Route index element={<HomePage />} />
  <Route path="jeu/hollow-knight"
    element={<GameDetailPage />} />
</Route>
```

</div>

<div class="box trap mt-3" v-click="2">
<span class="box-label">Créer le fichier ne crée pas la route</span>

Le fichier peut exister, être parfait, et ne **jamais** s'afficher.
C'est `App.tsx` qui décide des URL, **pas** les noms de vos dossiers.

</div>

<div class="big mt-3" v-click="3">Tapez <code>/jeu/hollow-knight</code> : ça marche.</div>

<div class="big mt-1" v-click="4">Pour <b>un</b> jeu.</div>

</div>

</div>

<!--
Slide charnière : ils CRÉENT avant qu'on théorise. La friction de la slide suivante
n'a de valeur que s'ils ont vu leur page s'afficher pour de vrai d'abord.

Deux minutes, pas plus. Le composant est volontairement vide : un h1, rien d'autre.
Ce n'est pas le sujet, le sujet c'est la route.

L'encadré orange est LE message. Le faire dire à voix haute : créer le fichier ne
crée pas la route. C'est la différence avec Next, et c'est la panne qu'ils vont vivre —
fichier créé, URL tapée, page blanche.

Le rangement : le test de la partie 1 s'applique encore. Est-ce que l'utilisateur
dirait "je suis sur cette page" ? Oui -> pages/. Layout, non -> components/.

Finir sur "pour UN jeu" et s'arrêter là. Les laisser trouver le problème eux-mêmes,
ne pas enchaîner. Quelqu'un dira "on va pas en écrire 200".

Rappel pour moi : dans ce cours le BrowserRouter est dans App.tsx, pas dans main.tsx.
-->

---

# Une route pour tous les jeux

```tsx
<Route path="/" element={<Layout />}>
  <Route path="jeu/hollow-knight" element={<GameDetailPage />} />
  <Route path="jeu/hades" element={<GameDetailPage />} />
</Route>
```

<div v-click="1" class="mt-3 box bad">Et pour 200 jeux ? Et quand vous en ajoutez un ?</div>

<div v-click="2" class="mt-5">

Les deux points déclarent un **paramètre** : un morceau d'URL qui varie.

```tsx
<Route path="/" element={<Layout />}>
  <Route path="jeu/:slug" element={<GameDetailPage />} />
</Route>
```

</div>

<div class="flow mt-5" v-click="3">
  <div class="flow-node"><b>/jeu/hades</b>slug = "hades"</div>
  <div class="flow-node"><b>/jeu/celeste</b>slug = "celeste"</div>
  <div class="flow-node"><b>/jeu/nawak</b>slug = "nawak"</div>
</div>

<div class="mt-4 muted text-sm" v-click="4">

Une seule route, un seul composant, autant de pages que de jeux.
Regardez la troisième : **l'URL matche quand même**. On y revient.

</div>

<!--
Ils viennent d'écrire la route en dur. Ici on duplique sous leurs yeux : c'est le
moment où quelqu'un râle. Laisser venir la râlerie avant de cliquer.

La route détail reste ENFANT du Layout : la nav et le footer sont là aussi.
Le montrer imbriqué à chaque fois, sinon ils la collent à plat et perdent le cadre.

Bien appuyer sur le troisième nœud du schéma : /jeu/nawak matche la route.
React Router ne vérifie pas que le jeu existe, ce n'est pas son travail.
C'est le point de départ de la slide sur les pages blanches.
-->

---
class: code-xs
---

# Étape 4 · Lire le paramètre

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="tag tag-violet mb-1">pages/GameDetailPage.tsx</div>

```tsx {all|5|7|9-11}
import { useParams } from 'react-router-dom'
import { gameList } from '../data/games'

const GameDetailPage = () => {
  const { slug } = useParams()

  const game = gameList.find((g) => g.slug === slug)

  if (!game) {
    return <p>Ce jeu n'existe pas.</p>
  }

  return <h1>{game.name}</h1>
}
export default GameDetailPage
```

<div class="box trap mt-2" v-click="3">
<span class="box-label">Ne sautez pas ce if</span>

Sans lui, `/jeu/nawak` fait planter la page — et TypeScript vous y oblige de toute façon.

</div>

</div>

<div>

<v-clicks at="1">

- Les clés de `useParams()` sont **les noms déclarés dans la route** : `jeu/:slug` donne `{ slug }`.
- React Router transmet une **chaîne**. Il ne connaît pas vos jeux : à vous de retrouver la donnée.

</v-clicks>

<div class="box rule mt-2" v-click="2">
<span class="box-label">Rouge sous g.slug ? C'est normal</span>

Votre type `GameCardType` n'a pas de champ `slug`. Ajoutez-le, puis remplissez-le dans
`data/games.ts` : minuscules, sans accent, tirets.

`"Pokémon Champions"` → `"pokemon-champions"`

</div>

</div>

</div>

<!--
Le "il transmet une chaîne, il ne connaît pas vos jeux" débloque beaucoup de monde :
ils croient que le routeur va chercher la donnée. Non, il donne un morceau d'URL.

C'est TypeScript qui pose le problème du slug, pas moi : ils écrivent g.slug, ça
souligne en rouge, et là seulement on explique ce qu'est un slug. Ne pas dégainer
avant le souligné rouge — la question doit venir d'eux.

Pourquoi pas l'id ? /jeu/3 fonctionne, mais l'URL ne dit plus rien à personne.
Rappel de la slide "Le problème" : l'URL est un état, et un état qui se lit.
Pourquoi pas le titre ? /jeu/God%20of%20War%20Ragnar%C3%B6k. Ça suffit comme réponse.

Faire le lien avec Next si la question vient : ici useParams est synchrone,
ce n'est pas une Promise.
-->

---
class: code-xs
---

# Cliquer sur une carte

<div class="grid grid-cols-2 gap-5 mt-3">

<div>

<div class="mock">
  <div class="mock-bar">
    <div class="mock-dot"></div><div class="mock-dot"></div><div class="mock-dot"></div>
    <div class="mock-url">localhost:5173<b>/tier-list</b></div>
  </div>
  <div class="mock-body">
    <div class="mock-nav">
      <span>Accueil</span><span class="on">Tier list</span><span>À propos</span>
    </div>
    <div class="tier tier-s">
      <div class="tier-badge">S</div>
      <div class="tier-games">
        <span class="chip">Hollow Knight</span><span class="chip">Hades</span><span class="chip">Celeste</span>
      </div>
    </div>
  </div>
</div>

<div class="big mt-4">La page existe, l'URL marche.<br/>Reste à pouvoir y aller <b>en cliquant</b>.</div>

<div class="box good mt-4" v-click="2">
<span class="box-label">Le test qui prouve tout</span>

Copiez l'URL, collez-la dans un nouvel onglet. La bonne page s'affiche.
**Ça, un `useState` ne l'a jamais fait.**

</div>

</div>

<div>

<div class="tag tag-cyan mb-1">Là où vous affichez vos cartes</div>

```tsx
<Link to={`/jeu/${game.slug}`}>
  <GameCard game={game} />
</Link>
```

<v-clicks at="1">

- Le `Link` **enveloppe** la carte, il ne la remplace pas. `GameCard` ne connaît toujours pas le routeur — c'est la règle 1, une dernière fois.

</v-clicks>

</div>

</div>

<!--
Dernière brique, et la boucle est bouclée : URL -> paramètre -> donnée -> page,
et maintenant un clic produit l'URL.

Le Link ENVELOPPE la carte. S'ils mettent le Link DANS GameCard, le composant devient
dépendant du routeur et n'est plus réutilisable hors router. C'est la règle 1.

Finir sur l'encadré vert et le laisser à l'écran : c'est la réponse à la slide
"Le problème" de tout à l'heure. L'URL est un état. Ils viennent de le fabriquer.
-->
---
class: code-xs
---

# Les trois pages blanches

<div class="muted -mt-3 mb-3 text-sm">aucune erreur, aucun warning, juste du vide — votre checklist de debug</div>

<div class="grid grid-cols-3 gap-4">

<div class="box bad">
<span class="box-label">1 · Le slash de trop</span>

```tsx
<Route path="/" element={…}>
  <Route path="/tier-list" … />
</Route>
```

Le chemin enfant se colle au parent : ça donne `//tier-list`, que personne n'atteindra jamais.

</div>

<div class="box bad" v-click="1">
<span class="box-label">2 · Aucune route ne matche</span>

```tsx
<Route path="*"
  element={<NotFoundPage />} />
```

Sans elle, une URL inconnue n'affiche **rien**. `*` attrape tout le reste : mettez-la toujours, en dernier.

</div>

<div class="box bad" v-click="2">
<span class="box-label">3 · La donnée est introuvable</span>

```tsx
const game = gameList.find(…)
if (!game) return <p>…</p>
```

L'URL matche, la page se monte, mais `find()` rend `undefined`. Le `if` transforme le vide en message.

</div>

</div>

<div class="box rule mt-4" v-click="3">
<span class="box-label">Et pour renvoyer l'utilisateur ailleurs</span>

`useNavigate` : `const navigate = useNavigate()`, puis `navigate('/tier-list')`, ou `navigate(-1)` pour la page précédente.
La règle : l'utilisateur **clique pour aller** quelque part → `Link`. Le code **décide après une action** → `useNavigate`.

</div>

<!--
Cette slide est une checklist de debug, pas un cours. Leur dire de la
photographier : ils la ressortiront pendant le TP.

Les trois causes sont dans l'ordre où elles arrivent en vrai. La première est
celle qu'ils feront tous à l'exercice 4 — ne la corriger qu'au bout de deux
minutes de galère, elle ne s'oublie pas quand on l'a vécue.

useNavigate est cité, pas développé. S'ils veulent un exemple : le bouton
"← Retour" de la page détail, c'est navigate(-1).
-->
---
layout: center
class: text-center
---

# <span class="tag tag-cyan mr-3">Exo 4</span>Le layout et la page détail <span class="timer ml-3">⏱ 12 minutes</span>

<div class="mt-6 text-left mx-auto w-fit">

1. `components/Layout.tsx` : **déplacez-y votre nav**, ajoutez un `<Outlet />` et un `<footer>`
2. Passez vos routes en **routes imbriquées** sous le `Layout` — `index`, et pas de slash devant
3. **Créez** `pages/GameDetailPage.tsx` et branchez-la sur `jeu/:slug`, toujours sous le `Layout`
4. Ajoutez le champ `slug` au type `GameCardType` et à vos six jeux, puis lisez-le avec `useParams` — sans oublier le cas introuvable
5. Enveloppez chaque `GameCard` dans un `Link` vers `/jeu/<son slug>`
6. Une route `*` → `NotFoundPage`, et testez `/nawak`

</div>

<div class="mt-6 box good text-left">
<span class="box-label">Checkpoint</span>

Vous copiez l'URL `/jeu/hollow-knight`, vous la collez dans un nouvel onglet :
la bonne page s'affiche. Et le menu ne clignote jamais quand vous naviguez.

</div>

<!--
L'exercice le plus formateur de la séance. Circuler beaucoup.
La nav existe déjà depuis le mini-exercice : ici ils la DÉPLACENT, ils ne la
réécrivent pas. Le dire, sinon la moitié repart de zéro.

Erreurs fréquentes, toutes dans la slide "Les trois pages blanches" :
- slash devant la route enfant → page blanche. Les renvoyer à la checklist
  plutôt que de corriger : c'est l'exercice le plus formateur de la séance
- index oublié sur la route d'accueil
- oubli des backticks dans to={`/jeu/${game.slug}`}
- le slug oublié sur un jeu → sa carte pointe vers /jeu/undefined
- le Layout créé mais les routes non imbriquées → on ne le voit jamais
-->

---

# Récapitulatif

<div class="grid grid-cols-3 gap-4 mt-4 text-sm">

<div class="box">

### Bonnes pratiques

- Un composant, une seule chose
- Des props typées, jamais `any`
- `key` stable, jamais l'index
- `pages/` `components/` `hooks/` `types/` `data/`

</div>

<div class="box">

### Tailwind

- Une classe = une propriété
- Une échelle imposée, donc cohérente
- `md:` pour le responsive
- `dark:` `hover:` `focus:`
- Le style vit avec le composant

</div>

<div class="box">

### Routage et navigation

- `BrowserRouter` une fois, en haut
- `Routes` choisit **une** route
- `Link`, jamais `<a href>`
- `NavLink` pour le menu
- `Layout` + `Outlet`
- `:slug` + `useParams`
- `useNavigate` après une action
- `path="*"` toujours

</div>

</div>

<div class="mt-6 text-center big" v-click>

L'URL est un état : le seul que l'utilisateur peut copier, partager et recharger.

</div>

---

# Le TP

<div class="muted -mt-3 mb-4 text-sm">2 heures · le carnet de recettes · un projet neuf, de zéro</div>

<div class="grid grid-cols-2 gap-5">

<div class="box">

### Les quatre pages

<div class="mt-3 text-sm">

**1 · La liste** — `/recettes`<br/>
Vos recettes en cartes. Données locales et typées, une carte réutilisable.

**2 · Le détail** — `/recette/:slug`<br/>
Ingrédients, étapes, temps de préparation. Sans oublier le cas introuvable.

**3 · Le regroupement** — `/categorie/:nom`<br/>
Entrées, plats, desserts. Une seule route, autant de pages que de catégories.

**4 · La 404** — `path="*"`<br/>
En dernier, et testez-la pour de vrai.

</div>

</div>

<div class="box">

### Les règles du jeu

<div class="mt-3 text-sm">

- **Projet neuf.** `npm create vite@latest`. On ne prolonge pas GameRank : vous recâblez le router et le `Layout` vous-mêmes.
- `Layout` + `Outlet` + une nav en `NavLink`, comme en cours.
- Le style est **libre**, en Tailwind. Soyez créatifs.
- Librairies autorisées, sauf les kits de composants tout faits.
- L'IA pour **le contenu** — les recettes, les descriptions. Jamais pour le code.
- Les trois règles du cours s'appliquent : un composant une seule chose, des props typées, une `key` stable.

</div>

</div>

</div>

<!--
Le sujet est un projet NEUF, et c'est tout l'intérêt : s'ils prolongeaient GameRank,
ils ne réécriraient jamais le BrowserRouter, le Layout et l'Outlet — c'est-à-dire
exactement ce qui a besoin d'être refait une deuxième fois pour rentrer.
Le dire explicitement au lancement, sinon la moitié va vouloir ajouter une page à GameRank.

Les deux paramètres de route sont volontaires : :slug pour le détail, :nom pour la
catégorie. Ils rencontrent useParams sur deux routes différentes, pas une seule.

Le slug se fabrique à la main dans les données, comme en cours : minuscules, sans
accent, tirets. "Tarte Tatin" -> "tarte-tatin".

L'IA pour le contenu : qu'ils s'en servent pour écrire vingt recettes crédibles en
cinq minutes. Le temps gagné là doit passer dans le routage et le style, pas ailleurs.

La page de regroupement est celle qu'ils sous-estiment : leur rappeler qu'une catégorie
inexistante doit se comporter comme un jeu inexistant. C'est le même if.
-->
