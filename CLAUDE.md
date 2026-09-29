@AGENTS.md
# Projet global-assistant-lab

## Objectif

Application Next.js servant de laboratoire d'apprentissage : tester des fonctionnalités web modernes et une intégration d'IA (Claude), en petites étapes.
Le profil utilisateur et les règles d'autonomie / actions critiques sont dans le CLAUDE.md global. Ce fichier ne les remplace pas.

## Stack (versions installées)

- Next.js 16 (App Router), React 19, TypeScript 5
- Tailwind CSS 4 (configuration dans `src/app/globals.css`, pas de `tailwind.config.js` par défaut)
- Framer Motion (animations)
- Vercel AI SDK : `ai` et `@ai-sdk/anthropic` (appels à Claude, côté serveur uniquement)
- ESLint 9

Avant d'utiliser une API de Next.js ou de React, vérifie qu'elle existe dans la version installée (`package.json`). Ne te fie pas à ta mémoire pour les versions récentes.

## Commandes

- Lancer en local : `npm run dev` (http://localhost:3000)
- Vérifier le code : `npm run lint`
- Build de production : `npm run build`
- À lancer uniquement depuis la racine du projet (là où se trouve `package.json`).

## Architecture

- `src/app` : routes, pages, layouts et route handlers (`route.ts`)
- `src/components` : composants réutilisables
- `src/lib` : logique réutilisable (helpers, client IA, config)
- `src/types` : types TypeScript partagés
- `public` : assets statiques

Règles de structure :
- Un composant par fichier, nom en PascalCase (`ChatBox.tsx`).
- Les fichiers de `src/lib` ont un nom en camelCase (`anthropic.ts`).
- Pas de logique métier dans les pages : la déplacer dans `src/lib`.
- Ne crée pas de dossier ou de couche d'abstraction sans besoin réel.

## Règles de code

- TypeScript strict : pas de `any`. Si un type est inconnu, utilise `unknown` et vérifie-le.
- Par défaut, les composants sont des Server Components. Ajoute `"use client"` seulement si nécessaire (état, événements, Framer Motion, API du navigateur), et le plus bas possible dans l'arbre de composants.
- Composants typés explicitement (props nommées dans un type).
- Imports avec l'alias du projet (`@/`) plutôt que des chemins relatifs profonds.
- Pas de code mort, pas de `console.log` oublié, pas de contenu de démonstration du template.

## Design

- Minimaliste, cohérent, sobre.
- Responsive : mobile d'abord, puis `sm:`, `md:`, `lg:`.
- Accessible : HTML sémantique, `alt` sur les images, contrastes suffisants, navigation clavier, focus visible, labels sur les champs de formulaire.
- Tailwind : classes utilitaires ; extraire un composant plutôt que copier les mêmes classes partout.
- Animations Framer Motion : courtes et discrètes. Respecter `prefers-reduced-motion` (`useReducedMotion`).

## Intégration IA (Claude)

- Les appels à l'API Anthropic se font uniquement côté serveur : route handler dans `src/app/api/.../route.ts` ou Server Action. Jamais depuis un composant client.
- La clé s'appelle `ANTHROPIC_API_KEY`, stockée dans `.env.local` (jamais commité). Ne jamais la préfixer par `NEXT_PUBLIC_` : ce préfixe l'expose au navigateur.
- Ne jamais afficher, journaliser ou renvoyer la clé au client.
- Le client (navigateur) appelle notre route interne, jamais l'API Anthropic directement.
- Valider et limiter la taille des entrées utilisateur avant de les envoyer au modèle.

## Dépendances

Avant d'ajouter une dépendance, explique :
1. son utilité concrète ;
2. son coût architectural (poids, maintenance, verrouillage à la bibliothèque) ;
3. son alternative native (Next.js, React, CSS, Web API) et pourquoi elle ne suffit pas.

Puis donne le nom exact et la commande `npm install`, et attends ma validation (action critique). Pas de doublon fonctionnel avec ce qui est déjà installé.

## Après chaque modification

1. Lance `npm run lint`.
2. Lance `npm run build`.
3. Corrige les erreurs toi-même, puis relance.
4. Fais le résumé : fichiers modifiés (chemin exact + rôle), changements, résultat de lint et build.

Indique toujours le chemin du fichier avant de proposer du code.

## Sécurité

- Aucun secret dans le code, dans Git, ni côté client.
- `.env*` doit rester dans `.gitignore` (vérifie-le à la création du projet).
- Ne lis pas et ne modifie pas `.env*` sans ma validation.
- Avant tout `git push`, vérifie qu'aucun secret n'est dans les fichiers suivis.