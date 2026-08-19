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

### Routage

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

<div class="tag tag-cyan mb-2">Exercice 0</div>

# Créer le projet <span class="timer ml-3">⏱ 10 minutes</span>

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
  return <h2>{props.titre}</h2>
}
```

</div>

<div class="big mt-3" v-click="1">Ce composant affiche du vide. Pourquoi ?</div>

<div class="box bad mt-3" v-click="2">

La prop s'appelle `game`, pas `titre` : il fallait écrire `props.game.titre`.
Avec `any`, **TypeScript se tait**. Aucun souligné rouge, juste une page vide.

</div>

<div v-click="3" class="mt-3">

```tsx
type GameCardProps = {
  game: Game
  onSelect?: (slug: string) => void
}

const GameCard = ({ game, onSelect }: GameCardProps) => {
  return <h2>{game.titre}</h2>
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
  titre: string
  note: number
  nouveau?: boolean
}
const GameCard = (
  { titre, note, nouveau }: GameCardProps
) => {
  return (
    <article>
      <h2>{titre} — {note}/10</h2>
      {nouveau && <span>NOUVEAU</span>}
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
      <GameCard titre="Hades" note={9.5} nouveau />
      <GameCard titre="Celeste" note={9} />
    </div>
  )
}
```

<div class="box rule mt-3" v-click="2">
<span class="box-label">Trois réflexes</span>

- Le `?` rend la prop **facultative**.
- Guillemets pour une string, accolades pour le reste : `titre="Hades"` mais `note={9.5}`.
- Un composant ne **modifie jamais** ses props.

</div>

</div>

</div>

<!--
La slide précédente montre comment TYPER les props. Celle-ci montre comment
les UTILISER — c'est le morceau qui leur manque toujours.

Les deux erreurs à guetter au TP :
- note="9.5" avec des guillemets : c'est alors une string, et les calculs cassent
- vouloir modifier une prop dans le composant (titre = titre.toUpperCase())

Si quelqu'un connaît Next et écrit `@/components/GameCard` : cet alias n'existe pas
dans un projet Vite par défaut, il faut le configurer dans vite.config.ts ET
tsconfig.json. En chemin relatif, ça marche tout de suite.

Question à poser : "et si je veux passer la recette entière plutôt que titre + note ?"
Réponse : une seule prop `game: Game`. C'est souvent mieux, et c'est ce qu'on fait
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
<GameCard key={game.slug} game={game} />
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
  games: Game[]
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
        <GameCard key={game.slug} game={game} />
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
<div class="tag tag-cyan mb-2">Exercice 1</div>

# Les données et la carte <span class="timer ml-3">⏱ 10 minutes</span>

</div>

<div class="grid grid-cols-2 gap-5 mt-4 text-sm">

<div>

1. Dans `types/game.ts`, le type `Game` : titre, studio, année, note
2. Dans `data/games.ts`, un tableau `games: Game[]`
3. Remplissez-le avec **les jeux ci-contre** : à vous de trouver le studio, l'année et de mettre votre note
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
Le travail reste le leur : studio, année, note.

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
.game-card__titre {
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
  <h2 className="game-card__titre">{game.titre}</h2>
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
    {game.titre}
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

<div class="tag tag-cyan mb-4">Exercice 2</div>

# Reproduisez cette liste

<div class="timer mt-2">⏱ 12 minutes</div>

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
    <article key={game.id}
      className="flex items-center gap-4 p-4 rounded-xl bg-slate-800
                 hover:bg-slate-700 transition-colors">
      <div className="grid place-items-center w-10 h-10 rounded-lg
                      bg-rose-500 font-bold text-slate-900">{game.note}</div>
      <div>
        <h2 className="font-semibold text-slate-50">{game.titre}</h2>
        <p className="text-sm text-slate-400">{game.studio} · {game.annee}</p>
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

<div class="tag tag-cyan mb-4">Exercice 3</div>

# Trois pages, trois routes

<div class="timer mt-2">⏱ 8 minutes</div>

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
Ne rien corriger au tableau avant 6 minutes.
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

# Le menu dupliqué

```tsx {all|3,6,9}
<Routes>
  <Route path="/" element={
    <><Menu /><HomePage /><Footer /></>
  } />
  <Route path="/tier-list" element={
    <><Menu /><TierListPage /><Footer /></>
  } />
  <Route path="/a-propos" element={
    <><Menu /><AboutPage /><Footer /></>
  } />
