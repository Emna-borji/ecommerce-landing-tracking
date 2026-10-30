# ecommerce-landing-tracking

Landing e-commerce multi-produits avec **instrumentation Google Analytics 4** du parcours d'achat
(view / add_to_cart / add_to_favorites / purchase_click / scroll / back_home).

## Objectif
Démontrer la mise en place d'un **tracking GA4** complet sur un parcours e-commerce fictif :
page d'accueil → fiche produit → actions utilisateur (panier, favoris, achat), afin d'analyser
le comportement utilisateur dans l'interface Google Analytics.

## Données
Pas de base de données. Les événements sont envoyés à **Google Analytics 4** via `gtag.js`
(propriété `G-3ZZZ2244QL`), consultables dans l'interface GA du propriétaire du tag.

3 produits fictifs présentés sur la landing et chacun sur sa page détail :
- Montre Connectée ActiveX (199 €)
- Écouteurs Sans Fil Pro
- Smartphone Ultra 12

## Méthode
Code HTML/CSS/JS statique :
- `index.html` : landing (masthead, 3 features, 3 showcases cliquables vers les fiches produit,
  testimonials, formulaire d'inscription, footer).
- `produit1.html`, `produit2.html`, `produit3.html` : fiches produit avec 3 boutons d'action
  (`Ajouter au panier`, `Acheter maintenant`, `Ajouter aux favoris`) qui déclenchent chacun un
  événement GA4 custom via `onclick="gtag('event', ...)"`.

Événements GA4 trackés :

| Événement | Déclencheur | Paramètres |
|---|---|---|
| `view_product` | Chargement d'une page produit | `{produit}` |
| `add_to_cart` | Clic "Ajouter au panier" | `{produit}` |
| `purchase_click` | Clic "Acheter maintenant" | `{produit}` |
| `add_to_favorites` | Clic "Ajouter aux favoris" | `{produit}` |
| `back_home` | Clic "← Retour accueil" | — |
| `scroll_page` | Scroll vertical > 500 px | — |

> **Important** : les boutons panier/favoris sont **des points de mesure**, pas des
> fonctionnalités e-commerce : aucune persistance (ni localStorage, ni backend) n'est faite,
> l'ajout au panier déclenche l'événement GA et c'est tout.

## Stack
- HTML5, CSS3, Bootstrap 5.2.3 (via CDN)
- Bootstrap Icons 1.5.0, Google Fonts (Lato) — CDN
- **Google Analytics 4** (`gtag.js`)
- Chaîne de build Pug/Sass fournie par le template (optionnelle) : Pug 3, Sass 1.60,
  browser-sync, autoprefixer, postcss

Base graphique : [Start Bootstrap - Landing Page v6.0.6](https://startbootstrap.com/theme/landing-page/)
par David Miller / Start Bootstrap LLC (licence MIT), personnalisée (branding, i18n FR, pages
produit, instrumentation GA).

## Résultats
Projet pédagogique : captures d'écran de l'interface GA4 montrent les événements custom et
les rapports par produit. Les parcours sont mesurés mais l'app n'est pas un vrai e-commerce
(voir note ci-dessus).

## Comment lancer
Fichiers HTML statiques, aucune build requise pour prévisualiser :

```bash
# Ouvre simplement index.html dans un navigateur
# ou lance un serveur local pour éviter les restrictions file://

python -m http.server 8080
# puis http://localhost:8080
```

Pour utiliser la chaîne de build Pug/Sass du template :

```bash
npm install
npm start           # build + live preview
npm run build       # build dans dist/
```

### Changer le tag GA
Le tag ID `G-3ZZZ2244QL` est hardcodé dans :
- `index.html` (ligne du script gtag)
- `produit1.html`, `produit2.html`, `produit3.html` (identiques)
- `dist/index.html`

Remplace la valeur par ton propre tag GA4 avant de forker.
