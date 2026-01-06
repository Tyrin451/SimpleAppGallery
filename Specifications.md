# Specifications

Voici les spécifications techniques détaillées destinées à un agent de codage (LLM ou développeur) pour la génération de l'application "PWA Media Gallery".

## Document de Spécifications Techniques

**Projet :** PWA Media Gallery
**Stack Technique :** HTML5, CSS3 (Flexbox/Grid), Vanilla JavaScript (ES6+). Aucune dépendance externe.
**Persistance :** `localStorage` du navigateur.


### 1. Architecture des Données

* **Stockage :** Utilisation du `localStorage`.
* **Clé :** `media_gallery_source`
* **Format :** Chaîne de caractères (String) contenant les URLs séparées par des sauts de ligne (`n`).
* **État d'exécution (Runtime State) :**
* `currentIndex` (Integer) : Index du média actuellement affiché.
* `mediaList` (Array of Strings) : Liste parsée des URLs valides.

### 2. Interface Utilisateur (UI)

L'application est une Single Page Application (SPA) composée de deux conteneurs principaux basculant via la propriété CSS `display`.

#### A. Global Layout

* **Viewport :** `width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no`.
* **Thème :** Fond noir (`#000`), texte blanc.
* **Navigation (Navbar) :**
* Position : `fixed bottom`.
* Hauteur : 60px.
* Contenu : 2 Boutons (Icônes ou Texte) : "Configuration" et "Galerie".
* Comportement : Z-index élevé. Doit masquer/afficher les conteneurs `view-config` et `view-gallery`.

#### B. Vue Configuration (`#view-config`)

* **Composants :**
* `<textarea>` : Pleine largeur, hauteur 80vh. Placeholder : "Coller une URL par ligne...".
* `<button id="save-btn">` : Bouton "Charger / Sauvegarder".

* **Logique :** Au chargement, le `textarea` est pré-rempli avec le contenu du `localStorage`.

#### C. Vue Galerie (`#view-gallery`)

* **Style :** Fullscreen absolu. Flexbox centré.
* **Conteneur Média :**
* Doit occuper 100% largeur/hauteur (`object-fit: contain`).
* Fond noir.

* **Contrôles Overlay (Optionnel) :** Zones cliquables gauche/droite invisibles pour la navigation (desktop) ou gestion des gestes (mobile).

### 3. Logique Fonctionnelle (Business Logic)

#### A. Parsing et Validation

1. Récupération du texte brut depuis le `textarea`.
2. Split par le caractère de saut de ligne `\n`.
3. Trim des espaces blancs.
4. Filtrage des entrées vides.

#### B. Moteur de Rendu (Render Engine)

L'agent doit implémenter une fonction `renderMedia(url)` qui analyse l'extension du fichier ou le contenu de l'URL pour décider du tag HTML :

* **Vidéos** (Extensions: `.mp4`, `.webm`, `.ogg`) :
* Rendu via `<video controls autoplay loop playsinline>`.
* Attribut `muted` par défaut (requis par Chrome/Android pour l'autoplay).
* **Images** (Autres extensions) :
* Rendu via `<img>`.

* **Gestion d'erreur :** Si le média échoue à charger (`onerror`), afficher un placeholder "Média introuvable".

#### C. Navigation Galerie

* **Actions :** `Next()` et `Previous()`.
* **Boucle :** La navigation doit être circulaire (Next sur le dernier élément -> Index 0).
* **Inputs :**
* *Click/Tap :* Zone gauche (Précédent), Zone droite (Suivant).
* *Keyboard :* Flèche Gauche / Flèche Droite.
* *Swipe (Touch) :* Détection basique du `touchstart` et `touchend` pour glissement horizontal.

#### D. PWA Manifest (Spécifique Android)

* `display`: `"standalone"` (Cache la barre d'URL du navigateur).
* `orientation`: `"any"` ou `"portrait"`.
* `background_color`: `"#000000"`.


### 4. Structure de Fichiers Requise

```text
/
├── index.html       # Contient tout le HTML, CSS (in <style>) et JS (in <script>)
├── manifest.json    # Déclaration PWA
└── sw.js            # Service Worker minimal (Cache-First strategy)
```

### 5. Algorithme de Changement de Vue

Lors du clic sur le bouton "Galerie" dans la Navbar :

1. Sauvegarder le contenu du `textarea` dans `localStorage`.
2. Parser la liste `mediaList`.
3. Si la liste est vide, alerte "Aucune URL".
4. Sinon, masquer `#view-config`, afficher `#view-gallery`.
5. Charger le média à l'index `currentIndex` (défaut 0).

### Further Thinking

Pour garantir une expérience fluide (60fps) sur mobile, spécifiez à l'agent d'ajouter une logique de **"Preloading"**. L'image à `index + 1` doit être chargée en mémoire (via un objet `new Image()`) dès que l'image `index` est affichée. Sans cela, l'utilisateur verra un écran noir entre chaque slide en cas de réseau lent. De plus, pour les vidéos, il faut gérer le cycle de vie : `pause()` la vidéo sortante avant de charger la suivante pour économiser la RAM et la batterie.