</Routes>
```

<div v-click class="mt-4 box bad">

Trois fois le même menu. À la quinzième route, vous ajoutez un lien et vous en oubliez deux.

Et pire : à chaque navigation le `Menu` est **démonté puis reconstruit**.

</div>

<div v-click class="mt-3 big">La solution : un composant qui contient la partie fixe, avec un trou au milieu.</div>

---

# Layout et Outlet

<div v-if="$clicks < 2">

```tsx {all|7}
import { Outlet } from 'react-router-dom'

const Layout = () => {
  return (
    <div className="min-h-screen bg-slate-900 text-slate-100">
      <Menu />
      <main className="p-6"><Outlet /></main>
      <Footer />
    </div>
  )
}
export default Layout
```

<div class="muted text-sm mt-3">La partie fixe, avec un trou au milieu. Reste à dire quelles pages vont dans ce trou.</div>

</div>

<div v-if="$clicks >= 2">

```tsx
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<HomePage />} />
    <Route path="tier-list" element={<TierListPage />} />
    <Route path="a-propos" element={<AboutPage />} />
  </Route>
</Routes>
```

<div class="muted text-sm mt-3">La route parente rend le <code>Layout</code>. Les trois enfants s'affichent dans son <code>Outlet</code>.</div>

</div>

<div class="box trap mt-3" v-click="3">
<span class="box-label">Deux pièges</span>

`index` = la page affichée quand on est **exactement** sur `/`.
Et les chemins enfants **n'ont pas** de slash devant : ils se collent au parent.

</div>

<!--
Faire le geste avec les mains : le Layout est un cadre, l'Outlet est le trou.
Montrer en live dans les devtools React que le Menu n'est pas remonté
quand on change de page.
-->

---

# Une route pour tous les jeux

```tsx
<Route path="jeux/hollow-knight" element={<GameDetailPage />} />
<Route path="jeux/hades" element={<GameDetailPage />} />
```

<div v-click="1" class="mt-3 box bad">Et pour 200 jeux ? Et quand vous en ajoutez un ?</div>

<div v-click="2" class="mt-5">

Les deux points déclarent un **paramètre** : un morceau d'URL qui varie.

```tsx
<Route path="jeux/:slug" element={<GameDetailPage />} />
```

</div>

<div class="flow mt-5" v-click="3">
  <div class="flow-node"><b>/jeux/hades</b>slug = "hades"</div>
  <div class="flow-node"><b>/jeux/celeste</b>slug = "celeste"</div>
  <div class="flow-node"><b>/jeux/nawak</b>slug = "nawak"</div>
</div>

<div class="mt-4 muted text-sm" v-click="4">

Une seule route, un seul composant, autant de pages que de jeux.

</div>

---

# Lire le paramètre : useParams

```tsx {all|5|7|9-11}
import { useParams } from 'react-router-dom'
import { games } from '../data/games'

const GameDetailPage = () => {
  const { slug } = useParams()

  const game = games.find((g) => g.slug === slug)

  if (!game) {
    return <p>Ce jeu n'existe pas.</p>
  }

  return <h1>{game.titre}</h1>
}
export default GameDetailPage
```

<div class="box trap mt-3" v-click>

`slug` est de type `string | undefined`, et `find()` renvoie `Game | undefined`.
D'où le `if (!game)` juste après : il règle les deux cas d'un coup.

</div>

---

# Naviguer depuis le code, et gérer le reste

<div class="grid grid-cols-2 gap-4">

<div>

<div class="tag tag-cyan mb-2">useNavigate</div>

```tsx
const navigate = useNavigate()

<button onClick={() => navigate(-1)}>
  ← Retour
</button>

// navigate('/tier-list')  aller à une page
// navigate(-1)            page précédente
```

<div class="box rule mt-3" v-click="1">
<span class="box-label">La règle</span>

L'utilisateur **clique pour aller** quelque part → `Link`.
Le code décide **après une action** → `useNavigate`.

</div>

</div>

<div>

<div v-click="2">

<div class="tag tag-amber mb-2">La route 404</div>

```tsx
<Route path="/" element={<Layout />}>
  <Route index element={<HomePage />} />
  <Route path="tier-list" element={<TierListPage />} />
  <Route path="jeux/:slug" element={<GameDetailPage />} />
  <Route path="*" element={<NotFoundPage />} />
