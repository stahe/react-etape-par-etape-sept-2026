# Introduction étape par étape au framework React

📖 **Lire le tutoriel : [https://stahe.github.io/react-etape-par-etape-sept-2026/](https://stahe.github.io/react-etape-par-etape-sept-2026/)**

Ce cours vous apprend à écrire une application web avec la bibliothèque [React](https://react.dev) 19.3 : une application **à page unique** (SPA), dont les pages sont fabriquées **dans le navigateur**, à partir des données JSON d'un serveur.

Il fait suite au cours [Introduction étape par étape au framework web NestJS](https://stahe.github.io/nestjs-html-sept-2026/), dont il change le point de vue, et reprend le plan des cours [Introduction étape par étape au framework Vue.js](https://stahe.github.io/vuejs-etape-par-etape-sept-2026/) et [Introduction étape par étape au framework Angular](https://stahe.github.io/angular-etape-par-etape-sept-2026/) : même serveur, mêmes pages, écrites à la manière de React.

| Cours NestJS | Cours React |
|---|---|
| le serveur fabrique les pages HTML (Handlebars) | le navigateur fabrique les pages (React) |
| le navigateur affiche ce qu'il reçoit | le serveur ne renvoie que du JSON |
| contrôleurs, vues, `res.render` | composants, routeur, hooks |
| gardes `JwtAuthGuard`, `RolesGuard` | gardes du routeur (middlewares de React Router) ; le serveur garde les siens |
| dictionnaires lus par le serveur | dictionnaires dans le client (i18next) ; le serveur ne renvoie que des clés |
| message flash dans un cookie | message flash dans un magasin Zustand |

Les pages, elles, ne changent pas : ce sont celles de l'application **RdvMedecins** déjà présentée avec les autres frameworks.

## L'approche : de nombreux petits exemples, puis une étude de cas

Le cours s'articule autour de **25 petits exemples**, chacun centré sur une notion. Ils forment un seul projet Vite : une seule commande `npm install`, puis `npm start <exemple>` pour en lancer un.

| Chapitre | Contenu | Exemples |
|---|---|---|
| Premiers pas | un projet Vite, les composants fonctions, le JSX, l'état (`useState`, `useReducer`), les événements, les champs contrôlés, la validation, React Hook Form, la mise en forme (`Intl`) | 01–09 |
| Les composants | props, fonctions en props, `children`, cycle de vie (`useEffect`, `useEffectEvent`), contextes, hooks personnalisés, fenêtre de confirmation (`createPortal`) | 10–16 |
| Le routage | React Router 8 : routes, paramètres, query, chargement différé, `<title>`, gardes (middlewares) | 17–18 |
| L'asynchrone et l'état partagé | minuteries, anti-rebond, réponses périmées, `use()` + `<Suspense>`, Zustand, `localStorage`, `<Activity>` | 19–20 |
| Internationalisation | i18next / react-i18next : paramètres, pluriels, dates, montants | 21 |
| Le serveur, une boîte noire | installation du serveur JSON, son API, 48 exemples `curl` | – |
| Dialoguer avec le serveur | `fetch`, proxy de Vite, cookie `httpOnly`, `useActionState`, couche d'accès à l'API, TanStack Query, erreurs du serveur attachées aux champs | 22–25 |

Chaque exemple est présenté avec son code complet, commenté ligne par ligne, et une copie d'écran de son exécution.

## Le serveur : une boîte noire

Le serveur est le serveur NestJS du cours précédent, dont les contrôleurs renvoient du **JSON** — le même que pour les clients Vue.js et Angular. Le cours le traite comme une **boîte noire** : on l'installe, on étudie son API, on l'interroge avec `curl` — mais on n'a pas besoin de lire son code (fourni et commenté pour les curieux).

- toutes les erreurs ont la même forme : `{ "statusCode": 409, "cle": "ERRORS.LOGIN_TAKEN", "params": {...}, "champs": {...} }` — des **clés** de traduction, jamais de texte ;
- authentification par jeton JWT dans un cookie `httpOnly` / `sameSite=strict` : le code JavaScript du client ne voit jamais le jeton ;
- protection CSRF : cookie `sameSite`, et tout POST doit être en JSON ;
- un « mode test » du captcha pour pouvoir interroger l'API avec `curl`.

## L'étude de cas : le client React de RdvMedecins

Une application complète de **prise de rendez-vous dans un cabinet médical**, dont **tous** les fichiers (une quarantaine) sont listés et commentés.

- **React moderne** : composants fonctions et hooks, actions de formulaire, `<title>` dans les composants, React Router 8 en mode « données » (middlewares, pages chargées à la demande), TanStack Query pour les données du serveur, Zustand pour l'état partagé.
- **Trois rôles** : `ADMIN` (gère les médecins et les clients), `DOCTOR` (prend et annule les rendez-vous), `USER` (le patient : réserve pour lui-même, gère son compte).
- **Confidentialité** : un patient ne reçoit jamais le nom des autres patients — le serveur ne l'envoie pas.
- **Tout l'état de la page dans l'URL** : `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` rouvre la fenêtre de réservation après un F5 ; les boutons Précédent / Suivant fonctionnent.
- **Validation par le serveur** : les formulaires affichent sous chaque champ les erreurs renvoyées par l'API ; verrou optimiste, homonymes, login déjà pris...
- **Session** : rétablie après un F5 (`GET /api/auth/moi`), expiration gérée en un seul endroit (la fonction commune à toutes les requêtes, réponse 401).
- **Français / anglais**, titres d'onglet traduits, fenêtre de confirmation accessible au clavier, Bootstrap 5.
- **Déploiement** : le client compilé est servi par le serveur JSON lui-même (même origine, pas de CORS).

## Le contenu du dépôt

```
exemples-react/            les 25 petits exemples (un seul projet Vite)
rdvmedecins-nestjs-json/   le serveur JSON RdvMedecins (la « boîte noire »)
rdvmedecins-react/         le client React de l'étude de cas
```

## Technologies

React 19.3 · TypeScript 6.0 · Vite 8 · React Router 8.4 · TanStack Query 5 · Zustand 5 · i18next 26 / react-i18next 17 · React Hook Form 7 (exemple 08) · Bootstrap 5.3 · côté serveur : NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Prérequis

- Des bases de JavaScript (ou TypeScript), de HTML et du protocole HTTP.
- Node.js 24 (ou 22.22 au moins), Visual Studio Code, l'extension de navigateur React Developer Tools, un serveur MySQL (par exemple Laragon sous Windows) pour le serveur JSON. Les instructions d'installation sont données dans les annexes du cours.

## Auteur

Ce cours, ses exemples, le serveur JSON et l'étude de cas ont été rédigés par **Claude**, l'IA d'[Anthropic](https://www.anthropic.com) (septembre 2026), à la demande de Serge Tahé.
