# Spottr — Production (web)

Production de l'app **Spottr**, application de suivi musculation. Cette version est **promue manuellement** depuis la sandbox [`musculation-app-dev`](https://github.com/Striinox/musculation-app-dev) après validation visuelle (copie de `index.html`).

L'app est une PWA `index.html` unique en HTML/CSS/JavaScript pur, aussi consommée par le projet [`spottr-mobile`](https://github.com/Striinox/spottr-mobile) (Capacitor) qui la wrappe dans un shell natif Android/iOS.

> ⚠️ **Ne pas pousser directement sur ce repo**. Tout passe par la sandbox d'abord. La prod doit rester stable car elle est utilisée en salle de musculation par les utilisateurs réels.

---

## Environnements

| Environnement | URL | Repo | Rôle |
|---|---|---|---|
| **Prod web** (ce repo) | striinox.github.io/musculation-app | `musculation-app` | App utilisée en salle |
| Sandbox web | striinox.github.io/musculation-app-dev | `musculation-app-dev` | Source de vérité, toutes les modifs y passent d'abord |
| Mobile (Android/iOS) | (en cours, futur Play Store + App Store) | `spottr-mobile` | Wrapper Capacitor de la PWA |

---

## Fonctionnalités

### Programme personnalisé
- Onboarding (objectif, niveau, jours, équipement, contraintes) avec algo de génération adapté
- Programmes supportés : Push/Pull/Legs, Upper/Lower, Full Body, Poids du corps, Haut/Bas uniquement
- Variété entre jours (Jour A ≠ Jour B), aucun doublon de mouvement par séance
- Multi-programmes avec switcher actif
- Remplacement d'exercices à la volée depuis la Bibliothèque
- **Partage de programme par code court (7 caractères)** — un ami saisit le code, le programme est dupliqué dans son compte (snapshot indépendant)

### Suivi des séances
- Saisie poids et reps par série, sauvegarde Supabase
- Pré-remplissage en bleu avec les valeurs de la dernière séance
- **Suggestions de surcharge progressive** par exercice : ↗ cible, +rep, stagnate, plateau, etc., avec couleurs et bloc tinté CHARGE
- **Détection de plateau + suggestion de deload** (3 séances de suite sans progrès)
- Adaptation des incréments au programme actif (mass/cut/fitness × beginner/intermediate/advanced)
- Logs isolés par programme

### Mes Records
- Sous-écran dédié accessible depuis Stats : tous tes PRs par exercice
- Photo + nom + 1RM Epley + meilleure série + date + catégorie
- Filtres catégorie, tri par 1RM ou date récente
- Card cliquable : breakdown PR par tranche de reps (1-3, 4-6, 7-10, 11-15, 16+)

### Semaine
- Refonte CHARGE : layout vertical
- **Volume strip hebdo** + comparaison « ↑ +X kg vs S-1 »
- Pool sticky bottom drag-and-drop (long-press sur mobile)

### Bibliothèque
- 67 exercices catalogués avec illustrations DALL-E
- Cards single-column avec photo 130 px, filtres hybrides catégorie + niveau
- Overlay détail plein écran avec pills muscles, cues, specs, CTA d'ajout à une séance

### Dashboard
- Stats 2×2, graphiques area VOLT
- Bouton « 🏆 Voir tous mes records »

### Timer de repos
- Anneau circulaire CHARGE, auto-démarrage, wake lock
- Choix temps : 45s / 1min / 1m30 / 2min / 3min, mémorisé par exercice

---

## Stack technique

| Composant | Technologie |
|---|---|
| Frontend | HTML / CSS / JavaScript pur (`index.html` unique) |
| Hébergement | GitHub Pages |
| Backend | Supabase (PostgreSQL + Auth + Storage) |
| Authentification | Supabase Auth (email / mot de passe) |
| Illustrations | DALL-E + Supabase Storage |

Identité visuelle : design system **CHARGE v5** (dark mode VOLT jaune-vert, typographies Archivo Black / Archivo / JetBrains Mono).

---

## Workflow de promotion

```
1. Modifs faites et validées sur la sandbox musculation-app-dev
2. cp musculation-app-dev/index.html musculation-app/index.html
3. git commit + git push (ce repo)
4. ~1 minute plus tard, GitHub Pages republie automatiquement
```

Pour la version mobile, l'`index.html` est ensuite copié dans `spottr-mobile/www/` puis `npx cap sync` propage aux assets natifs Android/iOS.

---

## Architecture Supabase

Pour le détail complet (tables, RPC, RLS, algorithmes, table d'incréments de surcharge progressive, détection de plateau), voir le [README de la sandbox](https://github.com/Striinox/musculation-app-dev).

---

## Installation sur iPhone (PWA)

1. Ouvrir l'URL dans **Safari** sur iPhone
2. Bouton **Partager** → **« Sur l'écran d'accueil »**
3. L'app s'installe comme une application native

À terme, la version mobile native sera publiée sur l'App Store et le Play Store via le projet [`spottr-mobile`](https://github.com/Striinox/spottr-mobile).

---

## Roadmap

Voir la roadmap consolidée dans le [README de la sandbox](https://github.com/Striinox/musculation-app-dev). En résumé :
- **En cours** : transition app mobile native (Capacitor) + polish mobile
- **Planifié court terme** : tooltips débutants, analytics, stats par muscle, refonte CHARGE History/Replace modal
- **Reporté à la phase mobile native** : mode hors-ligne, push notifications, Apple Watch
- **Futur** : freemium (Stripe/RevenueCat), vidéos d'exécution, export PDF, coach mode

---

## Historique des versions

Voir le [historique complet dans la sandbox](https://github.com/Striinox/musculation-app-dev). Dernière version :

### v6 — Mai 2026 (rebrand Spottr + features avancées + transition mobile)
- Rebrand complet : Muscu/CHARGE → Spottr
- Suggestions de surcharge progressive adaptatives au programme
- Détection de plateau + deload
- Écran « Mes Records » dédié avec PR par tranche de reps
- Comparaison de volume vs semaine précédente
- Partage de programme par code court à 7 caractères
- Mise en évidence visuelle des suggestions (bordure gauche + pill + bloc tinté)
- Migration `programs.objective`/`level` + backfill
- Init du projet `spottr-mobile` (Capacitor + Android)