</Route>
```

</div>

<div class="box bad mt-3" v-click="3">

`*` attrape tout le reste. Sans elle, une mauvaise URL affiche une page
**blanche** et vous cherchez un bug qui n'existe pas.

</div>

</div>

</div>

---
layout: center
class: text-center
---

<div class="tag tag-amber mb-4">Quiz · 30 secondes</div>

# Page blanche. Pourquoi ?

```tsx
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<HomePage />} />
    <Route path="/tier-list" element={<TierListPage />} />
  </Route>
</Routes>
```

<div v-click class="mt-6">

<div class="box info text-left">

Le **slash** devant `tier-list`. Dans une route imbriquée, le chemin se colle au parent :
ça donne `//tier-list`, que personne n'atteindra jamais.

Aucune erreur affichée. Aucun warning. Juste une page vide.
Retenez-le, vous le ferez au moins une fois.

</div>

</div>

---
layout: center
class: text-center
---

<div class="tag tag-cyan mb-4">Exercice 4</div>

# Le layout et la page détail

<div class="timer mt-2">⏱ 12 minutes</div>

<div class="mt-6 text-left mx-auto w-fit">

1. `components/Layout.tsx` : un `<nav>`, un `<Outlet />`, un `<footer>`
2. Vos trois liens en `NavLink`, avec un style Tailwind sur le lien actif
3. Passez vos routes en **routes imbriquées** sous le `Layout`
4. Ajoutez `jeux/:slug` → `GameDetailPage` avec `useParams`
5. Chaque `GameCard` devient un `Link` vers son détail
6. Une route `*` → `NotFoundPage`, et testez `/nawak`

</div>

<div class="mt-6 box good text-left">
<span class="box-label">Checkpoint</span>

Vous copiez l'URL `/jeux/hollow-knight`, vous la collez dans un nouvel onglet :
la bonne page s'affiche. Et le menu ne clignote jamais quand vous naviguez.

</div>

<!--
L'exercice le plus formateur de la séance. Circuler beaucoup.
Erreurs fréquentes :
- slash devant la route enfant → page blanche (ils viennent de voir le quiz,
  leur rappeler seulement s'ils sèchent plus de 2 minutes)
- oubli des backticks dans to={`/jeux/${game.slug}`}
- index oublié sur la route d'accueil
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

### Routage

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

<div class="muted -mt-3 mb-4 text-sm">2 heures · vous repartez de votre GameRank</div>

<div class="grid grid-cols-2 gap-5">

<div class="box">

### Le minimum

<div class="mt-3 text-sm">

**1 · La page détail, pour de vrai**<br/>
Elle existe déjà. Remplissez-la et soignez-la : studio, année, description, jaquette.

**2 · Une deuxième tier list : la difficulté**<br/>
Un nouveau champ dans le type `Game`, une nouvelle page.

**3 · Une troisième page, au choix**<br/>
`/compare/:a/:b` le duel · `/stats` les chiffres<br/>
`/tier/:tier` un écran par rang · `/aleatoire` la surprise

</div>

</div>

<div class="box">

### Les règles du jeu

<div class="mt-3 text-sm">

- Le style est **libre**. Soyez créatifs.
- Librairies autorisées, sauf les kits de composants tout faits.
- L'IA pour **le contenu**, jamais pour le code.
- Date de sortie et studio doivent être **exacts**. Vérifiez-les.
- Les trois règles du cours s'appliquent aussi ici : un composant une seule chose, des props typées, une `key` stable.

</div>

</div>

</div>

<!--
Insister sur deux choses au moment de lancer le TP :
- la page détail EXISTE déjà (exercice 4), le travail c'est le contenu et le soin
- le champ difficulte ajouté au type Game va faire hurler TypeScript sur les 20 jeux
  du tableau tant qu'ils ne l'ont pas rempli partout. C'est normal, c'est même le sujet.

La troisième page au choix : leur dire de ne PAS toutes prendre la même,
on regarde les rendus ensemble à la fin.
-->
