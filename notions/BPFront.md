# Les bonnes pratiques en développement Frontend

## Pourquoi ?

Pour l'amélioration de la **lisibilité**, la **maintenabilité**, la **performance** et l’**expérience utilisateur**

---

## 🧱 **Structure et Organisation**
1. **Structure claire des dossiers** : séparer les composants, pages, styles, assets, services, etc.
2. **Nommer les fichiers de manière explicite** : ex. `Navbar.tsx` plutôt que `component1.tsx`.

---

## 💅 **HTML / Sémantique**
1. Utiliser des **balises sémantiques** (`<header>`, `<main>`, `<article>`, etc.).
2. Bien structurer les titres (`<h1>` à `<h6>`) pour l’accessibilité et le SEO.
3. Utiliser `alt` sur les images pour l’accessibilité.

---

## 🎨 **CSS / Style**
1. **Favoriser les classes utilitaires** (ex : TailwindCSS) ou CSS Modules pour éviter les conflits de styles.
2. Utiliser des **variables CSS** (ou SCSS) pour les couleurs, tailles, etc.
3. Garder le **style séparé de la logique** (pas de styles inline sauf cas particuliers).
4. Supprimer les styles inutilisés pour éviter les fichiers trop volumineux.

---

## 🧠 **JavaScript / TypeScript**
1. **Écrire du code lisible** et **modulaire** (fonctions pures, composants réutilisables).
2. Préférer **TypeScript** pour la robustesse et l’auto-complétion.
3. Ne pas répéter le code (principe DRY : *Don’t Repeat Yourself*).
4. Utiliser `async/await` plutôt que les `.then()` enchaînés pour une meilleure lisibilité.

---

## ⚛️ **Frameworks (React, Vue, etc.)**
1. **Découper les composants** (pas de fichiers de 500 lignes !).
2. **Utiliser des hooks personnalisés** (React) pour réutiliser la logique.
3. Nommer les composants de manière explicite (`UserCard` plutôt que `Component1`).
4. Utiliser des **props typées** (PropTypes ou interfaces avec TypeScript).

---

## ⚡ **Performance**
1. **Lazy loading** des composants/pages non critiques.
2. Utiliser la **mise en cache** pour les appels API.
3. Optimiser les **images** (taille, format WebP).
4. Minimiser les **re-rendus inutiles** (mémoïsation, `useMemo`, `React.memo`...).

---

## 🧪 **Tests et Qualité**
1. Ajouter des **tests unitaires** (ex: Jest) et des tests d’intégration (ex: Testing Library).
2. Utiliser des **linters** (ESLint) et des **formatters** (Prettier).
3. Mettre en place l’intégration continue (CI) pour automatiser les vérifications.

---

## 🌐 **Accessibilité & SEO**
1. Respecter les **contrastes** pour les textes et boutons.
2. Utiliser les **rôles ARIA** si nécessaire.
3. Ajouter des **métadonnées** (`title`, `meta description`, OpenGraph...).

---

## 🚀 **Bonnes pratiques UX/UI**
1. Réactivité : site **mobile-first** (responsive design).
2. Chargement rapide : feedbacks visuels pendant les chargements (`Skeleton`, `Spinners`...).
3. Éviter les popups ou alertes bloquantes sans contexte.

---

Le **design** en frontend, c’est un peu la touche magique qui transforme une app fonctionnelle en une expérience agréable et mémorable. 
Pas besoin d’être designer pro, mais **comprendre quelques bases** change tout 🔥

---

## 🎨 **Les fondamentaux du design en frontend**

### 1. **Hiérarchie visuelle**
- Utilise la **taille**, la **couleur**, et le **contraste** pour guider l’œil de l’utilisateur.
- Un bouton important ? Il doit *ressortir* : plus grand, plus coloré.
- Évite d’avoir 10 tailles de texte différentes — reste cohérent.

### 2. **Espacement et alignement**
- Le **white space** (espaces vides) est ton ami : il permet de respirer.
- Utilise une **grille** (ex. CSS Grid ou Flexbox) pour structurer ton contenu proprement.
- Garde une **marge/padding cohérente** entre les éléments (souvent 4, 8, 16, 32px...).

### 3. **Typographie**
- Une seule police principale (deux max).
- Utilise la **hiérarchie** typographique : `h1`, `h2`, `p` bien définis.
- Attention à la **lisibilité** : taille minimale de 14-16px pour du texte standard.

### 4. **Couleurs**
- Aie une **palette de couleurs cohérente** : 1 couleur principale, 1 secondaire, des tons neutres.
- Respecte les contrastes : texte noir sur fond gris clair > texte gris sur fond gris.
- Utilise des outils comme [Coolors](https://coolors.co) pour générer des palettes harmonieuses.

### 5. **Consistance**
- Les boutons doivent avoir le **même style partout**.
- Même logique pour les inputs, les icônes, les marges, etc.
- Un design cohérent = expérience utilisateur fluide.

---

## 🧩 Bonus : UI kits & Design Systems

Tu peux gagner un temps fou avec des **UI kits** ou **Design Systems** :
- [Tailwind UI](https://tailwindui.com/) + [Headless UI](https://headlessui.com/)
- [ShadCN](https://ui.shadcn.com/) (si tu utilises React + Tailwind)
- [Material UI](https://mui.com/) (Google)
- [Radix UI](https://www.radix-ui.com/) (composants accessibles et stylables)

Ils offrent des composants déjà pensés pour l’accessibilité et le responsive.

---

## ✨ En résumé

> Un bon frontend, c’est **50% technique, 50% design**.

Même avec un petit projet perso, prendre un peu de temps pour peaufiner le design fait toute la différence. 
Tu donnes envie à l’utilisateur de rester, de cliquer, de revenir.

---
