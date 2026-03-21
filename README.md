# 💪 Muscu — Application de Suivi Musculation

Application web progressive (PWA) de suivi d'entraînement musculation, optimisée pour iPhone. Installable sur l'écran d'accueil via Safari.

---

## Fonctionnalités

### Programme personnalisé
- Questionnaire d'onboarding en 5 étapes (objectif, niveau, jours, matériel, contraintes)
- Algorithme de recommandation qui génère automatiquement le bon programme selon le profil
- Programmes supportés : Push/Pull/Legs, Upper/Lower, Full Body, Poids du corps, Haut uniquement, Bas uniquement
- Gestion de plusieurs programmes avec possibilité de switcher
- Remplacement d'exercices dans le programme depuis la bibliothèque

### Suivi des séances
- Saisie des poids et répétitions par série
- Sélection de la date de séance
- Sauvegarde automatique dans Supabase (cloud)
- Historique par exercice avec détection automatique des PRs (1RM estimé)

### Bibliothèque d'exercices
- 67 exercices catalogués avec schémas SVG
- Filtres par catégorie (Push, Pull, Legs, Core) et niveau (Débutant, Intermédiaire, Avancé)
- Recherche par nom ou muscle ciblé
- Détails : muscle principal, muscles secondaires, description technique, variantes, matériel requis

### Dashboard de progression
- Statistiques globales : séances, séries, volume total soulevé, exercices suivis
- Graphiques de progression par exercice (poids max dans le temps)
- Détection et affichage des records personnels (PR)
- Filtres par jour d'entraînement

---

## Stack technique

| Composant | Technologie |
|-----------|-------------|
| Frontend | HTML / CSS / JavaScript pur (fichier unique) |
| Hébergement | GitHub Pages |
| Base de données | Supabase (PostgreSQL) |
| Authentification | Supabase Auth (email / mot de passe) |
| Schémas exercices | SVG inline |

---

## Architecture Supabase

### Tables

**`user_profiles`** — Profil et préférences de l'utilisateur
```
user_id, objective, level, days_per_week, equipment, constraints
```

**`programs`** — Programmes d'entraînement
```
user_id, name, is_active, structure (JSONB)
```

**`exercises_library`** — Bibliothèque des exercices (lecture publique)
```
id, name, muscle_primary, muscles_secondary, category, equipment, level, reps_recommended, description, tips, variants
```

**`workout_logs`** — Logs des séances
```
user_id, exercise_id, exercise_name, day_key, session_date, set_number, weight_kg, reps
```

**`sessions`** — Sessions d'entraînement
```
user_id, day_key, session_date
```

### Sécurité
- Row Level Security (RLS) activé sur toutes les tables
- Chaque utilisateur ne voit que ses propres données
- La table `exercises_library` est en lecture publique

---

## Installation sur iPhone (PWA)

1. Ouvrir l'URL dans **Safari** sur iPhone
2. Appuyer sur le bouton **Partager** (carré avec flèche)
3. Sélectionner **"Sur l'écran d'accueil"**
4. L'app s'installe comme une application native

---

## Développement

### Structure du projet
```
musculation-app/
└── index.html    # Application complète (HTML + CSS + JS)
```

### Algorithme de recommandation

L'algorithme sélectionne le template de programme selon ces règles :

| Condition | Programme généré |
|-----------|-----------------|
| Poids du corps | Full Body 3j max |
| Contrainte haut uniquement | Push/Pull alternés |
| Contrainte bas uniquement | Legs A/B/C |
| ≤ 3 jours ou Débutant | Full Body |
| 4 jours | Upper / Lower |
| 5-6 jours | Push / Pull / Legs |

---

## Roadmap

### En cours
- Timer de repos entre les séries
- Pré-remplissage avec les valeurs de la dernière séance

### Planifié
- Notifications de rappel d'entraînement
- Export PDF de la progression
- Accès coach (lecture seule)
- Mode hors-ligne (Service Worker)

### Futur
- Système de paiement (Stripe)
- Publication App Store
- Domaine personnalisé

---

## Historique des versions

### v3 — Mars 2026
- Correction bug Safari iOS (session persistante, chargement au retour)
- Refactoring complet du code JavaScript

### v2 — Mars 2026
- Questionnaire d'onboarding
- Algorithme de recommandation de programme
- Bibliothèque de 67 exercices
- Gestion multi-programmes
- Remplacement d'exercices
- Migration utilisateurs existants

### v1 — Mars 2026
- Programme Push/Pull/Legs 5 jours codé en dur
- Système de logging avec sauvegarde Supabase
- Dashboard de progression
- Schémas SVG des exercices
- Déploiement GitHub Pages + PWA iPhone
