# Next.js → React + React Router v6

Mémo de correspondance. Pour toi, pas pour la classe.

Tout ce que Next fait automatiquement, ici il faut le déclarer à la main.
C'est la seule idée à retenir : **React Router ne devine rien**.

---

## Déclarer une route

| Next.js (App Router) | React + React Router v6 |
|---|---|
| Créer `app/tier-list/page.tsx` **suffit** | Créer le fichier **ne suffit pas** |
| La route existe dès que le fichier existe | Il faut en plus ajouter `<Route path="tier-list" element={<TierListPage />} />` |
| Arborescence = arborescence des URL | La table des routes est un objet à part, dans `App.tsx` |

```tsx
// App.tsx — il n'y a pas d'équivalent Next, c'est ce fichier qui remplace
// toute la convention de dossiers de Next
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<HomePage />} />
    <Route path="tier-list" element={<TierListPage />} />
    <Route path="jeux/:slug" element={<GameDetailPage />} />
    <Route path="*" element={<NotFoundPage />} />
  </Route>
</Routes>
```

**Le piège que tes élèves vont tous faire :** créer `pages/AboutPage.tsx` et
s'étonner que `/a-propos` renvoie la 404. En Next ça aurait marché.

---

## Le tableau de correspondance

| Besoin | Next.js | React Router v6 |
|---|---|---|
| Segment dynamique | dossier `[slug]` | `path="jeux/:slug"` |
| Attrape-tout | dossier `[...slug]` | `path="*"` |
| Layout partagé | `app/layout.tsx`, imbriqué automatiquement | un composant `Layout` + `<Outlet />`, imbriqué **à la main** |
| Page d'index d'un dossier | `page.tsx` | `<Route index element={...} />` |
| Lien | `<Link href="/x">` de `next/link` | `<Link to="/x">` de `react-router-dom` |
| Lien actif | `usePathname()` + comparaison manuelle | `<NavLink>` + `isActive`, **fourni** |
| Navigation par code | `useRouter().push('/x')` | `useNavigate()` puis `navigate('/x')` |
| Retour arrière | `router.back()` | `navigate(-1)` |
| Redirection | `redirect('/x')` | `<Navigate to="/x" replace />` |
| Lire un paramètre | `params` (Promise côté serveur) ou `useParams()` | `useParams()`, synchrone |
| Query string | `useSearchParams()` **en lecture seule** | `const [params, setParams] = useSearchParams()` — **avec** le setter |
| Page 404 | `app/not-found.tsx` | `<Route path="*" element={<NotFoundPage />} />` |
| État de chargement | `loading.tsx` | rien de fourni, tu le codes |
| Erreur | `error.tsx` | rien de fourni, tu le codes |
| Racine du document | `app/layout.tsx` avec `<html>` | `index.html` à la racine du projet |

---

## Ce qui n'existe pas du tout

- **Les Server Components.** Pas de `'use client'`, pas de `async function Page()`.
  Tout est client, tout est interactif, tout est dans le bundle.
- **Le data fetching dans un composant.** Pas de `await fetch()` dans une page.
  Ce sera `useEffect` + `useState`, ou une lib comme TanStack Query.
- **Le prefetch au survol.** Next précharge les routes tout seul. React Router non.
- **`next/image`, `next/font`, `metadata`.** Rien de tout ça.
  Pour le SEO : une SPA sert un HTML vide, point.
- **Les route groups** `(marketing)`. La table de routes joue ce rôle, en plus explicite.

---

## Les cinq pièges que Next te masquait

**1. Le `BrowserRouter` est à brancher soi-même**

En Next il n'y a rien à faire. Ici, si on l'oublie :

```
useRoutes() may be used only in the context of a <Router> component
```

Il va dans `main.tsx`, **autour** de `<App />`. Pas dedans.

**2. `element={<X />}` et pas `element={X}`**

`element` attend du TSX, pas une référence de fonction. L'erreur numéro un en cours.
Elle ne produit pas toujours un message clair.

**3. Le slash devant une route enfant**

```tsx
<Route path="/" element={<Layout />}>
  <Route path="/tier-list" element={<TierListPage />} />   {/* ← faux */}
</Route>
```

Les chemins enfants se **concatènent** au parent : ça donne `//tier-list`.
Aucune erreur, aucun warning, page blanche. C'est le quiz de la slide 34.

**4. `useParams()` renvoie `string | undefined`**

Même avec un générique. React Router ne peut pas garantir que l'URL
correspondait à cette route. Donc en TS strict, on est obligé de gérer le cas
manquant — ce qui est une bonne chose, mais ça les surprend.

**5. Le déploiement d'une SPA casse les liens profonds**

`/jeux/hades` tapé directement dans la barre d'adresse renvoie une 404 **serveur**,
parce qu'aucun fichier ne s'appelle comme ça. Il faut une règle de rewrite
vers `index.html` (`vercel.json`, `_redirects` sur Netlify, `try_files` sur nginx).
Next gère ça tout seul. Ça n'est pas dans le cours, mais si un élève déploie, il tombera dessus.

---

## Versions

- Le cours utilise **React Router v6**, API déclarative (`BrowserRouter` / `Routes` / `Route`).
- **v7 existe** et cette API y est identique — rien à changer si `npm install` te donne une v7.
  Pour rester strictement en v6 : `npm install react-router-dom@6`.
- React Router propose aussi une API « data router » (`createBrowserRouter`, avec `loader`
  et `errorElement`), et un mode framework qui ressemble beaucoup à ce que tu connais de Next.
  **Hors sujet pour ce cours** : ça mélange routage et chargement de données, et les élèves
  ont besoin de séparer les deux d'abord.

---

## Si tu veux t'entraîner avant

Le plus rentable, dans cet ordre :

1. `npm create vite@latest test -- --template react-ts`, puis brancher le `BrowserRouter`
   à la main. Le simple fait de le faire une fois te vaccine.
2. Écrire un `Layout` + `<Outlet />` et sentir la différence avec `app/layout.tsx` :
   ici c'est **toi** qui décides quelles routes sont dans quel layout.
3. Faire une page `/jeux/:slug` avec `useParams` + un `find` sur un tableau local,
   et gérer le cas introuvable.

C'est exactement les exercices 3 et 4 du cours. En les faisant toi-même une fois,
tu auras déjà rencontré les erreurs que tes élèves vont produire.
